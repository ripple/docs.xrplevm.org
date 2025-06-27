# Interact with the Smart Contract

Interacting with smart contracts on the **XRPL EVM** allows developers and users to execute contract functions, query data, and create seamless integrations with decentralized applications (dApps). This guide outlines how to connect to the XRPL EVM network, call smart contract functions, and manage responses using Foundry’s **cast** CLI, popular libraries like **web3.js** and **ethers.js**, and user-friendly interfaces such as MetaMask and Remix IDE.


---

## Prerequisites

Before interacting with a smart contract, ensure you have the following set up:

1. **Deployed Smart Contract**

   - Deploy your smart contract using the guide: [Deploy a Smart Contract](./deploy-the-smart-contract.md).
   - Obtain the contract's address after deployment.

2. **XRPL EVM Network**
   - Connect your wallet to the XRPL EVM using MetaMask or other compatible tools.
   - Choose **Mainnet** or **Testnet** details below.

{% tabs %}
{% tab label="Mainnet" %}
**XRPL EVM Network Details**

- **Network Name**: XRPL EVM
- **RPC URL**: `https://rpc.xrplevm.org`
- **Chain ID**: `1440000`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: [https://explorer.xrplevm.org](https://explorer.xrplevm.org)
{% /tab %}
{% tab label="Testnet" %}
**XRPL EVM Testnet Network Details**

- **Network Name**: XRPL EVM Testnet
- **RPC URL**: `https://rpc.testnet.xrplevm.org`
- **Chain ID**: `1449000`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: [https://explorer.testnet.xrplevm.org](https://explorer.testnet.xrplevm.org)
{% /tab %}
{% /tabs %}

3. **Smart Contract ABI**

   - The Application Binary Interface (ABI) is required to interact with the contract. You can find the ABI in your contract’s build files or in Remix IDE.

4. **Wallet Setup**
   - Ensure you have test XRP (`XRP`) for transaction fees. Use the [XRPL EVM Faucet](../../users/faucet.md) to request `XRP` on the appropriate network.

---

## Interaction Methods

### 1. Using Remix IDE

Remix provides a simple way to interact with deployed contracts.

#### Steps:

1. **Connect to XRPL EVM**

   - Open [Remix IDE](https://remix.ethereum.org/).
   - Under the environment settings, select **Injected Web3** to connect MetaMask.
   - Make sure MetaMask is configured for either **XRPL EVM Mainnet** or **Testnet**, depending on which network you’re using.

2. **Load the Contract**

   - Paste the deployed contract’s address in the **Deployed Contracts** section.
   - Import the contract’s ABI if required.

3. **Call Functions**

   - Use the Remix interface to call public or payable functions on your contract.
   - MetaMask will prompt you to sign and confirm transactions.

4. **View Results**
   - The response from the contract is displayed in the Remix console.

---

### 2. Using web3.js

The **web3.js** library provides a programmatic way to interact with smart contracts in JavaScript.

#### Steps:

1. **Install web3.js**

   ```bash
   npm install web3
   ```

2. **Connect to XRPL EVM**

{% tabs %}
{% tab label="Mainnet" %}

```javascript
const Web3 = require("web3");

// XRPL EVM RPC
const web3 = new Web3("https://rpc.xrplevm.org");

// Chain ID = 1440000 (if needed in transaction config)
```

{% /tab %}
{% tab label="Testnet" %}

```javascript
const Web3 = require("web3");

// XRPL EVM Testnet RPC
const web3 = new Web3("https://rpc.testnet.xrplevm.org");

// Chain ID = 1449000 (if needed in transaction config)
```

{% /tab %}
{% /tabs %}

3. **Load the Contract**

   ```javascript
   const contractABI = [
     /* Your Contract ABI */
   ];
   const contractAddress = "0xYourContractAddress";
   const contract = new web3.eth.Contract(contractABI, contractAddress);
   ```

4. **Call Functions**

   ```javascript
   // Example: Reading a public variable
   const result = await contract.methods.publicVariable().call();
   console.log("Result:", result);

   // Example: Sending a transaction
   const receipt = await contract.methods
     .setVariable("New Value")
     .send({ from: "0xYourWalletAddress" });
   console.log("Transaction Receipt:", receipt);
   ```

---

### 3. Using ethers.js

The **ethers.js** library is another popular option for interacting with smart contracts.

#### Steps:

1. **Install ethers.js**

   ```bash
   npm install ethers
   ```

2. **Connect to XRPL EVM**

{% tabs %}
{% tab label="Mainnet" %}

```javascript
const { ethers } = require("ethers");

// XRPL EVM RPC
const provider = new ethers.providers.JsonRpcProvider(
  "https://rpc.xrplevm.org"
);
const wallet = new ethers.Wallet("0xYourPrivateKey", provider);

// Chain ID = 1440000 (if needed in transaction config)
```

{% /tab %}
{% tab label="Testnet" %}

```javascript
const { ethers } = require("ethers");

// XRPL EVM Testnet RPC
const provider = new ethers.providers.JsonRpcProvider(
  "https://rpc.testnet.xrplevm.org"
);
const wallet = new ethers.Wallet("0xYourPrivateKey", provider);

// Chain ID = 1449000 (if needed in transaction config)
```

{% /tab %}
{% /tabs %}

3. **Load the Contract**

   ```javascript
   const contractABI = [
     /* Your Contract ABI */
   ];
   const contractAddress = "0xYourContractAddress";
   const contract = new ethers.Contract(contractAddress, contractABI, wallet);
   ```

4. **Call Functions**

   ```javascript
   // Example: Reading a public variable
   const result = await contract.publicVariable();
   console.log("Result:", result);

   // Example: Sending a transaction
   const tx = await contract.setVariable("New Value");
   const receipt = await tx.wait();
   console.log("Transaction Receipt:", receipt);
   ```

---

### 4. Frontend Interaction

#### Querying XRP/USD Price via the Explorer UI

> **Contract (Testnet-only)**
> **StdReferenceProxy**
> Address: `0x8c064bCf7C0DA3B3b090BAbFE8f3323534D84d68`

1. **Open & Connect**

   * Go to the Testnet Explorer:
     `https://explorer.testnet.xrplevm.org`
   * Search for the StdReferenceProxy address above and open its page.
   * Click **Read/Write Contract** → **Connect** (approve in MetaMask).

2. **Locate `getReferenceData`**

   * In the **Read/Write** panel, click **Expand all** or search for `getReferenceData`.
   * You’ll see inputs for:

     * **base** (string)
     * **quote** (string)

3. **Enter “XRP” and “USD”**

   * In **base**, type: `XRP`
   * In **quote**, type: `USD`

4. **Query the Price**

   * Click **Read**.
   * The explorer will return a struct like:

     ```json
     {
       rate: 2459738388129070878,
       lastUpdatedBase: 1747305613,
       lastUpdatedQuote: 1747305684
     }
     ```
   * **Interpretation:**

     * `rate` is a ¹⁸-decimal–scaled integer.
     * To get the human-readable price, divide by 10¹⁸:

       ```
       2.459738388129070878 USD per XRP
       ```
### 5. Using Foundry’s `cast` CLI

Foundry’s `cast` tool lets you interact with your XRPL EVM contracts directly from the terminal—no JavaScript required. Below is a detailed walkthrough for reading state, sending transactions, decoding logs, estimating gas, and more, on **Testnet**, or **Mainnet**.

#### 5.1 Prerequisites

1. **Foundry Installed**

   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
   ```

2. **Contract Address & ABI**

   ```bash
   CONTRACT_ADDRESS=0xYourContractAddress
   ABI_PATH=out/HelloWorld.json
   ```

3. **Environment Variables** (in `.env`, add to `.gitignore`):

   ```bash
   PRIVATE_KEY=0xYOUR_PRIVATE_KEY

   # Testnet
   RPC_URL_TESTNET=https://rpc.testnet.xrplevm.org
   CHAIN_ID_TESTNET=1449000

   # Mainnet
   RPC_URL_MAINNET=https://rpc.xrplevm.org
   CHAIN_ID_MAINNET=1440000
   ```

   Load them:

   ```bash
   source .env
   ```

---

#### 5.2 Inspect Available Functions

```bash
cast sigs $ABI_PATH

# Example output:
# message()(string)
# setMessage(string)
```

To get a function’s selector:

```bash
cast sig "setMessage(string)"
# → 0x368b8772
```

---

#### 5.3 Reading State (View/Pure)

**Single return**:

```bash
cast call \
  --rpc-url $RPC_URL_TESTNET \
  --to      $CONTRACT_ADDRESS \
  "message()(string)"
```

**Multiple returns**:

```bash
cast call \
  --rpc-url $RPC_URL_MAINNET \
  --to      $CONTRACT_ADDRESS \
  "getStats()(uint256,address)"
```

---

#### 5.4 Sending Transactions

**Simple state change**:

```bash
cast send \
  --rpc-url     $RPC_URL_TESTNET \
  --private-key $PRIVATE_KEY \
  --chain-id    $CHAIN_ID_TESTNET \
  --to          $CONTRACT_ADDRESS \
  "setMessage(string)" "Foundry rocks!"
```

**Wait for mining**:

```bash
cast send \
  --rpc-url     $RPC_URL_TESTNET \
  --private-key $PRIVATE_KEY \
  --chain-id    $CHAIN_ID_TESTNET \
  --to          $CONTRACT_ADDRESS \
  --wait        \
  "setMessage(string)" "Waiting…"
```

---

#### 5.5 Fetching Receipts & Decoding Logs

**Fetch receipt**:

```bash
cast receipt \
  --rpc-url $RPC_URL_TESTNET \
  0xYourTxHash
```

**Decode a logged event** (given `out/HelloWorld.json` contains your ABI):

```bash
cast parse-logs \
  --abi $ABI_PATH \
  receipts.json
```

---

#### 5.6 Gas Estimation & Fees

**Estimate gas**:

```bash
cast estimate-gas \
  --rpc-url $RPC_URL_MAINNET \
  --to      $CONTRACT_ADDRESS \
  "setMessage(string)" "Gas?"
```

**Fetch gas price**:

```bash
cast gas-price --rpc-url $RPC_URL_MAINNET
```

**Calculate max fee**:

```bash
EST=$(cast estimate-gas --rpc-url $RPC_URL_MAINNET --to $CONTRACT_ADDRESS "setMessage(string)" "x")
GP=$(cast gas-price --rpc-url $RPC_URL_MAINNET)
echo "Max fee (wei):" $((EST * GP))
```

---

#### 5.7 Encoding & Decoding Data

* **Get calldata**:

  ```bash
  cast calldata "setMessage(string)" "Raw test"
  ```
* **Decode return data**:

  ```bash
  cast decode "message()(string)" 0xYourHexData
  ```

---

#### 5.8 Chain & Account Utilities

* **Current block**:

  ```bash
  cast block-number --rpc-url $RPC_URL_TESTNET
  ```
* **Chain ID**:

  ```bash
  cast chain-id --rpc-url $RPC_URL_MAINNET
  ```
* **Balance**:

  ```bash
  cast balance --rpc-url $RPC_URL_TESTNET 0xYourWalletAddress
  ```

---

#### 5.9 Sample Workflow

```bash
source .env

# 1) Read initial message on Mainnet
cast call --rpc-url $RPC_URL_MAINNET \
  --to $CONTRACT_ADDRESS "message()(string)"

# 2) Update message on Mainnet
cast send --rpc-url $RPC_URL_MAINNET \
  --private-key $PRIVATE_KEY \
  --chain-id $CHAIN_ID_MAINNET \
  --to $CONTRACT_ADDRESS \
  "setMessage(string)" "Deployed via Foundry"

# 3) Wait & fetch receipt
TX=$(cast send --rpc-url $RPC_URL_MAINNET \
  --private-key $PRIVATE_KEY \
  --chain-id $CHAIN_ID_MAINNET \
  --to $CONTRACT_ADDRESS \
  --wait \
  "setMessage(string)" "Confirmed!")
cast receipt --rpc-url $RPC_URL_MAINNET $TX

# 4) Decode any logs (if emitted)
cast parse-logs --abi $ABI_PATH receipts.json
```

---

## Debugging and Testing

1. **Use the XRPL EVM Explorer**

   * Access the appropriate explorer to view transaction details, logs, and contract interactions:
     * [Mainnet Explorer](https://explorer.xrplevm.org)
     * [Testnet Explorer](https://explorer.testnet.xrplevm.org)
     

2. **Check Gas Fees**

   * Ensure sufficient `XRP` balance in your wallet to cover gas fees for transactions.

3. **Debug Contract Functions**

   * Use the Remix IDE debugger to trace contract execution and identify issues.

---

## Advanced Use Case: Cross-Chain Interactions

The XRPL EVM supports cross-chain functionality via **Axelar General Message Passing (GMP)** and **Cosmos IBC**. These tools allow you to:

* Execute smart contract calls on other EVM or Cosmos chains.
* Transfer assets seamlessly between chains.

### Example

A decentralized voting dApp deployed on the XRPL EVM can fetch real-time voter statistics from Ethereum using Axelar GMP.

---

## Next Steps

Now that you’ve learned how to interact with smart contracts, explore the following guides to enhance your development journey:

* **Crosschain transfers with Axelar**: [Send Tokens](../../developers/making-a-cross-chain-dapp/send-tokens.md)
* **Crosschain messages with Axelar GMP**: [Send Messages](../../developers/making-a-cross-chain-dapp/send-messages.md)

Leverage the XRPL EVM’s low fees, fast transactions, and cross-chain capabilities to build innovative decentralized applications.