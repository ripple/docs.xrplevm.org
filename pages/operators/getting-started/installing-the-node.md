# Installing the Node

This guide focuses on the fastest and easiest operator path: install a network-compatible `exrpd` release, then bootstrap from a snapshot.

It covers **Mainnet**, **Testnet**, and **Devnet**, and includes the following run stacks:

- `systemctl`
- `docker`
- `cosmovisor`
- installation with raw binaries
- installation from source

If you specifically need full historical replay from block 0, use [Sync from Genesis](./sync-from-genesis.md).

## Network reference

| Network | Chain ID            | Current Version | Genesis                                                                                            |
| ------- | ------------------- | --------------- | -------------------------------------------------------------------------------------------------- |
| Mainnet | `xrplevm_1440000-1` | `v10.1.0`       | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json) |
| Testnet | `xrplevm_1449000-1` | `v10.1.0`       | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json) |
| Devnet  | `xrplevm_1449900-1` | `v11.0.0-rc.1`  | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/devnet/genesis.json)  |

## Recommended flow (snapshot-first)

1. Install `exrpd` (raw binary or from source).
2. Initialize and configure your node for the target network.
3. Restore snapshot data (when available).
4. Run the node with `systemctl`, `cosmovisor`, or `docker`.

{% admonition type="info" name="Snapshot availability" %}
Public snapshot providers are currently listed for Mainnet and Testnet in [Snapshots](../resources/snapshots.md). If a Devnet snapshot is not published, use [State Sync](../advanced/sync-options.md#state-sync) or [Sync from Genesis](./sync-from-genesis.md).
{% /admonition %}

## 1) Install `exrpd`

### Prerequisites

Install required tools first:

```bash
sudo apt-get update
sudo apt-get install -y curl wget jq lz4 build-essential git rsync
```

### Method A: Raw binary (recommended for most operators)

```bash
cd /tmp
TARGET_TAG=<target-tag>
wget "https://github.com/xrplevm/node/releases/download/${TARGET_TAG}/node_${TARGET_TAG#v}_Linux_amd64.tar.gz"
tar -xzf "node_${TARGET_TAG#v}_Linux_amd64.tar.gz"
sudo mv bin/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

Set `<target-tag>` from [Networks](../resources/networks.md) for your network:

- Mainnet: `v10.1.0`
- Testnet: `v10.1.0`
- Devnet: `v11.0.0-rc.1`

### Method B: Build from source

If you build from source, check the required Go version in upstream `go.mod` before compiling:

```bash
REQUIRED_GO=$(curl -fsSL https://raw.githubusercontent.com/xrplevm/node/main/go.mod | awk '/^go /{print $2; exit}')
echo "Required Go: ${REQUIRED_GO}"
go version
```

If your installed Go version is older than `REQUIRED_GO`, upgrade Go before compiling.

```bash
sudo apt-get update
sudo apt-get install -y golang-go

git clone https://github.com/xrplevm/node.git
cd node
make build
sudo mv build/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

## 2) Initialize and configure for your network

{% admonition type="warning" name="Mandatory EVM chain-id setting (v10+)" %}
Before starting the node, set `evm-chain-id` under `[evm]` in `app.toml`:

- Mainnet: `evm-chain-id = "1440000"`
- Testnet: `evm-chain-id = "1449000"`

File path (depending on your node home): `~/.exrpd/config/app.toml` or `/var/lib/exrpd/.exrpd/config/app.toml`.
{% /admonition %}

{% admonition type="warning" name="Validator signer safety" %}
If this node is an active validator signer, do not re-run `exrpd init` on an existing signer home and do not replace signer state from snapshots. Use one active signer only and preserve `~/.exrpd/data/priv_validator_state.json`.
{% /admonition %}

{% tabs %}

{% tab label="Mainnet" %}

```bash
exrpd config set client chain-id xrplevm_1440000-1
exrpd init <moniker> --chain-id xrplevm_1440000-1
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json
PEERS=$(curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/mainnet/peers.txt | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^seeds *=.*/seeds = \"${PEERS}\"/" ~/.exrpd/config/config.toml
```

{% /tab %}

{% tab label="Testnet" %}

```bash
exrpd config set client chain-id xrplevm_1449000-1
exrpd init <moniker> --chain-id xrplevm_1449000-1
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json
PEERS=$(curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/testnet/peers.txt | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^seeds *=.*/seeds = \"${PEERS}\"/" ~/.exrpd/config/config.toml
```

{% /tab %}

{% tab label="Devnet" %}

```bash
exrpd config set client chain-id xrplevm_1449900-1
exrpd init <moniker> --chain-id xrplevm_1449900-1
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/devnet/genesis.json
PEERS=$(curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/devnet/peers.txt | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^seeds *=.*/seeds = \"${PEERS}\"/" ~/.exrpd/config/config.toml
```

{% /tab %}

{% /tabs %}

## 3) Restore snapshot data

{% admonition type="warning" name="Signer role restriction" %}
Snapshot restore is for bootstrapping non-signing nodes (or fresh replacement nodes) only. For active validator signer nodes, do not overwrite chain state with snapshot restore.
{% /admonition %}

{% tabs %}

{% tab label="Mainnet" %}
Use any provider from [Snapshots](../resources/snapshots.md) and extract into `~/.exrpd`:

```bash
cd ~/.exrpd
wget -O exrpd.tar.lz4 https://evm-sidechain-snapshots-mainnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4
tar -xI lz4 -f exrpd.tar.lz4
```

If this URL is temporarily unavailable, use another provider from [Snapshots](../resources/snapshots.md) and follow that provider's archive format (`.tar.lz4` or `.tar.zst`) and extraction command.
{% /tab %}

{% tab label="Testnet" %}
Use any provider from [Snapshots](../resources/snapshots.md) and extract into `~/.exrpd`:

```bash
cd ~/.exrpd
wget -O exrpd.tar.lz4 https://evm-sidechain-snapshots-testnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4
tar -xI lz4 -f exrpd.tar.lz4
```

If this URL is temporarily unavailable, use another provider from [Snapshots](../resources/snapshots.md) and follow that provider's archive format (`.tar.lz4` or `.tar.zst`) and extraction command.
{% /tab %}

{% tab label="Devnet" %}
If no public Devnet snapshot is available, use [State Sync](../advanced/sync-options.md#state-sync) or [Sync from Genesis](./sync-from-genesis.md).
{% /tab %}

{% /tabs %}

## 4) Run the node

### Option A: `systemctl` (recommended)

Create a dedicated runtime user (recommended):

```bash
sudo useradd --system --home /var/lib/exrpd --create-home --shell /usr/sbin/nologin exrpd || true
sudo mkdir -p /var/lib/exrpd/.exrpd
sudo chown -R exrpd:exrpd /var/lib/exrpd
```

Then move your initialized node home to the service path:

```bash
sudo rsync -a ~/.exrpd/ /var/lib/exrpd/.exrpd/
sudo chown -R exrpd:exrpd /var/lib/exrpd/.exrpd
```

Create `/etc/systemd/system/exrpd.service`:

```ini
[Unit]
Description=XRPL EVM Node (exrpd)
After=network-online.target

[Service]
User=exrpd
Group=exrpd
WorkingDirectory=/var/lib/exrpd
Environment="HOME=/var/lib/exrpd"
ExecStart=/usr/local/bin/exrpd start --home /var/lib/exrpd/.exrpd
Restart=always
RestartSec=3
LimitNOFILE=65535
NoNewPrivileges=true
PrivateTmp=true
ProtectSystem=full

[Install]
WantedBy=multi-user.target
```

Start it:

```bash
sudo systemctl daemon-reload
sudo systemctl enable exrpd
sudo systemctl start exrpd
sudo journalctl -u exrpd -f
```

### Option B: `cosmovisor` + `systemctl`

Install Go first (required for `go install`):

```bash
sudo apt-get update
sudo apt-get install -y golang-go
```

Install and prepare layout:

```bash
sudo env GOBIN=/usr/local/bin go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
sudo mkdir -p /var/lib/exrpd/.exrpd/cosmovisor/genesis/bin
sudo cp /usr/local/bin/exrpd /var/lib/exrpd/.exrpd/cosmovisor/genesis/bin/exrpd
sudo chmod +x /var/lib/exrpd/.exrpd/cosmovisor/genesis/bin/exrpd
sudo ln -sfn /var/lib/exrpd/.exrpd/cosmovisor/genesis /var/lib/exrpd/.exrpd/cosmovisor/current
sudo chown -R exrpd:exrpd /var/lib/exrpd/.exrpd
```

If your restored data includes `upgrade-info.json`, pre-stage the binary for that upgrade name before starting Cosmovisor:

```bash
UPGRADE_NAME=$(jq -r '.name // empty' /var/lib/exrpd/.exrpd/data/upgrade-info.json 2>/dev/null || true)
if [ -n "$UPGRADE_NAME" ]; then
  sudo mkdir -p "/var/lib/exrpd/.exrpd/cosmovisor/upgrades/${UPGRADE_NAME}/bin"
  sudo cp /usr/local/bin/exrpd "/var/lib/exrpd/.exrpd/cosmovisor/upgrades/${UPGRADE_NAME}/bin/exrpd"
  sudo chmod +x "/var/lib/exrpd/.exrpd/cosmovisor/upgrades/${UPGRADE_NAME}/bin/exrpd"
  sudo chown -R exrpd:exrpd /var/lib/exrpd/.exrpd/cosmovisor/upgrades
fi
```

Create `/etc/systemd/system/cosmovisor-exrpd.service`:

```ini
[Unit]
Description=XRPL EVM Node (Cosmovisor)
After=network-online.target

[Service]
User=exrpd
Group=exrpd
WorkingDirectory=/var/lib/exrpd
Environment="DAEMON_NAME=exrpd"
Environment="DAEMON_HOME=/var/lib/exrpd/.exrpd"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="UNSAFE_SKIP_BACKUP=false"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="HOME=/var/lib/exrpd"
ExecStart=/usr/local/bin/cosmovisor run start --home /var/lib/exrpd/.exrpd
Restart=always
RestartSec=3
LimitNOFILE=65535
NoNewPrivileges=true
PrivateTmp=true
ProtectSystem=full

[Install]
WantedBy=multi-user.target
```

Start it:

```bash
sudo systemctl daemon-reload
sudo systemctl enable cosmovisor-exrpd
sudo systemctl start cosmovisor-exrpd
sudo journalctl -u cosmovisor-exrpd -f
```

With `UNSAFE_SKIP_BACKUP=false`, first start may take several minutes while Cosmovisor creates a full data backup before restart.

For upgrade operations and safer backup settings, follow [Upgrading your node](../guides/upgrading-your-node.md).

### Option C: `docker`

Install Docker first if it is not available:

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl enable --now docker
```

```bash
TARGET_TAG=<target-tag>

# Make RPC reachable through -p 26657:26657
# Note: this RPC listener is in config.toml ([rpc].laddr), not app.toml.
sed -i.bak -E 's#^laddr = "tcp://127.0.0.1:26657"#laddr = "tcp://0.0.0.0:26657"#' ~/.exrpd/config/config.toml

docker run -d \
  --name xrplevm-node \
  --restart unless-stopped \
  -p 26657:26657 \
  -v /root/.exrpd:/root/.exrpd \
  --entrypoint exrpd \
  peersyst/exrp:${TARGET_TAG} \
  start

docker logs -f xrplevm-node
```

Use the exact `<target-tag>` from [Networks](../resources/networks.md) (for example, Mainnet `v10.1.0`).

## Validation

```bash
exrpd status
curl -s localhost:26657/status | jq .result.sync_info
```

{% admonition type="warning" name="Validator signer safety" %}
If this node is a validator signer, keep a **single active validator signer** only. Never run two active instances with the same `~/.exrpd/config/priv_validator_key.json`, and never roll back `~/.exrpd/data/priv_validator_state.json`.
{% /admonition %}

{% admonition type="info" name="Operator security note" %}
For signer nodes, avoid `exrpd unsafe-reset-all`, snapshot overwrite on active signer data, and parallel runtime stacks (for example `systemctl` plus `docker`) using the same validator key.
{% /admonition %}

## Next steps

- [Join the XRPL EVM](./join-the-xrplevm.md)
- [Sync from Genesis](./sync-from-genesis.md)
- [Sync options](../advanced/sync-options.md)
