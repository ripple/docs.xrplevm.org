# Sync options

The XRPL EVM sidechain supports various synchronization methods for setting up and running the `exrpd` binary. These options allow node operators to choose the best method for their use case, depending on factors such as the desired setup time, available resources, and the level of historical data required. Below, we detail the three main synchronization options and provide step-by-step instructions for each.

## Sync from Genesis

Syncing from genesis initializes your `exrpd` node from the very beginning of the XRPL EVM blockchain's history. This method is comprehensive but time-intensive, as it downloads and verifies all blocks and transactions since the chain's inception.

For the full procedure (including Mainnet/Testnet/Devnet version paths and upgrade handling), follow [Sync from Genesis](../getting-started/sync-from-genesis.md).

1. **Install `exrpd`**: Ensure that the first version of the `exrpd` binary is installed and configured on your system. Refer to the [Networks page](../resources/networks.md) for downloading the first version of the binary for each network and to the [Installation Guide](../getting-started/installing-the-node.md) for detailed setup steps.

2. **Start the Node**:

```bash
exrpd start
```

3. **Monitor Progress**:
   Wait for the node to sync until it reaches the block of the next version. Then, upgrade to the newer version and repeat this process until the node is fully in sync with the network. For detailed upgrade instructions, refer to the [Upgrading your node](../guides/upgrading-your-node.md) page.

{% admonition type="warning" name="Validator signer safety" %}
If this node is a validator signer, keep a **single active validator signer** only. Never run two active instances with the same `~/.exrpd/config/priv_validator_key.json`, and never roll back `~/.exrpd/data/priv_validator_state.json`.
{% /admonition %}

## Sync from Snapshot

A snapshot starts your node from a recent state file instead of replaying the chain, which takes much less time. You download the archive and extract it into `~/.exrpd`, which creates the `data/` directory.

1. **Select a Snapshot**: Choose a snapshot file from a trusted provider listed on the [Snapshots](../resources/snapshots.md) page.

2. **Download and decompress the snapshot**:

```bash
cd ~/.exrpd
# On a node that already has data: move it aside and keep the signer state file
if [ -d data ]; then
  mv data data.old
  mkdir data
  cp data.old/priv_validator_state.json data/
fi
wget -O exrpd.tar.lz4 <snapshot_url>
tar -xI lz4 -f exrpd.tar.lz4 --exclude='data/priv_validator_state.json'
```

The Peersyst archives include a `priv_validator_state.json` reset to height `0`. Excluding it keeps your node's own signer state. Remove `data.old` once the node is syncing. See [Snapshots](../resources/snapshots.md) for other providers and archive formats.

{% admonition type="warning" name="Validator signer safety" %}
Do not restore snapshots over an active validator signer home. Snapshot restore is for non-signing nodes or fresh replacement nodes only.
{% /admonition %}

3. **Start the Node**:

```bash
exrpd start
```

## State Sync

State sync starts a new node from a snapshot of the application state served by its peers, instead of replaying every block. The node starts with a truncated block history, from the height of the restored snapshot. For more information, see the [CometBFT state sync documentation](https://docs.cosmos.network/cometbft/latest/docs/core/state-sync).

State sync needs two things: RPC servers to verify the snapshot with a light client, and peers that serve state sync snapshots. Several Mainnet and Testnet nodes publish snapshots every few hundred to few thousand blocks.

| Network | RPC server                                   |
| ------- | -------------------------------------------- |
| Mainnet | `https://cosmos-rpc.xrplevm.org:443`         |
| Testnet | `https://cosmos-rpc.testnet.xrplevm.org:443` |

This procedure covers Mainnet and Testnet. It is not validated on Devnet, whose public RPC node only lists private peers. On Devnet, use the [snapshot](../resources/snapshots.md) or [Sync from Genesis](../getting-started/sync-from-genesis.md) instead.

1. **Set the RPC server and peers**:

   ```bash
   SNAP_RPC="https://cosmos-rpc.xrplevm.org:443" # Testnet: https://cosmos-rpc.testnet.xrplevm.org:443
   CONFIG="$HOME/.exrpd/config/config.toml"

   # Peers currently connected to the RPC node. More peers make it more likely to find a recent snapshot.
   PERSISTENT_PEERS=$(curl -s "$SNAP_RPC/net_info" | jq -r '.result.peers[] | select(.remote_ip | test("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[01])\\.)") | not) | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":") | last)"' | paste -sd, -)
   sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"${PERSISTENT_PEERS}\"/" "$CONFIG"
   ```

2. **Retrieve a trusted block height and hash**:

   Choose a height within the trust period, close to `current height - 1000`:

   ```bash
   LATEST_HEIGHT=$(curl -s "$SNAP_RPC/block" | jq -r .result.block.header.height)
   if [ -z "$LATEST_HEIGHT" ] || [ "$LATEST_HEIGHT" = "null" ]; then
     echo "Could not read the latest height from $SNAP_RPC. Do not continue."
   else
     TRUST_HEIGHT=$((LATEST_HEIGHT - 1000))
     TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$TRUST_HEIGHT" | jq -r .result.block_id.hash)
   fi

   echo "trust_height = $TRUST_HEIGHT"
   echo "trust_hash   = $TRUST_HASH"
   ```

   {% admonition type="warning" name="Use block_id.hash" %}
   The trust hash must be the block's **`block_id.hash`** (the `jq` path above). The `/block` response contains several fields named exactly `"hash"`. A greedy regex over the single-line JSON picks the **last** one, which is `last_commit.block_id.parts.hash` (a part-set hash of the previous block), and the node then fails at startup with `expected header's hash X, but got Y`.

   Without `jq`, take the first match and strip the key: `block_id` is always serialized before `block`, so the first `"hash"` is always `block_id.hash`.

   ```bash
   TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$TRUST_HEIGHT" | grep -oE '"hash":"[A-F0-9]{64}"' | head -1 | grep -oE '[A-F0-9]{64}')
   ```
   {% /admonition %}

   You can also read a recent height and its block hash from a block explorer.

3. **Update the `[statesync]` section**:

   ```bash
   sed -i.bak \
     -e "/^\[statesync\]/,/^\[/ s|^enable = .*|enable = true|" \
     -e "/^\[statesync\]/,/^\[/ s|^rpc_servers = .*|rpc_servers = \"$SNAP_RPC,$SNAP_RPC\"|" \
     -e "/^\[statesync\]/,/^\[/ s|^trust_height = .*|trust_height = $TRUST_HEIGHT|" \
     -e "/^\[statesync\]/,/^\[/ s|^trust_hash = .*|trust_hash = \"$TRUST_HASH\"|" \
     -e "/^\[statesync\]/,/^\[/ s|^trust_period = .*|trust_period = \"112h0m0s\"|" \
     -e "/^\[statesync\]/,/^\[/ s|^discovery_time = .*|discovery_time = \"60s\"|" \
     "$CONFIG"
   ```

   The result looks like this:

   ```toml
   [statesync]
   enable = true
   rpc_servers = "https://cosmos-rpc.xrplevm.org:443,https://cosmos-rpc.xrplevm.org:443"
   trust_height = <trust_height>
   trust_hash = "<trust_hash>"
   trust_period = "112h0m0s"
   discovery_time = "60s"
   ```

   - `rpc_servers` needs at least two entries. Listing the same trusted server twice is allowed.
   - `trust_period` must be shorter than the unbonding time, which is 7 days (`168h`) on Mainnet and Testnet. Use about two thirds of it (`112h`). The generated `config.toml` default (`168h0m0s`) is too long.
   - A longer `discovery_time` gives the node time to find a recent snapshot among its peers.

4. **Start the Node**:

   - Run the following command to begin state sync:
     ```bash
     exrpd start
     ```
   - The node discovers snapshots, restores the most recent one it can verify, then starts executing blocks. Logs should look similar to:
     ```
     INF Discovered new snapshot format=3 hash=C740... height=8261500 module=statesync
     INF Snapshot accepted, restoring format=3 hash=C740... height=8261500 module=statesync
     INF Applied snapshot chunk to ABCI app chunk=0 format=3 height=8261500 module=statesync total=3
     INF Snapshot restored format=3 hash=C740... height=8261500 module=statesync
     ```
   - Older snapshots that cannot be verified are rejected (`Snapshot rejected`) and the node moves on to the next one.

5. **Verify Synchronization**:
   - Once state sync completes, the node will begin processing blocks normally. If state sync fails with an error such as `state sync aborted`, first restart `exrpd` and verify your `statesync` settings and trusted RPC endpoints.
   - For non-signing nodes only, you may reset data and retry state sync. Do not run reset commands on active validator signer nodes that hold production `priv_validator_key.json`.
