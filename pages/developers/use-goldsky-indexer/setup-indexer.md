Here’s the fully rewritten `.md` file with real data from the official Goldsky documentation and the XRPL EVM-specific note/link added:

---

# How to Deploy a Subgraph with Goldsky CLI

Goldsky makes it easy to index smart contracts with a powerful no-code subgraph configuration system. Using the interactive CLI wizard, you can deploy a fully working subgraph in minutes.

> 🛠 **Prerequisite**
> Make sure you’re authenticated via:
>
> ```bash
> goldsky login
> ```

---

## 🚀 Step-by-Step Guide to Deploy a Subgraph

### 1. Launch the Wizard

Start the Goldsky wizard:

```bash
goldsky subgraph init
```

---

### 2. Set Subgraph Name and Version

You’ll be prompted to enter a name and version:

```
Subgraph name: nouns-demo
Subgraph version: 1.0.0-demo+docs
```

---

### 3. Define Target Path

Set the target path where all configuration files will be saved:

```
Subgraph path: ~/my-subgraphs/nouns-demo/1.0.0-demo+docs
```

---

### 4. (Optional) Set ABI Source

You may skip this step to allow Goldsky to fetch ABI files automatically:

```
Contract ABI source: (leave blank)
```

---

### 5. Enter Contract Details

Provide the smart contract address:

```
Contract address: 0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03
```

Select the network:

```
Contract network: mainnet
```

Set the start block (autodetected or manually entered):

```
Start block: 12985438
```

> ℹ️ **For XRPL EVM**
> See [Supported Networks](https://docs.goldsky.com/chains/supported-networks) for correct network IDs including XRPL Mainnet and Testnet.

---

### 6. Name the Contract

Provide a human-readable name:

```
Contract name: NOUNS
```

---

### 7. Enable Call Handlers (Optional)

To also index function calls:

```
Enable subgraph call handlers? Yes
```

---

### 8. Confirm & Initialize

Review your setup:

```
Proceed with subgraph initialization? Yes
Proceed with subgraph build? Yes
```

---

### 9. Deploy the Subgraph

Deploy to Goldsky’s indexing service:

```
Proceed with subgraph deploy? Yes
```

You’ll receive links like:

* **Dashboard:**
  `https://app.goldsky.com/.../dashboard/subgraphs/nouns-demo-mainnet/1.0.0-demo+docs`

* **GraphiQL API:**
  `https://api.goldsky.com/api/public/.../subgraphs/nouns-demo-mainnet/1.0.0-demo+docs/gn`

---

## 🔍 Query Your Subgraph

After deployment, test your subgraph using GraphiQL:

```graphql
query LatestNouns($count: Int = 5) {
  nounCreateds(first: $count, orderBy: tokenId, orderDirection: desc) {
    id
    block_number
    transactionHash_
    timestamp_
    tokenId
    seed_background
    seed_body
    seed_accessory
    seed_head
    seed_glasses
  }
}
```

---

## ⚙️ Non-Interactive Deployment (CI/CD)

To automate deployment (e.g., for CI pipelines):

```bash
goldsky subgraph init nouns-demo/1.0.0 \
  --contract 0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03 \
  --network mainnet \
  --start-block 12985438 \
  --contract-name NOUNS \
  --call-handlers \
  --deploy
```

---

## 🧩 Useful CLI Flags

| Flag              | Description                               |
| ----------------- | ----------------------------------------- |
| `--target-path`   | Set a custom path for the subgraph config |
| `--force`         | Overwrite existing files                  |
| `--from-config`   | Use existing config as a template         |
| `--abi`           | Provide local ABI source                  |
| `--contract`      | Contract address(es) to index             |
| `--network`       | Network(s) to index                       |
| `--start-block`   | Block number to start indexing from       |
| `--contract-name` | Set custom contract name                  |
| `--description`   | Set subgraph description                  |
| `--call-handlers` | Enable indexing of function calls         |
| `--build`         | Trigger build step automatically          |
| `--deploy`        | Trigger deployment after build            |

---

## 🔗 Resources

* 🧾 [Supported Networks – Goldsky Docs](https://docs.goldsky.com/chains/supported-networks)
* 📚 [Full Goldsky Documentation](https://docs.goldsky.com)

---

Let me know if you want an XRPL EVM–specific version with an example subgraph using one of your contracts!
