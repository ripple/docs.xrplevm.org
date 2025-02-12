# Sync options

The XRPL EVM sidechain supports various synchronization methods for setting up and running the `exrpd` binary. These options allow node operators to choose the best method for their use case, depending on factors such as the desired setup time, available resources, and the level of historical data required. Below, we detail the three main synchronization options and provide step-by-step instructions for each.

## Sync from Genesis

Syncing from genesis initializes your `exrpd` node from the very beginning of the XRPL EVM blockchain's history. This method is comprehensive but time-intensive, as it downloads and verifies all blocks and transactions since the chain's inception.

1. **Install `exprd`**: Ensure that the first version of the `exrpd` binary is installed and configured on your system. Refer to the [Networks page](../resources/networks.md) for downloading the first version of the binary for each network and to the [Installation Guide](../getting-started/installing-the-node.md) for detailed setup steps.

2. **Start the Node**:

```bash
exrpd start
```

3. **Monitor Progress**:
   Wait for the node to sync until it reaches the block of the next version. Then, upgrade to the newer version and repeat this process until the node is fully in sync with the network. For detailed upgrade instructions, refer to the [Upgrading your node](../guides/upgrading-your-node.md) page.

## Sync from Snapshot

Syncing from a snapshot significantly reduces setup time by initializing your node using a pre-generated state file. The snapshot is downloaded and then decompressed to the ~/.exrpd/data directory, preparing the node for quick synchronization with the network.

1. **Select a Snapshot**: Choose a snapshot file from a trusted provider listed on the Snapshots page.

2. **Download and decompress the snapshot**:

```bash
wget <snapshot_url> -O ~/.exrpd
cd ~/.exrpd
tar -xI lz4 -f exrpd.tar.lz4
```

3. **Start the Node**:

```bash
exrpd start
```

## State Sync

State Sync is an efficient and fast way to bootstrap a new node. It works by replaying larger chunks of application state directly rather than replaying individual blocks or consensus rounds. For more information, see [CometBFT's State Sync docs](https://docs.cometbft.com/).

1. **Retrieve a Recent Block Height and Hash**:

   - Visit a blockchain explorer to get a recent block height and corresponding hash.
   - Choose any height/hash in the current bonding period. It is recommended to select a height close to `current height - 1000` to align with the snapshot period.

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
   - Once state sync completes, the node will begin processing blocks normally. If state sync fails with an error such as `state sync aborted`, try restarting `exrpd` or running `exrpd unsafe-reset-all` (ensure you back up any configuration or history before using this command).
