# Join the XRPL EVM

Now that you have successfully installed the `exrpd` binary, it’s time to take the next steps and fully connect to the XRPL EVM. This page provides separate instructions for **Mainnet** and **Testnet**. Pick the environment you want to join and follow the steps in that tab.

## Prerequisites

> **Note**: Make sure the [`exrpd` is installed](./installing-the-node.md) before proceeding.

{% tabs %}

{% tab label="Mainnet" %}

## Mainnet Setup

The following guide outlines the steps for any participant to launch or join the **XRPL EVM**.

### 1. Infrastructure Provision

Provision a new machine with stable connectivity and enough resources to run the node. Refer to the XRPL EVM documentation for [minimum specifications](./system-requirements.md).

### 2. Node Installation

Download and install the XRPL EVM node binary using the official instructions:  
[Installing the Node](./installing-the-node.md)

### 3. Configure the Node

Set up the node configuration with the **Mainnet** chain ID:

```bash
exrpd config set client chain-id xrplevm_1440000-1
```

Choose a secure keyring backend:

```bash
exrpd config set client keyring-backend <keyring>
```

Adjust other recommended settings:

- **Pruning**: Default
- **API Access**: Disable all except Tendermint RPC (restrict to localhost)
- **Peer Exchange**: Keep default unless specific network config is required

For more details, refer to the [node configuration documentation](https://docs.xrplevm.org/pages/operators/advanced/node-configuration-options) as well as the [node configuration reference](https://docs.xrplevm.org/pages/operators/resources/configuration-reference).

### 4. Create a New Key

Generate a new validator key:

```bash
exrpd keys add <key_name> --key-type eth_secp256k1
```

**Ensure that you back up the mnemonic securely**. Effective [key management](../validators/managing-keys.md) is critical for the security and operation of validators in the XRPL EVM sidechain.

### 5. Initialize the Node

Initialize your node with a moniker (a unique name for your node/validator) and the **Mainnet** chain ID:

```bash
exrpd init <moniker> --chain-id xrplevm_1440000-1
```

**Ensure that you back up the node keys securely**. [Read more](https://docs.xrplevm.org/pages/operators/validators/managing-keys).

```sh
~/.exrpd/config/node_key.json
~/.exrpd/config/priv_validator_key.json
```

### 6. Download the Genesis File

Fetch the **Mainnet** genesis file:

```sh
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json
```

### 7. Add Seeds to the Node Configuration

Add seeds to the node configuration that will allow connecting to the rest of the node. Modify the file `~/.exrpd/config/config.toml` to include the following seed node:

```sh
PEERS=`curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/mainnet/peers.txt | sort -R | head -n 10 | awk '{print $1}' | paste -s -d, -`
sed -i.bak -e "s/^seeds *=.*/seeds = \"$PEERS\"/" ~/.exrpd/config/config.toml
cat ~/.exrpd/config/config.toml | grep seeds
```

{% /tab %}

{% tab label="Testnet" %}

## Testnet Setup

The following guide outlines the steps for any participant to launch or join the **XRPL EVM Testnet**.

### 1. Infrastructure Provision

Provision a new machine with stable connectivity and enough resources to run the node. Refer to the XRPL EVM documentation for [minimum specifications](./system-requirements.md).

### 2. Node Installation

Download and install the XRPL EVM node binary using the official instructions:  
[Installing the Node](./installing-the-node.md)

### 3. Configure the Node

Set up the node configuration with the **Testnet** chain ID:

```bash
exrpd config set client chain-id xrplevm_1449000-1
```

Choose a secure keyring backend:

```bash
exrpd config set client keyring-backend <keyring>
```

Adjust other recommended settings:

- **Pruning**: Default
- **API Access**: Disable all except Tendermint RPC (restrict to localhost)
- **Peer Exchange**: Keep default unless specific network config is required

For more details, refer to the [node configuration documentation](https://docs.xrplevm.org/pages/operators/advanced/node-configuration-options) as well as the [node configuration reference](https://docs.xrplevm.org/pages/operators/resources/configuration-reference).

### 4. Create a New Key

Generate a new validator key:

```bash
exrpd keys add <key_name> --key-type eth_secp256k1
```

**Ensure that you back up the mnemonic securely**. Effective [key management](../validators/managing-keys.md) is critical for the security and operation of validators in the XRPL EVM sidechain.

### 5. Initialize the Node

Initialize your node with a moniker (a unique name for your node/validator) and the **Testnet** chain ID:

```bash
exrpd init <moniker> --chain-id xrplevm_1449000-1
```

**Ensure that you back up the node keys securely**. [Read more](https://docs.xrplevm.org/pages/operators/validators/managing-keys).

```sh
~/.exrpd/config/node_key.json
~/.exrpd/config/priv_validator_key.json
```

### 6. Download the Genesis File

Fetch the **Testnet** genesis file:

```sh
wget -O ~/.exrpd/config/genesis.json https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json
```

### 7. Add Seeds to the Node Configuration

Add seeds to the node configuration that will allow connecting to the rest of the node. Modify the file `~/.exrpd/config/config.toml` to include the following seed node:

```sh
PEERS=`curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/testnet/peers.txt | sort -R | head -n 10 | awk '{print $1}' | paste -s -d, -`
sed -i.bak -e "s/^seeds *=.*/seeds = \"$PEERS\"/" ~/.exrpd/config/config.toml
cat ~/.exrpd/config/config.toml | grep seeds
```

{% /tab %}
{% /tabs %}

### Start the Node

Once your configuration is complete, start the node using your preferred process manager (e.g., systemd, Docker Compose, or another init system) as outlined in the [Installing the Node](./installing-the-node.md) guide. This ensures the node will run continuously and restart on failure:

```sh
# Example using systemd:
systemctl enable exrpd
systemctl start exrpd

# Or, using Docker Compose:
docker-compose up -d xrplevm-node
````

Monitor the logs to verify syncing:

```sh
journalctl -u exrpd -f      # systemd
# or
docker logs -f xrplevm-node # Docker
```

Once it connects to the majority of validators, blocks will start streaming in and your node will produce data.

### Sync the Node

After your node is configured and started, you need to let it synchronize with the network. There are several methods to bring your node up to the current block height, each balancing speed, resource usage, and historical data retention. Common sync approaches include:

- **Sync from Genesis**: Start from block 0 and replay all transactions (slowest, but fully trustless).
- **Snapshot**: Download a pre-generated snapshot of the chain state for faster setup.
- **State Sync**: Leverage CometBFT’s state sync feature to bootstrap the node from recent application state.

For detailed instructions and best practices on each approach, see the [Sync Options page](../advanced/sync-options.md).

### Validate the Setup

- Run `exrpd status` or check logs to confirm synchronization progress.
- Explore [Interacting With the node CLI](../guides/interacting-with-the-node-cli.md) for more commands.

### Next Steps

Once your node is fully synced, you can:

- [Upgrade your node](../guides/upgrading-your-node.md) when new releases are available.
- Customize your node’s behavior in [Node configuration options](../advanced/node-configuration-options.md).
