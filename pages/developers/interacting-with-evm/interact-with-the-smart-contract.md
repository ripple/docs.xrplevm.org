# Interact with the Smart Contract

Interacting with smart contracts on the **XRPL EVM** allows developers and users to execute contract functions, query data, and create seamless integrations with decentralized applications (dApps). This guide outlines how to connect to the XRPL EVM network, call smart contract functions, and manage responses using Foundry’s **cast** CLI, popular libraries like **web3.js** and **ethers.js**, and user-friendly interfaces such as MetaMask and Remix IDE.

{% partial file="/snippets/_evm-compatibility-notice.md" /%}

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
   - Under the environment settings, select **Injected Provider - MetaMask** to connect MetaMask.
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

The web3.js library lets you call smart contracts from JavaScript. The examples below use web3.js v4.

#### Steps:

1. **Install web3.js**

   ```bash
   npm install web3
   ```

2. **Connect to XRPL EVM**

{% tabs %}
{% tab label="Mainnet" %}

```javascript
const { Web3 } = require("web3");

// XRPL EVM RPC
const web3 = new Web3("https://rpc.xrplevm.org");

// Chain ID = 1440000 (if needed in transaction config)
```

{% /tab %}
{% tab label="Testnet" %}

```javascript
const { Web3 } = require("web3");

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
   // Public RPC endpoints hold no accounts, so add a local signer first
   const account = web3.eth.accounts.wallet.add("0xYourPrivateKey")[0];
   const receipt = await contract.methods
     .setVariable("New Value")
     .send({ from: account.address });
   console.log("Transaction Receipt:", receipt);
   ```

---

### 3. Using ethers.js

The ethers.js library is another JavaScript option for calling smart contracts. The examples below use ethers v6.

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
const provider = new ethers.JsonRpcProvider("https://rpc.xrplevm.org");
const wallet = new ethers.Wallet("0xYourPrivateKey", provider);

// Chain ID = 1440000 (if needed in transaction config)
```

{% /tab %}
{% tab label="Testnet" %}

```javascript
const { ethers } = require("ethers");

// XRPL EVM Testnet RPC
const provider = new ethers.JsonRpcProvider("https://rpc.testnet.xrplevm.org");
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

> **Contract (Testnet)** > **StdReferenceProxy** (for the Mainnet address, see [Band Protocol](./use-oracle-data/band-protocol.md))
> Address: `0x8c064bCf7C0DA3B3b090BAbFE8f3323534D84d68`

1. **Open & Connect**

   - Go to the Testnet Explorer:
     `https://explorer.testnet.xrplevm.org`
   - Search for the StdReferenceProxy address above and open its page.
   - Click **Read/Write Contract** → **Connect** (approve in MetaMask).

2. **Locate `getReferenceData`**

   - In the **Read/Write** panel, click **Expand all** or search for `getReferenceData`.
   - You’ll see inputs for:

     - **base** (string)
     - **quote** (string)

3. **Enter “XRP” and “USD”**

   - In **base**, type: `XRP`
   - In **quote**, type: `USD`

4. **Query the Price**

   - Click **Read**.
   - The explorer will return a struct like:

     ```json
     {
       "rate": 2459738388129070878,
       "lastUpdatedBase": 1747305613,
       "lastUpdatedQuote": 1747305684
     }
     ```

   - **Interpretation:**

     - `rate` is an integer scaled to 18 decimals.
     - To get the human-readable price, divide by 10¹⁸:

       ```
       2.459738388129070878 USD per XRP
       ```

### 5. Using Foundry’s `cast` CLI

Foundry’s `cast` tool calls your XRPL EVM contracts from the terminal, with no JavaScript required. The examples below read state, send transactions, decode logs and estimate gas on Testnet or Mainnet.

#### 5.1 Prerequisites

1. **Foundry Installed**

   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
   ```

2. **Contract Address & ABI**

   ```bash
   CONTRACT_ADDRESS=0xYourContractAddress
   ABI_PATH=out/HelloWorld.sol/HelloWorld.json
   ```

3. **Environment Variables** (in `.env`, add to `.gitignore`):

   ```bash
   PRIVATE_KEY=0xYOUR_PRIVATE_KEY

   # Testnet
   RPC_URL_TESTNET=https://rpc.testnet.xrplevm.org

   # Mainnet
   RPC_URL_MAINNET=https://rpc.xrplevm.org
   ```

   Load them:

   ```bash
   source .env
   ```

`cast` takes the contract address, the function signature and its arguments as positional arguments, and reads the chain ID from the RPC endpoint.

---

#### 5.2 Inspect Available Functions

From a Foundry project, list the function selectors:

```bash
forge inspect HelloWorld methodIdentifiers

# | message()          | e21f37ce |
# | setMessage(string) | 368b8772 |
```

Or generate a Solidity interface from the ABI:

```bash
cast interface $ABI_PATH
```

To get a single function’s selector:

```bash
cast sig "setMessage(string)"
# → 0x368b8772
```

---

#### 5.3 Reading State (View/Pure)

**Single return**:

```bash
cast call $CONTRACT_ADDRESS "message()(string)" --rpc-url $RPC_URL_TESTNET
```

**Multiple returns**:

```bash
cast call $CONTRACT_ADDRESS "getStats()(uint256,address)" --rpc-url $RPC_URL_MAINNET
```

---

#### 5.4 Sending Transactions

`cast send` waits for the transaction receipt by default:

```bash
cast send $CONTRACT_ADDRESS "setMessage(string)" "Foundry rocks!" \
  --rpc-url     $RPC_URL_TESTNET \
  --private-key $PRIVATE_KEY
```

To return immediately with the transaction hash instead, add `--async`.

---

#### 5.5 Fetching Receipts & Decoding Logs

**Fetch receipt**:

```bash
cast receipt 0xYourTxHash --rpc-url $RPC_URL_TESTNET
```

**Query and decode events** emitted by a contract (for example, an `event MessageChanged(string newMessage)`):

```bash
cast logs "MessageChanged(string)" \
  --address    $CONTRACT_ADDRESS \
  --from-block <START_BLOCK> \
  --rpc-url    $RPC_URL_TESTNET
```

**Decode a single log's data**:

```bash
cast decode-event --sig "MessageChanged(string)" 0xLogData
```

---

#### 5.6 Gas Estimation & Fees

**Estimate gas**:

```bash
cast estimate $CONTRACT_ADDRESS "setMessage(string)" "Gas?" --rpc-url $RPC_URL_MAINNET
```

**Fetch gas price**:

```bash
cast gas-price --rpc-url $RPC_URL_MAINNET
```

**Calculate max fee**:

```bash
EST=$(cast estimate $CONTRACT_ADDRESS "setMessage(string)" "x" --rpc-url $RPC_URL_MAINNET)
GP=$(cast gas-price --rpc-url $RPC_URL_MAINNET)
echo "Max fee (wei):" $((EST * GP))
```

---

#### 5.7 Encoding & Decoding Data

- **Get calldata**:

  ```bash
  cast calldata "setMessage(string)" "Raw test"
  ```

- **Decode return data**:

  ```bash
  cast decode-abi "message()(string)" 0xYourHexData
  ```

---

#### 5.8 Chain & Account Utilities

- **Current block**:

  ```bash
  cast block-number --rpc-url $RPC_URL_TESTNET
  ```

- **Chain ID**:

  ```bash
  cast chain-id --rpc-url $RPC_URL_MAINNET
  ```

- **Balance**:

  ```bash
  cast balance 0xYourWalletAddress --rpc-url $RPC_URL_TESTNET
  ```

---

#### 5.9 Sample Workflow

```bash
source .env

# 1) Read initial message on Mainnet
cast call $CONTRACT_ADDRESS "message()(string)" --rpc-url $RPC_URL_MAINNET

# 2) Update message on Mainnet (waits for the receipt)
cast send $CONTRACT_ADDRESS "setMessage(string)" "Deployed via Foundry" \
  --rpc-url     $RPC_URL_MAINNET \
  --private-key $PRIVATE_KEY

# 3) Send without waiting, then fetch the receipt
TX=$(cast send $CONTRACT_ADDRESS "setMessage(string)" "Confirmed!" \
  --rpc-url     $RPC_URL_MAINNET \
  --private-key $PRIVATE_KEY \
  --async)
cast receipt $TX --rpc-url $RPC_URL_MAINNET
```

---

## Debugging and Testing

1. **Use the XRPL EVM Explorer**

   - Access the appropriate explorer to view transaction details, logs, and contract interactions:
     - [Mainnet Explorer](https://explorer.xrplevm.org)
     - [Testnet Explorer](https://explorer.testnet.xrplevm.org)

2. **Check Gas Fees**

   - Ensure sufficient `XRP` balance in your wallet to cover gas fees for transactions.

3. **Debug Contract Functions**

   - Use the Remix IDE debugger to trace contract execution and identify issues.

---

## Advanced Use Case: Cross-Chain Interactions

The XRPL EVM supports cross-chain functionality via **Axelar General Message Passing (GMP)** and **Cosmos IBC**. These tools allow you to:

- Execute smart contract calls on other EVM or Cosmos chains.
- Transfer assets seamlessly between chains.

### Example

A decentralized voting dApp deployed on the XRPL EVM can fetch real-time voter statistics from Ethereum using Axelar GMP.

---

## Next Steps

Now that you’ve learned how to interact with smart contracts, explore the following guides to enhance your development journey:

- **Crosschain transfers with Axelar**: [Send Tokens](../interacting-with-evm/advanced-guides/cross-chain-transactions/send-tokens.md)
- **Crosschain messages with Axelar GMP**: [Send Messages](../interacting-with-evm/advanced-guides/cross-chain-transactions/send-messages.md)

Leverage the XRPL EVM’s low fees, fast transactions, and cross-chain capabilities to build innovative decentralized applications.
