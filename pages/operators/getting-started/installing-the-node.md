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
| Mainnet | `xrplevm_1440000-1` | `v11.2.0`       | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json) |
| Testnet | `xrplevm_1449000-1` | `v11.1.1`       | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json) |
| Devnet  | `xrplevm_1449900-1` | `v11.0.0-rc.1`  | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/devnet/genesis.json)  |

## Recommended flow (snapshot-first)

1. Install `exrpd` (raw binary or from source).
2. Initialize and configure your node for the target network.
3. Restore snapshot data (when available).
4. Run the node with `systemctl`, `cosmovisor`, or `docker`.

{% admonition type="info" name="Snapshot availability" %}
Public snapshot providers are listed for Mainnet, Testnet and Devnet in [Snapshots](../resources/snapshots.md). Snapshots are pruned and do not contain full history. If you need full history, use [Sync from Genesis](./sync-from-genesis.md). If a snapshot is not available on Mainnet or Testnet, use [State Sync](../advanced/sync-options.md#state-sync), which also starts from a recent height.
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
ARCH=amd64 # or arm64
ASSET="node_${TARGET_TAG#v}_Linux_${ARCH}.tar.gz"
wget -O "${ASSET}" "https://github.com/xrplevm/node/releases/download/${TARGET_TAG}/${ASSET}"
wget -O checksums.txt "https://github.com/xrplevm/node/releases/download/${TARGET_TAG}/checksums.txt"
sha256sum -c checksums.txt --ignore-missing
tar -xzf "${ASSET}"
sudo mv bin/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

Set `<target-tag>` from [Networks](../resources/networks.md) for your network:

- Mainnet: `v11.2.0`
- Testnet: `v11.1.1`
- Devnet: `v11.0.0-rc.1`

Release binaries are published for Linux (`amd64` and `arm64`) and macOS. Docker images are `amd64` only.

### Method B: Build from source

Always build a release tag. The default branch is under active development and does not match any network.

Check the Go version required by the tag you are building:

```bash
TARGET_TAG=<target-tag>
REQUIRED_GO=$(curl -fsSL "https://raw.githubusercontent.com/xrplevm/node/${TARGET_TAG}/go.mod" | awk '/^go /{print $2; exit}')
echo "Required Go: ${REQUIRED_GO}"
go version
```

Distribution packages (`apt install golang-go`) are usually too old. Install the `REQUIRED_GO` version or newer from [go.dev/dl](https://go.dev/dl/), then build:

```bash
git clone https://github.com/xrplevm/node.git
cd node
git checkout "${TARGET_TAG}"
make build
sudo mv bin/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

{% admonition type="info" name="Releases without public sources" %}
`v10.1.0`, `v10.2.0` and `v10.2.1` depend on non-public security fixes and cannot be built from source. Use the release binaries or Docker images for those versions.
{% /admonition %}

## 2) Initialize and configure for your network

{% admonition type="warning" name="Mandatory EVM chain-id setting (v10+)" %}
`evm-chain-id` under `[evm]` in `app.toml` must match your network. `exrpd init --chain-id <chain-id>` (v10 or later) writes it for you, but only when `app.toml` does not exist yet: run `exrpd init` before any `exrpd config set` command, which creates `app.toml` with `evm-chain-id = 0`. Check it before starting, especially on homes initialized with an older binary:

- Mainnet: `evm-chain-id = 1440000`
- Testnet: `evm-chain-id = 1449000`
- Devnet: `evm-chain-id = 1449900`

File path (depending on your node home): `~/.exrpd/config/app.toml` or `/var/lib/exrpd/.exrpd/config/app.toml`. To check it:

```bash
awk '/^\[evm\]/{f=1;next} /^\[/{f=0} f && /^evm-chain-id/' ~/.exrpd/config/app.toml
```
{% /admonition %}

{% admonition type="warning" name="Validator signer safety" %}
If this node is an active validator signer, do not re-run `exrpd init` on an existing signer home and do not replace signer state from snapshots. Use one active signer only and preserve `~/.exrpd/data/priv_validator_state.json`.
{% /admonition %}

{% tabs %}

{% tab label="Mainnet" %}

```bash
exrpd init <moniker> --chain-id xrplevm_1440000-1
exrpd config set client chain-id xrplevm_1440000-1
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json
PEERS=$(curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/mainnet/peers.txt | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^seeds *=.*/seeds = \"${PEERS}\"/" ~/.exrpd/config/config.toml
# Persistent peers taken from a public RPC node, in case the seeds are not enough
PERSISTENT_PEERS=$(curl -s https://cosmos-rpc.xrplevm.org/net_info | jq -r '.result.peers[] | select(.remote_ip | test("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[01])\\.)") | not) | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":") | last)"' | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"${PERSISTENT_PEERS}\"/" ~/.exrpd/config/config.toml
```

{% /tab %}

{% tab label="Testnet" %}

```bash
exrpd init <moniker> --chain-id xrplevm_1449000-1
exrpd config set client chain-id xrplevm_1449000-1
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json
PEERS=$(curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/testnet/peers.txt | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^seeds *=.*/seeds = \"${PEERS}\"/" ~/.exrpd/config/config.toml
# Persistent peers taken from a public RPC node, in case the seeds are not enough
PERSISTENT_PEERS=$(curl -s https://cosmos-rpc.testnet.xrplevm.org/net_info | jq -r '.result.peers[] | select(.remote_ip | test("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[01])\\.)") | not) | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":") | last)"' | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"${PERSISTENT_PEERS}\"/" ~/.exrpd/config/config.toml
```

{% /tab %}

{% tab label="Devnet" %}

```bash
exrpd init <moniker> --chain-id xrplevm_1449900-1
exrpd config set client chain-id xrplevm_1449900-1
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/devnet/genesis.json
PEERS=$(curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/devnet/peers.txt | sort -R | head -n 10 | paste -sd, -)
sed -i.bak -e "s/^seeds *=.*/seeds = \"${PEERS}\"/" ~/.exrpd/config/config.toml
```

{% /tab %}

{% /tabs %}

`peers.txt` lists seed nodes, which only help your node discover other peers. The `persistent_peers` line adds peers your node keeps connected to, taken from the `net_info` endpoint of a public RPC node. You can refresh them the same way at any time.

## 3) Restore snapshot data

{% admonition type="warning" name="Signer role restriction" %}
Snapshot restore is for bootstrapping non-signing nodes (or fresh replacement nodes) only. For active validator signer nodes, do not overwrite chain state with snapshot restore.
{% /admonition %}

Peersyst snapshots are pruned nodes restored with state sync and regenerated daily. They do not contain historical blocks or state. The archives also contain a `data/priv_validator_state.json` reset to height `0`, so the commands below exclude it and keep the file created by `exrpd init` (or your existing one).

{% tabs %}

{% tab label="Mainnet" %}
Use any provider from [Snapshots](../resources/snapshots.md) and extract into `~/.exrpd`:

```bash
cd ~/.exrpd
wget -O exrpd.tar.lz4 https://xrpl-evm-snapshots.s3.us-east-1.amazonaws.com/mainnet.tar.lz4
tar -xI lz4 -f exrpd.tar.lz4 --exclude='data/priv_validator_state.json'
```

If this URL is temporarily unavailable, use another provider from [Snapshots](../resources/snapshots.md) and follow that provider's archive format (`.tar.lz4` or `.tar.zst`) and extraction command.
{% /tab %}

{% tab label="Testnet" %}
Use any provider from [Snapshots](../resources/snapshots.md) and extract into `~/.exrpd`:

```bash
cd ~/.exrpd
wget -O exrpd.tar.lz4 https://xrpl-evm-snapshots.s3.us-east-1.amazonaws.com/testnet.tar.lz4
tar -xI lz4 -f exrpd.tar.lz4 --exclude='data/priv_validator_state.json'
```

If this URL is temporarily unavailable, use another provider from [Snapshots](../resources/snapshots.md) and follow that provider's archive format (`.tar.lz4` or `.tar.zst`) and extraction command.
{% /tab %}

{% tab label="Devnet" %}
Use any provider from [Snapshots](../resources/snapshots.md) and extract into `~/.exrpd`:

```bash
cd ~/.exrpd
wget -O exrpd.tar.lz4 https://xrpl-evm-snapshots.s3.us-east-1.amazonaws.com/devnet.tar.lz4
tar -xI lz4 -f exrpd.tar.lz4 --exclude='data/priv_validator_state.json'
```

If this URL is temporarily unavailable, use [Sync from Genesis](./sync-from-genesis.md). The [State Sync](../advanced/sync-options.md#state-sync) procedure covers Mainnet and Testnet only.
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

Install a recent Go release from [go.dev/dl](https://go.dev/dl/) first (required for `go install`; current Cosmovisor releases need Go 1.23 or newer).

Install and prepare layout:

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
sudo install -m755 "$(go env GOPATH)/bin/cosmovisor" /usr/local/bin/
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

With `UNSAFE_SKIP_BACKUP=false`, Cosmovisor backs up the full `data/` directory before applying each upgrade, so every upgrade can take several minutes and needs free disk space for that copy.

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

# Make RPC reachable from the host through -p 127.0.0.1:26657:26657 (localhost only)
# Note: this RPC listener is in config.toml ([rpc].laddr), not app.toml.
sed -i.bak -E 's#^laddr = "tcp://127.0.0.1:26657"#laddr = "tcp://0.0.0.0:26657"#' ~/.exrpd/config/config.toml

docker run -d \
  --name xrplevm-node \
  --restart unless-stopped \
  -p 26656:26656 \
  -p 127.0.0.1:26657:26657 \
  -v "$HOME/.exrpd:/root/.exrpd" \
  --entrypoint exrpd \
  peersyst/exrp:${TARGET_TAG} \
  start

docker logs -f xrplevm-node
```

Use the exact `<target-tag>` for your network from [Networks](../resources/networks.md). Do not use `latest` (it follows the newest release, which may not support your network) or `main` (development build).

The image runs as `root`, so the container reads the node home from `/root/.exrpd`; the volume maps your host `~/.exrpd` there. Port `26656` accepts inbound P2P connections, and the RPC is published on `127.0.0.1` only, because Docker port publishing bypasses host firewalls such as `ufw`.

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
