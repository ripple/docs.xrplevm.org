# Installing the node

The `exrpd` binary is the primary software component for running XRPL EVM nodes. By installing and configuring `exrpd`, you will be able to join the network, synchronize with the blockchain, and participate in consensus (if you are a validator). The following steps will guide you through the installation process.

## Installation Methods

There are multiple ways to install the `exrpd` binary on your system. Choose the method that best suits your requirements and expertise.

---

## Method 1: Downloading Raw Binaries

This method involves downloading precompiled binaries from the repository's latest release and running them on your system. It’s quick to set up and requires minimal prerequisites.

### Pre-requisites

- **None:** Just ensure your system meets the hardware requirements.

### Steps

1. **Download the Latest Binaries:**  
   Visit the [official repository's releases page](https://github.com/xrplevm/node/releases) and download the binary that corresponds to your operating system and architecture. You can use `wget` (or `curl` for Windows) to download the file directly from the command line.

   {% tabs %}
   {% tab label="Linux" %}
   **For Linux Users:**

   - **AMD64:**  
     ```bash
     wget https://github.com/xrplevm/node/releases/download/v7.0.0/node_7.0.0_Linux_amd64.tar.gz
     ```
   - **ARM64:**  
     ```bash
     wget https://github.com/xrplevm/node/releases/download/v7.0.0/node_7.0.0_Linux_arm64.tar.gz
     ```
   {% /tab %}

   {% tab label="macOS" %}
   **For macOS Users:**

   - **Intel (x86_64):**  
     ```bash
     wget https://github.com/xrplevm/node/releases/download/v7.0.0/node_7.0.0_Darwin_amd64.tar.gz
     ```
   - **Apple Silicon (ARM64):**  
     ```bash
     wget https://github.com/xrplevm/node/releases/download/v7.0.0/node_7.0.0_Darwin_arm64.tar.gz
     ```
   {% /tab %}
   
   {% tab label="Windows" %}
   **For Windows Users:**

   Windows doesn't include `wget` by default. Instead, you can use `curl` (available in Windows 10 and later) to download the file:
   
   - **Download using curl:**  
     Open PowerShell and run:
     ```powershell
     curl -LO https://github.com/xrplevm/node/releases/download/v7.0.0/node_7.0.0_Windows_amd64.zip
     ```
   {% /tab %}
   {% /tabs %}

2. **Extract the Binaries:**  
   Once downloaded, extract the file using the appropriate command for your platform. For example, on Linux:
   ```bash
   tar -xzf node_7.0.0_Linux_amd64.tar.gz
   ```
   This will extract the files into a directory.

3. **Move the Binaries:**  
   Move the extracted files (especially the `exrpd` binary) to your preferred directory. For instance, to place it in `/usr/local/bin` (which is typically in your PATH), run:
   ```bash
   sudo mv /root/bin/exrpd /usr/local/bin/
   ```

4. **Make the Binary Executable:**  
   Ensure the binary has executable permissions:
   ```bash
   chmod +x /usr/local/bin/exrpd
   ```
      - The **exrpd binary** is now located in `/usr/local/bin/exrpd` (if you followed the installation steps).
   - When you run `exrpd` from the terminal, your system finds it in `/usr/local/bin`.

   You can verify its location by running:
   ```bash
   which exrpd
   ```

   Additionally, once you start your node, configuration files (if created) will be placed in your home directory under `~/.exrpd`.

5. **Verify the Installation:**  
   Check that the binary is accessible and working by running:
   ```bash
   exrpd version
   ```
   You should see version information (e.g., `v7.0.0`).

6. **Configure and Run Your Node (Optional):**  
   Once the binary is installed, follow the [node configuration instructions](./join-the-xrplevm.md)


Using this method, you can quickly set up your node by downloading the latest release from the [repository](https://github.com/xrplevm/node/releases).

---

Below is the updated documentation for Methods 2 and 3, now including machine-specific tabs and additional details for Docker regarding optional API/RPC port exposure. For more details on configuration options, please refer to the [Node Configuration Options](../resources/configuration-reference.md) documentation.

---

## Method 2: Building from Source

This method involves cloning the source code repository and building the binaries from source. It is ideal for developers who want to customize the node.

### Pre-requisites

- `go v1.23.4`, `make` and `gcc` installed.

#### Install Dependencies for Building from Source

{% tabs %}
{% tab label="Linux" %}
Install **make** and **gcc**:
```bash
sudo apt-get update
sudo apt-get install build-essential
```
Install **Go v1.23.4**:
```bash
sudo rm -rf /usr/local/go
wget https://go.dev/dl/go1.23.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
source ~/.profile
go version
```
{% /tab %}
{% tab label="macOS" %}
Install Xcode Command Line Tools (includes **make** and **gcc**):
```bash
xcode-select --install
```
Install **Go v1.23.4** using Homebrew:
```bash
brew uninstall go
brew install go@1.23.4
```
Ensure the correct Go binary is in your PATH:
```bash
export PATH="/usr/local/opt/go@1.23.4/bin:$PATH"
go version
```
{% /tab %}
{% tab label="Windows" %}
**Note:** Building from source on Windows directly can be challenging due to the lack of native support for `make` and `gcc`. It is recommended to use [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install) and then follow the Linux instructions.
{% /tab %}
{% /tabs %}

### Steps to Build the Node

1. **Clone the repository:**
   ```bash
   git clone https://github.com/xrplevm/node.git
   ```
2. **Navigate to the project directory:**
   ```bash
   cd node
   ```
3. **Build the project:**
   ```bash
   make build
   ```
4. **Locate the Binaries:**  
   The compiled binaries will be available in the `build` directory.

---

## Method 3: Using Docker

A containerized approach ensures that the node runs in a consistent environment, avoiding dependency issues. This method is highly recommended if you prefer a ready-to-run environment.

**Important:** If the Docker image you’re using does not match the genesis file, which will not unless you are using the first docker image: [peersyst/xrp-evm-blockchain:latest](https://hub.docker.com/layers/peersyst/exrp/latest/images/sha256-dd77f81a2f8e349349fcd1266c465c77e580681764f0abbdd052bd4f4360c24e), you must start the container in interactive mode first to complete the setup steps. This interactive session lets you run the [join-the-xrplevm](join-the-xrplevm.md) instructions (such as `exrpd init`, downloading the correct genesis file, configuring persistent peers, adding keys and syncing) within the container. Once setup is complete, you can restart the node in detached mode.

### Pre-requisites

- [Docker 19+ installed](https://docs.docker.com/engine/install/).

### Steps to Run the Node via Docker

{% tabs %}
{% tab label="Linux" %}

1. **Pull the Docker Image:**

   ```bash
   docker pull peersyst/exrp:latest
   ```

2. **Interactive Setup (if using an image that doesn’t match the genesis):**  
   If the image you pulled isn’t the one that was used to generate the genesis (for example, if you’re using an image with a newer version), you need to set up the node configuration interactively. This allows you to run the necessary join-the-xrplevm steps inside the container. For example:
   ```bash
   docker run -it --name xrplevm-setup \
     -v /home/xrplevmuser/.exrpd:/root/.exrpd \
     -e DAEMON_NAME=exrpd \
     -e DAEMON_HOME=/root/.exrpd \
     peersyst/exrp:latest /bin/sh
   ```
   Complete the [join-the-xrplevm](join-the-xrplevm.md) steps (initialization, genesis file download, peer configuration, add keys and sync) inside the shell and then exit.

3. **Run the Docker Container in Detached Mode:**  
   Once the configuration is complete, you can start the container as a background service:
   ```bash
   docker run -d --name xrplevm-node \
     -v /home/xrplevmuser/.exrpd:/root/.exrpd \
     -e DAEMON_NAME=exrpd \
     -e DAEMON_HOME=/root/.exrpd \
     peersyst/exrp:latest
   ```

4. **Verify the Container:**  
   Run:
   ```bash
   docker ps -a
   ```
   to ensure your container is running.

{% /tab %}
{% tab label="macOS" %}

1. **Pull the Docker Image:**

   ```bash
   docker pull peersyst/exrp:latest
   ```

2. **Interactive Setup (if required):**  
   Replace `<your-username>` with your macOS username:
   ```bash
   docker run -it --name xrplevm-setup \
     -v /Users/<your-username>/.exrpd:/root/.exrpd \
     -e DAEMON_NAME=exrpd \
     -e DAEMON_HOME=/root/.exrpd \
     peersyst/exrp:latest /bin/sh
   ```
   Complete the [join-the-xrplevm](join-the-xrplevm.md) steps (initialization, genesis file download, peer configuration, add keys and sync) inside the shell and then exit.

3. **Run the Docker Container in Detached Mode:**
   ```bash
   docker run -d --name xrplevm-node \
     -v /Users/<your-username>/.exrpd:/root/.exrpd \
     -e DAEMON_NAME=exrpd \
     -e DAEMON_HOME=/root/.exrpd \
     peersyst/exrp:latest
   ```
   4. **Verify the Container:**  
   Run:
   ```bash
   docker ps -a
   ```
   to ensure your container is running.

{% /tab %}
{% tab label="Windows" %}

1. **Pull the Docker Image (via Docker Desktop):**

   ```powershell
   docker pull peersyst/exrp:latest
   ```

2. **Interactive Setup (if required):**  
   Replace `<your-username>` with your Windows username:
   ```powershell
   docker run -it --name xrplevm-setup `
     -v C:\Users\<your-username>\.exrpd:/root/.exrpd `
     -e DAEMON_NAME=exrpd `
     -e DAEMON_HOME=/root/.exrpd `
     peersyst/exrp:latest /bin/sh
   ```
   Complete the [join-the-xrplevm](join-the-xrplevm.md) steps (initialization, genesis file download, peer configuration, add keys and sync) inside the shell and then exit.

3. **Run the Docker Container in Detached Mode:**
   ```powershell
   docker run -d --name xrplevm-node `
     -v C:\Users\<your-username>\.exrpd:/root/.exrpd `
     -e DAEMON_NAME=exrpd `
     -e DAEMON_HOME=/root/.exrpd `
     peersyst/exrp:latest
   ```
   4. **Verify the Container:**  
   Run:
   ```bash
   docker ps -a
   ```
   to ensure your container is running.

{% /tab %}
{% /tabs %}

> **Note:**  
> If you’re using a Docker image that isn’t the first one matching the genesis state (for example, an image that differs from the one used to generate the genesis file), you must run the container in interactive mode to complete the initial setup. Follow the join-the-xrplevm steps inside that session, then restart the node container in detached mode.

Finally, verify that your container is running using:
```bash
docker ps -a
```

---