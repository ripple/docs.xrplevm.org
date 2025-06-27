# Operators 🤖

## 1. Getting Started

1. **[System Requirements](./getting-started/system-requirements.md)**  
   - **Purpose:** Describes recommended hardware (CPU, RAM, storage) and network specifications.  
   - **Key Topics:** Minimum specs, additional reliability/security considerations, scalability, and resource monitoring.

2. **[Installing the Node](./getting-started/installing-the-node.md)**  
   - **Purpose:** Explains how to install the `exrpd` binary via different methods.  
   - **Subsections:**  
     - **Method 1: Downloading Raw Binaries**  
     - **Method 2: Building from Source**  
     - **Method 3: Using Docker**

3. **[Join the XRPL EVM](./getting-started/join-the-xrplevm.md)**  
   - **Purpose:** Guides you through initializing and connecting your node to the Testnet.  
   - **Subsections:**  
     - **Mainnet Setup**
     - **Testnet Setup**  
     - **Sync the Node** (genesis, snapshot, state sync)  
     - **Validate the Setup** (checking logs/status)

---

## 2. Guides

1. **[Interacting with the Node CLI](./guides/interacting-with-the-node-cli.md)**  
   - **Purpose:** Explains `exrpd` commands/subcommands for querying, broadcasting transactions, managing keys, and more.  
   - **Key Topics:**  
     - Configuration commands (`exrpd config`)  
     - Key management (`exrpd keys`)  
     - Querying on-chain data (`exrpd query`)  
     - Broadcasting transactions (`exrpd tx`)  
     - Checking versions (`exrpd version`)

2. **[Upgrading your Node](./guides/upgrading-your-node.md)**  
   - **Purpose:** Covers chain upgrade proposals and how operators can upgrade to new versions.  
   - **Key Topics:**  
     - On-chain governance process for upgrades  
     - Manual upgrade steps  
     - Automated upgrades with Cosmovisor (setup and usage)

---

## 3. Advanced

1. **[Sync Options](./advanced/sync-options.md)**  
   - **Purpose:** Details different synchronization methods and tradeoffs.  
   - **Subsections:**  
     - Sync from Genesis  
     - Sync from Snapshot  
     - State Sync (CometBFT)

2. **[Node Configuration Options](./advanced/node-configuration-options.md)**  
   - **Purpose:** Explains how to customize node behavior in `app.toml` and `config.toml`.  
   - **Key Topics:**  
     - Historical Data Pruning (archive, pruned, full-pruned)  
     - Exposing APIs (Tendermint RPC, Cosmos gRPC, Cosmos RPC, Ethereum RPC/WS)  
     - Peer Exchange Roles (Seed, Sentry, Private Validator)  
     - Example table for different roles (Validator, Seed, Historical, etc.)

3. **[Monitoring the Node](./advanced/monitoring-the-node.md)**  
   - **Purpose:** Shows how to enable Prometheus and Grafana for observability.  
   - **Key Topics:**  
     - Editing `config.yaml` for Prometheus metrics  
     - Integrating with Grafana (scrape configs, dashboards)  
     - Recommended dashboards

4. **[Adding Horcrux](./advanced/adding-horocrux.md)**  
   - **Purpose:** Explains multi-party computation signing for higher validator security and availability.  
   - **Key Topics:**  
     - Benefits of threshold signing  
     - Installation references

---

## 4. Resources

1. **[Networks](./resources/networks.md)**  
   - **Purpose:** Provides details on XRPL EVM Mainnet and Testnet (chain IDs, genesis files, upgrade info).  
   - **Key Topics:**  
     - Mainnet upgrades  
     - Testnet upgrades
     - Links to official Docker images and GitHub repos

2. **[Snapshots](./resources/snapshots.md)**  
   - **Purpose:** Links to third-party snapshot providers for faster node bootstrapping.  
   - **Key Topics:**  
     - Mainnet/Testnet snapshot sources  
     - Verification and usage notes

3. **[Configuration Reference](./resources/configuration-reference.md)**  
   - **Purpose:** Comprehensive, line-by-line reference for `config.toml`, `app.toml`, and `client.toml`.  
   - **Subsections:**  
     - `config.toml` (CometBFT settings, P2P, RPC, instrumentation)  
     - `app.toml` (Gas pricing, API/gRPC, Rosetta, etc.)  
     - `client.toml` (Client chain ID, keyring backend, node address)

---

## 5. Validators

1. **[Join the Proof of Authority](./validators/join-the-proof-of-authority.md)**  
   - **Purpose:** Explains the governance process for becoming a validator on the XRPL EVM.  
   - **Key Topics:**  
     - Submitting your validator operator address and public key  
     - Discord introduction process  
     - Proposal creation and voting timeline

2. **[Managing Keys](./validators/managing-keys.md)**  
   - **Purpose:** Explains the difference between node keys and operator keys, plus best practices for security.  
   - **Key Topics:**  
     - Using `exrpd` key commands with OS/file/test keyrings  
     - Backing up and restoring keys  
     - External wallets vs. local keyring

3. **[Validator Security](./validators/validator-security.md)**  
   - **Purpose:** Security hardening tips to protect validator infrastructure.  
   - **Key Topics:**  
     - Hosting environment (redundant power, networking, physical access controls)  
     - System security (OS patches, minimal services, firewall, SSH hardening)  
     - Infrastructure (monitoring & alerts, hot standby, sentry nodes)  
     - Key management with Horcrux or distributed signing

4. **[Maintaining the Validator](./validators/maintaining-the-validator.md)**  
   - **Purpose:** Practical steps for day-to-day validator operations and status checks.  
   - **Key Topics:**  
     - Editing validator description (moniker, commission, identity)  
     - Viewing validator info and signing data  
     - Unjailing a validator  
     - Confirming validator is active  
     - Halting the node for maintenance

---

Use this TOC to quickly find topics relevant to running XRPL EVM infrastructure, including node setup, configuration, validator management, and security best practices.