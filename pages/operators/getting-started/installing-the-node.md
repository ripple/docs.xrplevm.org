# Installing the node

The `exrpd` binary is the primary software component for running XRPL EVM nodes. By installing and configuring `exrpd`, you will be able to join the network, synchronize with the blockchain, and participate in consensus (if you are a validator). The following steps will guide you through the installation process.

## Installation Methods

There are multiple ways to install the `exrpd` binary on your system. Choose the method that best suits your requirements and expertise.

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
test
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
   