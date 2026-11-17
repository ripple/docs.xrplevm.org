# How to Deploy a Subgraph with Goldsky CLI

Goldsky makes it easy to index smart contracts with a powerful no-code subgraph configuration system. Using the interactive CLI wizard, you can deploy a fully working subgraph in minutes.

> **Prerequisite**
> Make sure you’re authenticated via:
>
> ```bash
> goldsky login
> ```

---

## Step-by-Step Guide to Deploy a Subgraph

### 1. Launch the Wizard

Start the Goldsky wizard:

```bash
goldsky subgraph init
```

---

### 2. Set Subgraph Name and Version

You’ll be prompted to enter a name and version:

```
Subgraph name: my-xrplevm-subgraph
Subgraph version: 1.0.0
```

---

### 3. Define Target Path

Set the target path where all configuration files will be saved:

```
Subgraph path: ~/my-subgraphs/my-xrplevm-subgraph/1.0.0
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
Contract address: <your-contract-address>
```

Select the network: choose **XRPL EVM** (Mainnet) or **XRPL EVM Testnet** from the list the wizard shows.

Set the start block (autodetected or manually entered). Use the block in which your contract was deployed, which you can find on the [XRPL EVM Explorer](https://explorer.xrplevm.org):

```
Start block: <deployment-block>
```

> **For XRPL EVM**
> Goldsky supports subgraphs on XRPL EVM Mainnet and Testnet. See Goldsky's [XRPL EVM Sidechain page](https://docs.goldsky.com/chains/xrpl-evm).

---

### 6. Name the Contract

Provide a human-readable name:

```
Contract name: MyContract
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

- **Dashboard:**
  `https://app.goldsky.com/.../dashboard/subgraphs/my-xrplevm-subgraph/1.0.0`

- **GraphiQL API:**
  `https://api.goldsky.com/api/public/.../subgraphs/my-xrplevm-subgraph/1.0.0/gn`

---

## Query Your Subgraph

After deployment, test your subgraph using GraphiQL:

Entity names follow your contract's events. For example, if your contract emits an ERC-20 `Transfer` event:

```graphql
query LatestTransfers($count: Int = 5) {
  transfers(first: $count, orderBy: block_number, orderDirection: desc) {
    id
    block_number
    transactionHash_
    timestamp_
    from
    to
    value
  }
}
```

---

## Non-Interactive Deployment (CI/CD)

To automate deployment (e.g., for CI pipelines):

```bash
goldsky subgraph init my-xrplevm-subgraph/1.0.0 \
  --contract <your-contract-address> \
  --network <xrpl-evm-network> \
  --start-block <deployment-block> \
  --contract-name MyContract \
  --call-handlers \
  --deploy
```

Replace `<xrpl-evm-network>` with the XRPL EVM network name exactly as the interactive wizard lists it.

---

## Useful CLI Flags

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

## Resources

- [XRPL EVM Sidechain on Goldsky](https://docs.goldsky.com/chains/xrpl-evm)
- [Supported Networks (Goldsky Docs)](https://docs.goldsky.com/chains/supported-networks)
- [Full Goldsky Documentation](https://docs.goldsky.com)

---
