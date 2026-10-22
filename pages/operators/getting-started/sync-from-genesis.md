# Sync from Genesis

Use this guide when you need full historical replay from block 0.

For most operators, [Installing the Node](./installing-the-node.md) with snapshot/state-sync is faster and simpler. Genesis sync is slower and requires binary upgrades at specific heights.

## Network upgrade path

When syncing from genesis, you must start from the network genesis binary and upgrade at each hard fork.

{% tabs %}

{% tab label="Mainnet" %}

| Upgrade Name | Height    | Binary Version                           |
| ------------ | --------- | ---------------------------------------- |
| Genesis      | `0`       | `v7.0.0`                                 |
| `v8.0.0`     | `497000`  | `v8.0.2`                                 |
| `v9.0.0`     | `4688681` | `v9.0.3`                                 |
| `v10.0.0`    | `4749000` | `v10.0.2`                                |
| `v10.1.0`    | `6856000` | `v10.1.0`, then `v10.2.1` from `7360029` |
| `v11.2.0`    | `7823200` | `v11.2.0`                                |

{% /tab %}

{% tab label="Testnet" %}

| Upgrade Name | Height    | Binary Version |
| ------------ | --------- | -------------- |
| Genesis      | `0`       | `v6.0.0`       |
| `v7.0.0`     | `547100`  | `v7.0.0`       |
| `v8.0.0`     | `1485600` | `v8.0.0`       |
| `v9.0.0`     | `3827000` | `v9.0.3`       |
| `v10.0.0`    | `5601606` | `v10.0.1`      |
| `v10.1.0`    | `7725000` | `v10.1.0`      |
| `v11.1.0`    | `8158200` | `v11.1.1`      |

{% /tab %}

{% tab label="Devnet" %}

| Upgrade Name | Height    | Binary Version |
| ------------ | --------- | -------------- |
| Genesis      | `0`       | `v9.0.3`       |
| `v10.0.0`    | `1198644` | `v10.0.3`      |
| `v11.0.0`    | `2089191` | `v11.0.0-rc.1` |

{% /tab %}

{% /tabs %}

**Upgrade Name** is the on-chain upgrade plan name, which is also the Cosmovisor folder name (`cosmovisor/upgrades/<upgrade-name>/bin/exrpd`). It does not always match the release tag you install in that folder.

{% admonition type="warning" name="Pick the binary for each range" %}

- A binary only contains the upgrade handlers registered for its release line. `v11.2.0` includes the Mainnet `v11.2.0` handler but not the Testnet `v11.1.0` handler, so Testnet must use `v11.1.x` from height `8158200`. Do not use the Docker `latest` tag (it follows the newest Mainnet release) or `main` (development build).
- Mainnet halted at block `7360028` on 2026-08-23 and resumed at `7360029` on the patched `v10.2.0` release, without an on-chain upgrade. `v10.2.x` shares the `v10.1.0` upgrade handler. To replay this range as it was produced, run `v10.1.0` with `--halt-height 7360028`, then continue with `v10.2.1`.
- `v10.1.0`, `v10.2.0` and `v10.2.1` cannot be built from source because they depend on non-public security fixes. Use the release binaries or the Docker images for that range.

{% /admonition %}

{% admonition type="info" name="Source of truth" %}
Upgrade schedule and versions are maintained in [Networks](../resources/networks.md).
{% /admonition %}

## Prerequisites

- `exrpd` installed (raw binary or built from source)
- Node initialized for your target network
- Genesis file and peers configured
- Process manager ready (`systemctl`, `cosmovisor`, or `docker`)

See [Installing the Node](./installing-the-node.md) for install and runtime options.

## Step 1: Install the genesis binary

Install the binary for the **first** version in your network’s upgrade path table.

```bash
cd /tmp
wget https://github.com/xrplevm/node/releases/download/v7.0.0/node_7.0.0_Linux_amd64.tar.gz
tar -xzf node_7.0.0_Linux_amd64.tar.gz
sudo mv bin/exrpd /usr/local/bin/exrpd
sudo chmod +x /usr/local/bin/exrpd
exrpd version
```

Adjust the version/tag according to your network (for example, Testnet genesis starts at `v6.0.0`).

## Step 2: Start the node and sync

Run with your preferred stack:

- `systemctl`: `systemctl start exrpd`
- `cosmovisor`: `systemctl start cosmovisor-exrpd` (or `cosmovisor run start`)
- `docker`: run container with explicit tag matching the current upgrade stage version

Monitor logs:

```bash
journalctl -u exrpd -f
# or
journalctl -u cosmovisor-exrpd -f
# or
docker logs -f xrplevm-node
```

## Step 3: Upgrade when the node halts

At each upgrade height, old binaries halt with an upgrade-needed message. Replace the binary with the target version and restart.

{% admonition type="warning" name="Mandatory EVM chain-id setting (v10+)" %}
Before starting with a v10+ binary, make sure `evm-chain-id` under `[evm]` in `app.toml` matches your network. Homes initialized with an older binary do not have the right value:

- Mainnet: `evm-chain-id = 1440000`
- Testnet: `evm-chain-id = 1449000`
- Devnet: `evm-chain-id = 1449900`

File path (depending on your node home): `~/.exrpd/config/app.toml` or `/var/lib/exrpd/.exrpd/config/app.toml`.
{% /admonition %}

{% admonition type="warning" name="Validator signer safety" %}
If this node is a validator signer, keep a **single active validator signer** only. Never run two active instances with the same `~/.exrpd/config/priv_validator_key.json`, and never roll back `~/.exrpd/data/priv_validator_state.json`.
{% /admonition %}

### Raw binary / `systemctl`

```bash
sudo systemctl stop exrpd
# replace /usr/local/bin/exrpd with new version
sudo systemctl start exrpd
```

### `cosmovisor`

Place the new binary at the folder named after the on-chain upgrade (the **Upgrade Name** column above):

```text
~/.exrpd/cosmovisor/upgrades/<upgrade-name>/bin/exrpd
```

For a full replay you can stage every folder before starting. For example, on Mainnet: `genesis` (`v7.0.0`), `v8.0.0` (`v8.0.2`), `v9.0.0` (`v9.0.3`), `v10.0.0` (`v10.0.2`), `v10.1.0` (`v10.1.0`, swapped for `v10.2.1` after block `7360028`) and `v11.2.0` (`v11.2.0`).

Ensure executable permissions and restart if needed:

```bash
chmod +x ~/.exrpd/cosmovisor/upgrades/<upgrade-name>/bin/exrpd
sudo systemctl restart cosmovisor-exrpd
```

### `docker`

```bash
docker pull peersyst/exrp:<new-version>
docker stop xrplevm-node && docker rm xrplevm-node

# Make RPC reachable through -p 26657:26657
# Note: this RPC listener is in config.toml ([rpc].laddr), not app.toml.
sed -i.bak -E 's#^laddr = "tcp://127.0.0.1:26657"#laddr = "tcp://0.0.0.0:26657"#' ~/.exrpd/config/config.toml

docker run -d --name xrplevm-node --restart unless-stopped \
  -p 26657:26657 \
  -v /root/.exrpd:/root/.exrpd --entrypoint exrpd peersyst/exrp:<new-version> start
```

Repeat until you reach the latest network version.

## Verify sync

```bash
exrpd status
curl -s localhost:26657/status | jq .result.sync_info
```

## Related guides

- [Installing the Node](./installing-the-node.md)
- [Upgrading your node](../guides/upgrading-your-node.md)
- [Sync options](../advanced/sync-options.md)
