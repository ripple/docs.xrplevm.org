# Node Configuration Options

This documentation outlines the various configuration options for nodes in the XRPL EVM sidechain project. Configuring a node correctly is essential for ensuring its intended role within the network. Below, we categorize configuration options into historical data storage, API exposure, and peer exchange configurations.

---

## Historical Data based configuration

Nodes can be configured to store historical data in different ways based on their needs:

### Archive or Historical

Stores all historical data from the genesis block. Useful for querying historical state and chain analysis.

Configuration in `app.toml`:

```toml
pruning = "nothing"
```

### Pruned

Stores only the most recent state and minimal historical data to save disk space.

Configuration in `app.toml`:

```toml
pruning = "default"
```

### Full Pruned

Prunes historical data but retains enough for the complete validation of all blocks.

Configuration in `app.toml`:

```toml
pruning = "everything"
```

---

## API based Configuration

Nodes can expose one or more APIs depending on their required functionality:

### Tendermint RPC

Provides Tendermint-specific API for consensus and block queries.

Configuration in `config.toml`:

```toml
[rpc]
laddr = "tcp://0.0.0.0:26657"
```

### Cosmos gRPC

Enables gRPC interface for Cosmos-based interactions.

Configuration in `app.toml`:

```toml
[grpc]
enable = true
address = "0.0.0.0:9090"
```

### Cosmos RPC

Provides Cosmos REST API for interaction with the blockchain.

Configuration in `app.toml`:

```toml
[api]
enable = true
address = "tcp://0.0.0.0:1317"
```

### Ethereum RPC and WS

Exposes Ethereum-compatible JSON-RPC and WS for EVM interaction.

Configuration in `app.toml`:

```toml
[json-rpc]
enable = true
address = "0.0.0.0:8545"
ws-address = "0.0.0.0:8546"
```

---

## Peer Exchange based Configuration

Nodes can be configured for different peer exchange roles:

### Seed Node

Stores a large address book that other peers can use to discover network participants but does not participate in consensus.

Configuration in `config.toml`:

```toml
[p2p]
max_num_inbound_peers = 200
max_num_outbound_peers = 500
pex = true
seed_mode = true
```

### Sentry Node

Acts as an intermediary layer, providing additional security to validators by isolating them from direct peer connections.

Configuration in `config.toml`:

```toml
[p2p]
unconditional_peer_ids = "validator_node_id"
private_peer_ids = "validator_node_id"
addr_book_strict = false
pex = true
```

### Private validator Node

Operates in isolation, connecting only to pre-specified peers.

Configuration in `config.toml`:

```toml
[p2p]
persistent_peers = "sentry_node_ids"
private_peer_ids = ""
double_sign_check_height = 10
pex = false
```

---

## Examples

The following table provides example configurations for different node roles:

| Role            | Historical       | API Configuration          | Peer Exchange |
| --------------- | ---------------- | -------------------------- | ------------- |
| Validator       | Pruned Node      | No API                     | Private       |
| Ethereum API    | Pruned Node      | Ethereum JSON RPC and WS   | Sentry        |
| Cosmos API      | Pruned Node      | Cosmos gRPC and Cosmos RPC | Sentry        |
| Seed Node       | Full Pruned Node | No API                     | Seed          |
| Historical node | Full history     | No API                     | Private       |

---

For further and more advanced configurations, please refer to the [Configuration Reference](../resources/configuration-reference.md).
