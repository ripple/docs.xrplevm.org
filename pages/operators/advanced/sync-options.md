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

Syncing from a snapshot significantly reduces setup time by initializing your node using a pre-generated state file. The snapshot is downloaded and then decompressed to the ~/.exrpd/data directory, preparing the node for quick synchronization with the network.

1. **Select a Snapshot**: Choose a snapshot file from a trusted provider listed on the Snapshots page.

2. **Download and decompress the snapshot**:

```bash
cd ~/.exrpd
wget -O exrpd.tar.lz4 <snapshot_url>
tar -xI lz4 -f exrpd.tar.lz4
```

{% admonition type="warning" name="Validator signer safety" %}
Do not restore snapshots over an active validator signer home. Snapshot restore is for non-signing nodes or fresh replacement nodes only.
{% /admonition %}

3. **Start the Node**:

```bash
exrpd start
```

## State Sync

State Sync is an efficient and fast way to bootstrap a new node. It works by replaying larger chunks of application state directly rather than replaying individual blocks or consensus rounds. For more information, see [CometBFT's State Sync docs](https://docs.cometbft.com/).

1. **Retrieve a Recent Block Height and Hash**:

   Choose any height/hash within the current bonding period. A height close to `current height - 1000` aligns well with the snapshot period.

   **Option A — automated (recommended).** Point `SNAP_RPC` at a trusted RPC endpoint and let the shell compute both values:

   ```bash
   SNAP_RPC="https://<trusted-rpc-endpoint>"

   LATEST_HEIGHT=$(curl -s "$SNAP_RPC/block" | jq -r .result.block.header.height)
   TRUST_HEIGHT=$((LATEST_HEIGHT - 1000))
   TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$TRUST_HEIGHT" | jq -r .result.block_id.hash)

   echo "trust_height = $TRUST_HEIGHT"
   echo "trust_hash   = $TRUST_HASH"
   ```

   **Option B — manual.** Visit a blockchain explorer and read a recent block height and its corresponding block hash directly.

   {% admonition type="warning" name="Use block_id.hash — the FIRST hash, not any hash" %}
   The trust hash must be the block's **`block_id.hash`** (the `jq` path above), not the header hash or app hash. The `/block` response contains several `"hash"` fields. If you extract it without `jq` on a minimal image, a naive greedy match such as `sed 's/.*"hash":"\([A-F0-9]*\)".*/\1/'` grabs the **last** `"hash"` on the (single-line) JSON — an app/header hash — and the node crash-loops at startup with `expected header's hash X, but got Y`. Without `jq`, extract the first hash explicitly: `grep -oE '"hash":"[A-F0-9]{64}"' | head -1`.
   {% /admonition %}

2. **Update Configuration**:

   - Edit the `~/.exrpd/config/config.toml` file:
     ```
     [statesync]
     enable = true
     rpc_servers = "<rpc_servers>"
     trust_height = <trust_height>
     trust_hash = "<trust_hash>"
     trust_period = "168h0m0s"
     ```
   - Ensure you set `rpc_servers` to trusted providers. There must be at least two servers listed, though duplicates are technically allowed.

   If you used **Option A** above, you can apply the computed values in place:

   ```bash
   CONFIG="$HOME/.exrpd/config/config.toml"
   sed -i.bak \
     -e "/^\[statesync\]/,/^\[/ s|^enable = .*|enable = true|" \
     -e "/^\[statesync\]/,/^\[/ s|^trust_height = .*|trust_height = $TRUST_HEIGHT|" \
     -e "/^\[statesync\]/,/^\[/ s|^trust_hash = .*|trust_hash = \"$TRUST_HASH\"|" \
     "$CONFIG"
   ```

3. **Start the Node**:

   - Run the following command to begin state sync:
     ```bash
     exrpd start
     ```
   - The process may take some time while the node acquires and restores a snapshot. Logs should look similar to:
     ```
     > INF Discovered new snapshot format=1 hash="0x000..." height=8967000 module=statesync
     > INF Fetching snapshot chunk chunk=4 format=1 height=8967000 module=statesync total=45
     > INF Applied snapshot chunk to ABCI app chunk=0 format=1 height=8967000 module=statesync total=45
     ```

4. **Verify Synchronization**:
   - Once state sync completes, the node will begin processing blocks normally. If state sync fails with an error such as `state sync aborted`, first restart `exrpd` and verify your `statesync` settings and trusted RPC endpoints.
   - For non-signing nodes only, you may reset data and retry state sync. Do not run reset commands on active validator signer nodes that hold production `priv_validator_key.json`.
