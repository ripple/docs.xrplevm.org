# Verify a Smart Contract on the XRPL EVM

After deploying your smart contract on the **XRPL EVM**, it is essential to verify the contract source code on the **XRPL EVM Explorer**. Verification ensures transparency and allows users to interact with your contract directly through the explorer.

This guide provides step-by-step instructions for verifying a smart contract using **Remix IDE**, **Hardhat**, and **Foundry**, with a recommended approach of using **Standard JSON Input** for seamless verification.

---

## Step 1: Access the XRPL EVM Explorer

{% tabs %}
{% tab label="Mainnet" %}

1. Open the **XRPL EVM Mainnet Explorer**:
   `https://explorer.xrplevm.org`
2. Search for your deployed contract by **contract address**.
3. Go to the **Contract** tab and click **Verify & Publish**.
{% /tab %}
{% tab label="Testnet" %}
4. Open the **XRPL EVM Testnet Explorer**:
   `https://explorer.testnet.xrplevm.org`
5. Search for your deployed contract by **contract address**.
6. Go to the **Contract** tab and click **Verify & Publish**.
{% /tab %}
{% /tabs %}

---

## Step 2: Select Verification Method

The **XRPL EVM Explorer** supports multiple methods. For the most reliable results, use **Standard JSON Input** exported from Remix, Hardhat, or Foundry.

**Why Standard JSON Input?**

* Captures all compiler settings (version, optimizer, metadata).
* Minimizes bytecode mismatches.

---

## Step 3: Verification with Remix IDE

### Generate Standard JSON Input

1. Open your contract in **Remix IDE** ([remix.ethereum.org](https://remix.ethereum.org)).
2. Compile under **Solidity Compiler**:

   * Match the **compiler version** and **optimizer settings** from deployment.
3. Under **Compilation Details**, copy **COMPILER INPUT**.
4. Save it as `input.json` locally.

### Upload & Verify

1. In the XRPL EVM Explorer’s **Verify & Publish** page, choose **Standard JSON Input**.
2. Upload `input.json`.
3. Select your **License Type**.
4. Click **Verify**.

---

## Step 4: Verification with Hardhat

Two approaches—**Standard JSON Input** and the **Hardhat Verify Plugin**—each work on Mainnet, and Testnet.

### A) Standard JSON Input

1. Compile:

   ```bash
   npx hardhat compile
   ```
2. Locate your JSON at:

   ```
   artifacts/solc-input/standard-input.json
   ```
3. On the **Verify & Publish** page, choose **Standard JSON Input**, upload, pick license, and click **Verify**.

---

### B) Hardhat Verify Plugin

Automate via CLI. Configuration varies per network:

{% tabs %}
{% tab label="Mainnet" %}

```ts
// hardhat.config.ts
import "@nomicfoundation/hardhat-toolbox";
import "@nomicfoundation/hardhat-verify";
import * as dotenv from "dotenv";
dotenv.config();

export default {
  solidity: "0.8.24",
  networks: {
    xrplEVM: {
      url: process.env.XRPL_EVM_URL,    // https://rpc.xrplevm.org
      chainId: 1440000,
      accounts: [process.env.PRIVATE_KEY!],
    },
  },
  etherscan: {
    apiKey: { xrplEVM: "mainnet-key" },
    customChains: [{
      network: "xrplEVM",
      chainId: 1440000,
      urls: {
        apiURL:    "https://explorer.xrplevm.org/api",
        browserURL:"https://explorer.xrplevm.org"
      }
    }]
  }
}
```

{% /tab %}
{% tab label="Testnet" %}

```ts
// hardhat.config.ts (Testnet)
import "@nomicfoundation/hardhat-toolbox";
import "@nomicfoundation/hardhat-verify";
import * as dotenv from "dotenv";
dotenv.config();

export default {
  solidity: "0.8.24",
  networks: {
    xrplEVMTestnet: {
      url: process.env.XRPL_EVM_TESTNET_URL, // https://rpc.testnet.xrplevm.org
      chainId: 1449000,
      accounts: [process.env.PRIVATE_KEY!],
    },
  },
  etherscan: {
    apiKey: { xrplEVMTestnet: "testnet-key" },
    customChains: [{
      network: "xrplEVMTestnet",
      chainId: 1449000,
      urls: {
        apiURL:    "https://explorer.testnet.xrplevm.org/api",
        browserURL:"https://explorer.testnet.xrplevm.org"
      }
    }]
  }
}
```

{% /tab %}
{% /tabs %}

#### Verify via CLI

Replace `<ADDRESS>` and `[args]`:

```bash
# Mainnet
npx hardhat verify --network xrplEVM <ADDRESS> [constructorArg1] …

# Testnet
npx hardhat verify --network xrplEVMTestnet <ADDRESS> …
```

> If no constructor parameters, omit the args. Use `--force` to reverify.

---

## Step 5: Verification with Foundry

Foundry’s `forge` can both generate Standard JSON and invoke on-chain verification via Etherscan-compatible API.

### A) Export Standard JSON Input

1. Build:

   ```bash
   forge build
   ```
2. Generate JSON input:

   ```bash
   forge inspect src/HelloWorld.sol:HelloWorld --pretty-json > input.json
   ```
3. On the Explorer’s **Verify & Publish**, choose **Standard JSON Input**, upload `input.json`, pick license, and click **Verify**.

---

### B) CLI Verification with `forge verify-contract`

Use your `.env` for RPC, key, and chain ID:

```dotenv
# .env
PRIVATE_KEY=0xYOUR_KEY
CHAIN_ID=1440000           # 1440000=Mainnet, 1449000=Testnet
RPC_URL=https://rpc.xrplevm.org  # switch URL per network
```

Run:

```bash
# Install etherscan support if needed:
forge install foundry-rs/forge-etherscan

# Then verify:
forge verify-contract \
  --chain-id $CHAIN_ID \
  --constructor-args "Hello, XRPL EVM!" \
  --etherscan-api-key any-string \
  <DEPLOYED_ADDRESS> \
  HelloWorld \
  $RPC_URL
```

* Replace `<DEPLOYED_ADDRESS>` and `HelloWorld` with your contract’s name.
* For Testnet, set `CHAIN_ID` and `RPC_URL` accordingly.

---

## Additional Tips

* **Compiler Settings**: Match version & optimizer exactly.
* **License**: Choose MIT, GPL, or Unlicense.
* **Explorer Indexing**: Wait \~10–15 s or retry on “no bytecode” errors.
* **Cross-chain Ready**: Plan for Cosmos IBC or Axelar GMP if you need interoperability.

---

## Why Verify?

1. **Transparency:** Users can audit your code on-chain.
2. **Trust:** Builds community confidence.
3. **Interaction:** Enables Read/Write directly in the explorer UI.

By following these steps—across **Mainnet** and **Testnet**, and with **Remix**, **Hardhat**, or **Foundry**—you’ll have a fully verified contract visible and interactable on the XRPL EVM Explorer.
