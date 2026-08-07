# Upgrading your node

Upgrades are coordinated at a specific block height. Nodes running old binaries halt at that height and must be upgraded before they can continue.

For validators, the primary risk during upgrades is **double-signing**. This happens when more than one active validator signer uses the same validator key at the same time (or with inconsistent signer state), which can lead to slashing/jailing.

This guide is a security-first runbook for all run stacks:

- `systemctl`
- `cosmovisor`
- `docker`
- raw binary upgrades
- source-built upgrades

{% admonition type="info" name="List of upgrades" %}
Upgrade heights and target versions for Mainnet/Testnet/Devnet are listed in [Networks](../resources/networks.md).
{% /admonition %}

## Critical validator safety rules

Follow these rules for every validator upgrade:

1. **One signer only**: keep a single active validator signer for `priv_validator_key.json`.
2. **Stop before replace**: fully stop old process/container/service before changing binaries.
3. **Preserve signer state**: never delete or roll back `priv_validator_state.json`.
4. **Never clone signing state to a second active host**.
5. **Verify single active instance** before restart.

{% admonition type="warning" name="Double-sign risk" %}
Do not start a second node (VM, container, backup host, or old binary) with the same validator key while your primary validator is still running. This is the most common path to double-signing and can tombstone your validator, meaning that validator key can never become an active validator again.
{% /admonition %}

## Secure upgrade flow (mandatory order)

Use this exact sequence for validators, regardless of stack.

Use your actual node home path in commands below:

- If you run as root defaults: `~/.exrpd`
- If you followed hardened `systemd`/`cosmovisor` setup: `/var/lib/exrpd/.exrpd`

### 1) Confirm target upgrade

- Read target height/version from [Networks](../resources/networks.md).
- Download the target release artifact from [XRPL EVM node releases](https://github.com/xrplevm/node/releases).
- Use the **on-chain upgrade name** from `upgrade-info.json` for Cosmovisor folder naming. Do not assume upgrade proposal name equals release tag (for example, proposal name `v10.0.0` may require binary `v10.0.1` or `v10.0.2`).

### 2) Pre-upgrade checks

```bash
exrpd status
```

Record current height and ensure your node is healthy before starting.

### 3) Stop signer process completely

Stop your runtime stack and confirm the process is gone.

```bash
pgrep -fa exrpd
```

Expected: no active validator signer `exrpd` process.

### 4) Backup validator-critical files

```bash
NODE_HOME=${NODE_HOME:-~/.exrpd}
mkdir -p ~/exrpd-upgrade-backup
cp "$NODE_HOME"/config/priv_validator_key.json ~/exrpd-upgrade-backup/
cp "$NODE_HOME"/data/priv_validator_state.json ~/exrpd-upgrade-backup/
cp "$NODE_HOME"/config/node_key.json ~/exrpd-upgrade-backup/
```

Do not edit these files manually.

### 5) Upgrade binary (raw or source)

#### Raw binary upgrade

```bash
cd /tmp
TARGET_TAG=<target-tag>
wget "https://github.com/xrplevm/node/releases/download/${TARGET_TAG}/node_${TARGET_TAG#v}_Linux_amd64.tar.gz"
tar -xzf "node_${TARGET_TAG#v}_Linux_amd64.tar.gz"
sudo mv bin/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

#### Upgrade from source

```bash
cd /tmp
rm -rf node
git clone https://github.com/xrplevm/node.git
cd node
git checkout <target-tag>
make build
sudo mv build/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

If you build from source, check required Go version first:

```bash
REQUIRED_GO=$(curl -fsSL https://raw.githubusercontent.com/xrplevm/node/main/go.mod | awk '/^go /{print $2; exit}')
echo "Required Go: ${REQUIRED_GO}"
go version
```

### 5.1) Apply mandatory EVM chain-id config (v10+)

After installing the target binary and **before starting the node**, ensure `evm-chain-id` is present under `[evm]` in `app.toml`:

- Mainnet (`xrplevm_1440000-1`): `evm-chain-id = "1440000"`
- Testnet (`xrplevm_1449000-1`): `evm-chain-id = "1449000"`

File location depends on your home path:

- `~/.exrpd/config/app.toml`
- or `/var/lib/exrpd/.exrpd/config/app.toml`

Example check:

```bash
NODE_HOME=${NODE_HOME:-~/.exrpd}
awk '/^\[evm\]/{f=1;next} /^\[/{f=0} f && /^evm-chain-id/{print;found=1} END{if(!found) print "MISSING"}' "$NODE_HOME"/config/app.toml
```

{% admonition type="warning" name="Consensus safety" %}
If `evm-chain-id` is missing or wrong for your network, your node can fail to participate correctly in consensus after upgrade height.
{% /admonition %}

### 6) Start exactly one signer

Start your runtime stack (only one instance) and follow logs.

### 7) Post-upgrade validation

```bash
exrpd status
curl -s localhost:26657/status | jq .result.sync_info
```

Confirm blocks are advancing and no upgrade halt error remains.

## Stack-specific upgrade commands

### `systemctl`

```bash
sudo systemctl stop exrpd
pgrep -fa exrpd

# replace /usr/local/bin/exrpd with target binary

sudo systemctl start exrpd
sudo journalctl -u exrpd -f
```

### `cosmovisor`

Place the new binary in the upgrade folder matching the on-chain upgrade name:

```bash
DAEMON_HOME=${DAEMON_HOME:-~/.exrpd}
mkdir -p "$DAEMON_HOME"/cosmovisor/upgrades/<upgrade-name>/bin
cp /usr/local/bin/exrpd "$DAEMON_HOME"/cosmovisor/upgrades/<upgrade-name>/bin/exrpd
chmod +x "$DAEMON_HOME"/cosmovisor/upgrades/<upgrade-name>/bin/exrpd
```

If `upgrade-info.json` is present, you can derive the folder name directly:

```bash
DAEMON_HOME=${DAEMON_HOME:-~/.exrpd}
UPGRADE_NAME=$(jq -r '.name // empty' "$DAEMON_HOME"/data/upgrade-info.json 2>/dev/null || true)
echo "$UPGRADE_NAME"
```

`UPGRADE_NAME` is the folder to use under `cosmovisor/upgrades/`, even when the required release binary version differs from that name.

Then run:

```bash
sudo systemctl restart cosmovisor-exrpd
sudo journalctl -u cosmovisor-exrpd -f
```

{% admonition type="warning" name="Cosmovisor path" %}
Use `~/.exrpd/...` (not `~/.exprd/...`).
{% /admonition %}

### `docker`

```bash
docker pull peersyst/exrp:<target-tag>
docker stop xrplevm-node
docker rm xrplevm-node

docker run -d \
  --name xrplevm-node \
  --restart unless-stopped \
  -v /root/.exrpd:/root/.exrpd \
  --entrypoint exrpd \
  peersyst/exrp:<target-tag> \
  start

docker logs -f xrplevm-node
```

Do not keep an old validator container running in parallel.

## Hotfix recovery runbook (only for impacted existing nodes)

Use this section only when governance/core team announces an incident hotfix and your node/validator was already running and impacted.

If you are a fresh node setup, skip this section and follow the normal install flow for your network's current version.

1. Stop all signer instances that could use the same validator key, then verify no signer is running:

```bash
sudo systemctl stop exrpd 2>/dev/null || true
sudo systemctl stop cosmovisor-exrpd 2>/dev/null || true
docker stop xrplevm-node 2>/dev/null || true
pgrep -fa exrpd
```

Expected: no active validator signer process.

2. Install the announced hotfix binary (do not start yet).
3. Backup signer state:

```bash
cp ~/.exrpd/data/priv_validator_state.json ~/.exrpd/priv_validator_state.json
```

4. Restore state:
  - Recommended: restore from a compatible snapshot provider (for example PolkaChu, Cumulo, or XRPL EVM snapshot S3).
  - Alternative (resource-heavy): `exrpd rollback`
5. Apply required config changes announced with the hotfix.

If governance requires an EVM chain-id update, set it in `app.toml` under `[evm]`:

```toml
[evm]
evm-chain-id = "<network-evm-chain-id>"
```

6. Restore signer state file and permissions:

```bash
cp ~/.exrpd/priv_validator_state.json ~/.exrpd/data/priv_validator_state.json
chmod 600 ~/.exrpd/data/priv_validator_state.json
```

7. Before start, confirm only one host/container will be allowed to sign with this validator key.

8. Start exactly one runtime stack:

```bash
exrpd start
```

{% admonition type="warning" name="Critical double-sign protection" %}
Never run two active instances with the same `priv_validator_key.json` at the same time (primary + backup, old + new binary, systemd + docker, etc.). During hotfixes, this is the highest-risk mistake and can lead to slashing/jailing and tombstoning (permanent validator removal for that key).
{% /admonition %}

{% admonition type="info" name="Current known example" %}
During the Testnet v10 incident, affected nodes moved from `v10.0.0` to `v10.0.1` and required `evm-chain-id = "1449000"`.
{% /admonition %}

{% admonition type="info" name="Snapshot restore during incident" %}
If restoring from a pre-upgrade snapshot with Cosmovisor, stage the required binary in the upgrade folder from `upgrade-info.json` before service start.
{% /admonition %}

## Automated upgrades with Cosmovisor

Cosmovisor can reduce manual intervention during upgrades. It can also increase operational risk if misconfigured, so validator operators should test in non-production first.

Install:

```bash
sudo apt-get update
sudo apt-get install -y golang-go
sudo env GOBIN=/usr/local/bin go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
/usr/local/bin/cosmovisor --help >/dev/null
```

Recommended environment variables:

```bash
export DAEMON_NAME=exrpd
export DAEMON_HOME=$HOME/.exrpd
export DAEMON_RESTART_AFTER_UPGRADE=true
export UNSAFE_SKIP_BACKUP=false
export DAEMON_ALLOW_DOWNLOAD_BINARIES=false
```

{% admonition type="warning" name="Auto-download caution" %}
For validators, prefer `DAEMON_ALLOW_DOWNLOAD_BINARIES=false` and stage binaries manually. Auto-download introduces an external dependency at upgrade time.
{% /admonition %}

## Incident prevention checklist

Before starting after upgrade, confirm:

- old process is fully stopped
- no second host/container is signing with same key
- `priv_validator_state.json` is intact and not rolled back
- target binary version is installed
- logs show normal block progression

## Additional Resources

- [Cosmovisor Docs (Cosmos SDK)](https://docs.cosmos.network/main/build/tooling/cosmovisor)
- [XRPL EVM Sidechain Repository](https://github.com/xrplevm/node)
