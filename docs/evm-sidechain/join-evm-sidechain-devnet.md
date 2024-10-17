---
html: join-evm-sidechain-devnet.html
parent: evm-sidechains.html
blurb: Learn how to join the XRPL EVM Sidechain Devnet.
labels:
  - Development, Interoperability
status: not_enabled
---
# Join the XRPL EVM Sidechain Devnet

<embed src="/snippets/_evm-sidechain-disclaimer.md" />

This tutorial walks you through the steps to join the existing **XRPL EVM Sidechain Devnet**. 

For ease of use, create an alias, `exprd`, to run all commands inside your Docker container.

## XRPL EVM Sidechain Node Hardware Requirements

- Linux/AMD64 operating system.
- 4 or more physical CPU cores.
- At least 500GB of NVME SSD disk storage.
- At least 32GB of RAM.
- At least 100Mbps network bandwidth.

## Installation Methods

### Downloading Raw Binaries

A straightforward method that involves downloading the latest binaries and running them on your system. This method requires minimal prerequisites and is quick to set up.

#### Pre-requisites

- None (Ensure your system meets the hardware requirements).

#### Steps

1. Download the latest binaries from the [official repository](https://github.com/xrplevm/node).
2. Extract the binaries and move them to your preferred directory.
3. Make the binaries executable:
    ```bash
    chmod +x exrpd
    ```

### Building from Source

This method involves cloning the source code repository and building the binaries from source. It is ideal for developers who want to customize the node.

#### Pre-requisites

- `make` and `gcc` installed.

#### Steps

1. Clone the repository:
    ```bash
    git clone https://github.com/xrplevm/node.git
    ```
2. Navigate to the project directory:
    ```bash
    cd node
    ```
3. Build the project:
    ```bash
    make build
    ```
4. The binaries will be available in the `build` directory.

### Using Docker

A containerized approach that ensures the node runs in a consistent environment. This method is highly recommended for users who want to avoid dependency issues.

#### Pre-requisites

- Docker 19+ installed.

#### Steps

1. Pull the Docker image:
    ```bash
    docker pull peersyst/exrp:latest
    ```
2. Create an alias to run all commands inside the Docker container:
    ```bash
    alias exrpd="docker run -it --rm -v ~/.exrpd:/root/.exrpd --entrypoint=\"\" peersyst/exrp:latest exrpd"
    ```

### Using Cosmovisor for automated upgrades

A more advanced method using Cosmovisor, which automates the process of binary management and upgrades.

For detailed steps, refer to [Using Cosmovisor for automated upgrades](automate-upgrades-using-comsovisor.html).


## Initialize Node

The first task is to initialize the node, which creates the necessary validator and node configuration files.

1. Initialize the chain parameters using the following command:

    ```bash
    exrpd config chain-id exrp_1440002-1
    ```

2. Create or add a key to your node. For this tutorial, we use the `test` keyring:

    ```bash
    exrpd keys add <key_name> --keyring-backend test
    ```

   Note the `key_name` you enter as you need to reference it in subsequent steps.

   **Note** For more information on a more secure setup for your validator, refer to [cosmos-sdk keys and keyrings](https://docs.cosmos.network/v0.46/run-node/keyring.html) and [validator security](evm-sidechain-validator-security.md).

3. Initialize the node using the following command:

    ```bash
    exrpd init <your_custom_moniker> --chain-id exrp_1440002-1
    ```

   Monikers can contain only ASCII characters. Using Unicode characters renders your node unreachable.

All these commands create your `~/.exrpd` (i.e `$HOME`) directory with subfolders `config/` and `data/`. In the `config` directory, the most important files for configuration are `app.toml` and `config.toml`.

## Genesis & Seeds

1. Copy the Genesis File.

   Download the `genesis.json` file from here and copy it to the `config` directory: `~/.exrpd/config/genesis.json`. This is a genesis file with the chain-id and genesis accounts balances.

    ```bash
    wget https://raw.githubusercontent.com/Peersyst/xrp-evm-archive/main/poa-devnet/genesis.json -O ~/.exrpd/config/genesis.json
    ```

   :::attention Attention

   Before jumping to the next item, make sure that the contents of the file `~/.exrpd/config/genesis.json` match the contents of the file [genesis.json](https://raw.githubusercontent.com/Peersyst/xrp-evm-archive/main/poa-devnet/genesis.json).

   :::

   Verify the genesis configuration file:

    ```bash
    exrpd validate-genesis
    ```

2. Add Persistent Peer Nodes

   Set the [`persistent_peer`](https://docs.tendermint.com/master/tendermint-core/using-tendermint.html#persistent-peer)s field in `~/.exrpd/config/config.toml` to specify peers with which your node maintains persistent connections. You can retrieve them from the list of available peers on the archive repo ([https://raw.githubusercontent.com/Peersyst/xrp-evm-archive/main/poa-devnet/peers.txt](https://raw.githubusercontent.com/Peersyst/xrp-evm-archive/main/devnet/peers.txt)).

   To get a list of entries from the `peers.txt` file in the `PEERS` variable, run the following command:

    ```bash
    PEERS=`curl -sL https://raw.githubusercontent.com/Peersyst/xrp-evm-archive/main/poa-devnet/peers.txt | sort -R | head -n 10 | awk '{print $1}' | paste -s -d, -`
    ```

   Use `sed` to include them in the configuration. You can also add them manually:

    ```bash
    sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" ~/.exrpd/config/config.toml
    ```

   At this point you should have the list of the peers copied to the `persistent_peers` field in the `config.toml` file. You can verify this by running the following command:

   ```bash
   cat ~/.exrpd/config/config.toml | grep persistent_peers
   ```

## Run and Synchronize Your Node

To run and synchronize your node, follow the instructions in the [Run and Synchronize Your Node](run-and-synchronize-your-node.md) guide.
