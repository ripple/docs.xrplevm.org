# Installing the Node

The `exrpd` binary is the cornerstone of running an XRPL EVM node. It enables your machine to communicate with the XRPL EVM network, synchronize with the blockchain, and—if configured as a validator—actively participate in consensus. This guide walks you through the process of installing and configuring `exrpd` so you can join the network effectively.

Before proceeding, it’s crucial to understand that node versioning plays a vital role in how you synchronize with the blockchain—especially if you're starting from genesis.

 The XRPL EVM (Mainnet) initially launched using `exrpd` version 7. At block **497,000**, it upgraded to version 8.0.2. If you intend to sync your node from the very beginning of the chain (i.e., from genesis), you **must** install the same node version used at the network’s genesis—**v7**. Your node will sync until it reaches the block where a version upgrade occurred. At that point, you must manually upgrade your node to the corresponding version (e.g., from v7 to v8.0.2 at block 497,000 to continue syncing without interruption.)

 The XRPL EVM Testnet initially launched using `exrpd` version 6. At block **547,100**, it upgraded to version 7, then to version 8 at block **1,485,600** and later to v9.0.0 at block **3827000**. If you intend to sync your node from the very beginning of the chain (i.e., from genesis), you **must** install the same node version used at the network’s genesis—**v6**. Your node will sync until it reaches the block where a version upgrade occurred. At that point, you must manually upgrade your node to the corresponding version (e.g., from v6 to v7 at block 547,100, from v7 to v8 at block 1,485,600 and from v8 to v9 at block 3827000) to continue syncing without interruption.

Alternatively, if syncing from genesis is not required, you can take a more efficient approach by starting from [**sync from snapshot**](https://docs.xrplevm.org/pages/operators/advanced/sync-options#sync-from-snapshot) or using [**sync from state sync**](https://docs.xrplevm.org/pages/operators/advanced/sync-options#state-sync), which allows you to join the network at a later state. In this case, you can install the [**latest version**](../resources/networks.md) of `exrpd` and bypass the need for version hopping altogether.

{% admonition type="info" name="List of upgrades" %}
For a detailed list of XRPL EVM network versions—including timestamps, upgrade blocks, and version changes across all supported chains—refer to the official network documentation: [XRPL EVM Networks Overview](../resources/networks.md).
{% /admonition %}

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

A containerized approach ensures a consistent environment and avoids host-dependency issues. With version **v7.0.0**, you only need **two** Docker commands:

### Prerequisites

* Docker 19+ installed on your host.
* Run as root (or via sudo) so that `/root/.exrpd` is writable.

### 1. Interactive setup (one-time)

Launch a shell in the container, mounting your host’s config directory. Inside, run **all** of the “Join the XRPL EVM” commands (chain-ID, keygen, init, genesis download, seed configuration, etc.) as per the Mainnet or Testnet guide—then exit when done.

```bash
docker run -it --name xrplevm-setup \
  -v /root/.exrpd:/root/.exrpd \
  peersyst/exrp:7.0.0 \
  /bin/sh
```

*(Inside that shell, complete the join-the-xrplevm steps from the docs, then `exit`.)*

---

### 2. Detached, auto-restarting run

Now start your fully-configured node in the background. The `--entrypoint` override ensures that `exrpd start` actually runs:

```bash
docker run -d \
  --restart unless-stopped \
  --name xrplevm-node \
  -v /root/.exrpd:/root/.exrpd \
  --entrypoint exrpd \
  peersyst/exrp:v7.0.0 \
  start
```

---

### Verification

```bash
docker ps | grep xrplevm-node
docker logs -f xrplevm-node
```

You should see your node’s Tendermint/exrpd startup logs and syncing progress.

### Upgrade the node

To upgrade your running XRPL EVM node from v7.0.0 to v8.0.2 in Docker, you just need to pull the new image, stop & remove the old container, and re-run it with the same volume mount. Here’s a concise step‐by‐step:

1. **Pull the v8.0.2 image**

   ```bash
   docker pull peersyst/exrp:v8.0.2
   ```

2. **Stop and remove your old container**

   ```bash
   docker stop xrplevm-node
   docker rm xrplevm-node
   ```

3. **Run the container with the new tag**

   ```bash
   docker run -d \
     --restart unless-stopped \
     --name xrplevm-node \
     -v /root/.exrpd:/root/.exrpd \
     --entrypoint exrpd \
     peersyst/exrp:v8.0.2 \
     start
   ```

   * You’re re-using the same `/root/.exrpd` data directory, so your ledger state stays intact.
   * The `--entrypoint exrpd … start` invocation is exactly the same as before, just pointing to the new image.

4. **Verify it’s running and has upgraded**

   ```bash
   docker logs -f --tail 50 xrplevm-node
   ```

   You should no longer see the `UPGRADE "v8.0.2" NEEDED at height` error and your node will proceed to sync/replay under the new binary.

---

### If you’re using Docker Compose

If you prefer `docker-compose.yml`, just change the image tag and do a `docker-compose up -d`:

```yaml
version: '3.8'
services:
  xrplevm-node:
    image: peersyst/exrp:v8.0.2
    container_name: xrplevm-node
    entrypoint: ["exrpd", "start"]
    restart: unless-stopped
    volumes:
      - /root/.exrpd:/root/.exrpd
```

Then:

```bash
docker-compose pull xrplevm-node
docker-compose up -d xrplevm-node
```

That’s it—your node will now run v8.0.2 and continue syncing from height 547100 onward.

Do the same with future versions when they launch.

If you don't want to sync from genesis you can also install the v8.0.2 directly and [**sync from snapshot**](https://docs.xrplevm.org/pages/operators/advanced/sync-options#sync-from-snapshot) or [**sync from state sync**](https://docs.xrplevm.org/pages/operators/advanced/sync-options#state-sync).

