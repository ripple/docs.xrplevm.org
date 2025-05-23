# Interact with the Smart Contract

Interacting with smart contracts on the **XRPL Ethereum Virtual Machine (EVM) Sidechain** allows developers and users to execute contract functions, query data, and create seamless integrations with decentralized applications (dApps). This guide outlines how to connect to the XRPL EVM network, call smart contract functions, and manage responses using popular tools like **web3.js**, **ethers.js**, and user-friendly interfaces such as MetaMask and Remix IDE.

---

## Prerequisites

Before interacting with a smart contract, ensure you have the following set up:

1. **Deployed Smart Contract**

   - Deploy your smart contract using the guide: [Deploy a Smart Contract](./deploy-the-smart-contract.md).
   - Obtain the contract's address after deployment.

2. **XRPL EVM Network**
   - Connect your wallet to the XRPL EVM using MetaMask or other compatible tools.
   - Choose **Devnet** or **Testnet** details below.

{% tabs %}
{% tab label="Devnet" %}
**XRPL EVM Devnet Network Details**

- **Network Name**: XRPL EVM Devnet
- **RPC URL**: `https://rpc.devnet.xrplevm.org`
- **Chain ID**: `1440002`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: [https://explorer.devnet.xrplevm.org](https://explorer.devnet.xrplevm.org)
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
   - Make sure MetaMask is configured for either **XRPL EVM Devnet** or **Testnet**, depending on which network you’re using.

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
{% tab label="Devnet" %}

```javascript
const Web3 = require("web3");

// XRPL EVM Devnet RPC
const web3 = new Web3("https://rpc.devnet.xrplevm.org");

// Chain ID = 1440002 (if needed in transaction config)
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
{% tab label="Devnet" %}

```javascript
const { ethers } = require("ethers");

// XRPL EVM Devnet RPC
const provider = new ethers.providers.JsonRpcProvider(
  "https://rpc.devnet.xrplevm.org"
);
const wallet = new ethers.Wallet("0xYourPrivateKey", provider);

// Chain ID = 1440002 (if needed in transaction config)
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

You can also create a frontend to interact with smart contracts using frameworks like **React** or **Vue.js**. Use **ethers.js** or **web3.js** to connect the frontend with the contract.

---

## Debugging and Testing

1. **Use the XRPL EVM Explorer**

   - Access the appropriate explorer (Devnet or Testnet) to view transaction details, logs, and contract interactions:
     - [Devnet Explorer](https://explorer.devnet.xrplevm.org)
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

- **Faucet**: [Obtain test XRP](../../users/faucet.md)
- **Deploy**: [Deploy a Smart Contract](./deploy-the-smart-contract.md)
- **Bridge**: [Using the XRPL EVM Bridge](../../users/using-the-bridge.md)

Leverage the XRPL EVM’s low fees, fast transactions, and cross-chain capabilities to build innovative decentralized applications.
