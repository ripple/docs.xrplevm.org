# Verify a Smart Contract on the XRPL EVM

After deploying your smart contract on the **XRPL EVM**, it is essential to verify the contract source code on the **XRPL EVM Explorer**. Verification ensures transparency and allows users to interact with your contract directly through the explorer.

This guide provides step-by-step instructions for verifying a smart contract using **Remix IDE**, **Hardhat**, and **Foundry**, with a recommended approach of using **Standard JSON Input** for seamless verification.

{% partial file="/snippets/_evm-compatibility-notice.md" /%}

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

Two approaches work on both Mainnet and Testnet: **Standard JSON Input** and the **Hardhat Verify Plugin**.

### A) Standard JSON Input

1. Compile:

   ```bash
   npx hardhat compile
   ```
2. Hardhat writes the compiler input to a build-info file:

   ```
   artifacts/build-info/<hash>.json
   ```

   The Standard JSON Input is the `input` field of that file. Extract it with:

   ```bash
   jq .input artifacts/build-info/<hash>.json > standard-input.json
   ```
3. On the **Verify & Publish** page, choose **Standard JSON Input**, upload `standard-input.json`, pick license, and click **Verify**.

---

### B) Hardhat Verify Plugin

Automate via CLI. The examples below use Hardhat 2 (`hardhat@hh2`), where `@nomicfoundation/hardhat-toolbox@hh2` already includes the verify plugin. They extend the `hardhat.config.js` and `.env` (`XRPL_EVM_URL`, `PRIVATE_KEY`) from [Deploy the Smart Contract](./deploy-the-smart-contract.md), keeping the same `xrplEVM` network name. The explorer runs Blockscout, which accepts any non-empty API key.

{% tabs %}
{% tab label="Mainnet" %}

```js
// hardhat.config.js
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.24",
  networks: {
    xrplEVM: {
      url: process.env.XRPL_EVM_URL, // set in .env, see Deploy the Smart Contract
      chainId: 1440000,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
  etherscan: {
    apiKey: { xrplEVM: "blockscout" },
    customChains: [
      {
        network: "xrplEVM",
        chainId: 1440000,
        urls: {
          apiURL: "https://explorer.xrplevm.org/api",
          browserURL: "https://explorer.xrplevm.org",
        },
      },
    ],
  },
};
```

{% /tab %}
{% tab label="Testnet" %}

```js
// hardhat.config.js
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.24",
  networks: {
    xrplEVM: {
      url: process.env.XRPL_EVM_URL, // set in .env, see Deploy the Smart Contract
      chainId: 1449000,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
  etherscan: {
    apiKey: { xrplEVM: "blockscout" },
    customChains: [
      {
        network: "xrplEVM",
        chainId: 1449000,
        urls: {
          apiURL: "https://explorer.testnet.xrplevm.org/api",
          browserURL: "https://explorer.testnet.xrplevm.org",
        },
      },
    ],
  },
};
```

{% /tab %}
{% /tabs %}

#### Verify via CLI

Replace `<ADDRESS>` and the constructor arguments. The command is the same on both networks; the target is set by `XRPL_EVM_URL` and the config above:

```bash
npx hardhat verify --network xrplEVM <ADDRESS> [constructorArg1] …
```

> If no constructor parameters, omit the args. Use `--force` to reverify.

---

## Step 5: Verification with Foundry

Foundry’s `forge` can both generate Standard JSON and verify directly against the explorer, which runs Blockscout.

### A) Export Standard JSON Input

1. Build:

   ```bash
   forge build
   ```
2. Generate JSON input:

   ```bash
   forge inspect src/HelloWorld.sol:HelloWorld standardJson > input.json
   ```
3. On the Explorer’s **Verify & Publish**, choose **Standard JSON Input**, upload `input.json`, pick license, and click **Verify**.

---

### B) CLI Verification with `forge verify-contract`

No plugin is needed. Constructor arguments must be ABI-encoded, so pass them through `cast abi-encode`:

```bash
# Mainnet
forge verify-contract \
  --rpc-url https://rpc.xrplevm.org \
  --verifier blockscout \
  --verifier-url https://explorer.xrplevm.org/api/ \
  --constructor-args $(cast abi-encode "constructor(string)" "Hello, XRPL EVM!") \
  <DEPLOYED_ADDRESS> \
  src/HelloWorld.sol:HelloWorld
```

* Replace `<DEPLOYED_ADDRESS>` with your contract address and `src/HelloWorld.sol:HelloWorld` with your contract path and name.
* For Testnet, use `--rpc-url https://rpc.testnet.xrplevm.org` and `--verifier-url https://explorer.testnet.xrplevm.org/api/`.
* For Devnet, use `--rpc-url https://rpc.devnet.xrplevm.org` and `--verifier-url https://explorer.devnet.xrplevm.org/api/`.
* If the contract has no constructor arguments, drop `--constructor-args`.

---

## Additional Tips

* **Compiler Settings**: Match version & optimizer exactly.
* **License**: Choose MIT, GPL, or Unlicense.
* **Explorer Indexing**: Wait about 10 to 15 seconds or retry on “no bytecode” errors.
* **Cross-chain Ready**: Plan for Axelar GMP, Wormhole or Cosmos IBC if you need interoperability.

---

## Why Verify?

1. **Transparency:** Users can audit your code on-chain.
2. **Trust:** Builds community confidence.
3. **Interaction:** Enables Read/Write directly in the explorer UI.

By following these steps across **Mainnet** and **Testnet**, with **Remix**, **Hardhat**, or **Foundry**, you’ll have a fully verified contract visible and interactable on the XRPL EVM Explorer.
