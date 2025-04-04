# Deploy the Smart Contract

Deploying a smart contract on the **XRPL Ethereum Virtual Machine (EVM)** sidechain can be accomplished using two popular tools: **Remix IDE** and **Hardhat**. This guide will provide a complete walkthrough for both methods.

---

## Option 1: Deploy Using Remix IDE

Remix is a web-based development environment for smart contracts, perfect for quick deployments and testing.

### Steps:

### 1. Set Up Your Wallet

- **Install MetaMask**  
  If you haven't already, install the [MetaMask wallet](https://metamask.io/) extension in your browser.

- **Connect MetaMask to XRPL EVM**  
  Below are the **Testnet** and **Devnet** RPC details. Choose the appropriate tab for the network you want to use.

{% tabs %}
{% tab label="Testnet" %}
**XRPL EVM Testnet MetaMask Settings**

- **Network Name**: XRPL EVM Testnet
- **RPC URL**: `https://rpc.testnet.xrplevm.org`
- **Chain ID**: `1449000`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: `https://explorer.testnet.xrplevtest.org`

Make sure MetaMask is switched to **XRPL EVM Testnet** before proceeding.
{% /tab %}

{% tab label="Devnet" %}
**XRPL EVM Devnet MetaMask Settings**

- **Network Name**: XRPL EVM Devnet
- **RPC URL**: `https://rpc.devnet.xrplevm.org`
- **Chain ID**: `1440002`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: `https://explorer.devnet.xrplevm.org`

Make sure MetaMask is switched to **XRPL EVM Devnet** before proceeding.
{% /tab %}
{% /tabs %}

### 2. Open Remix IDE

- Navigate to [Remix IDE](https://remix.ethereum.org/).

### 3. Write or Import Your Smart Contract

Write your Solidity smart contract or import an existing one. For example:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HelloWorld {
    string public message;

    constructor(string memory _message) {
        message = _message;
    }

    function setMessage(string memory _message) public {
        message = _message;
    }
}
```

### 4. Compile the Contract

- In Remix, navigate to the **Solidity Compiler** tab.
- Select the appropriate Solidity version and click **Compile**.

### 5. Deploy the Contract

- Go to the **Deploy & Run Transactions** tab.
- Select **Injected Web3** as the environment (this will connect Remix to MetaMask).
- Ensure MetaMask is connected to **XRPL EVM Testnet** or **XRPL EVM Devnet**, whichever you chose above.
- Click **Deploy**, approve the transaction in MetaMask, and wait for the contract to be deployed.

### 6. Verify Deployment

- After deployment, you can interact with the contract directly in Remix or view the deployed contract on the **XRPL EVM Explorer** (Testnet or Devnet) using the contract address.

---

## Option 2: Deploy Using Hardhat

Hardhat is a development framework for Ethereum-compatible smart contracts, ideal for larger projects and automation.

### Steps:

### 1. Set Up Your Development Environment

- **Install Node.js**  
  Download and install [Node.js](https://nodejs.org/).

- **Install Hardhat**  
  Create a new project folder and install Hardhat:

  ```bash
  mkdir my-hardhat-project
  cd my-hardhat-project
  npm init -y
  npm install --save-dev hardhat
  ```

- **Create a Hardhat Project**
  ```bash
  npx hardhat
  ```
  Select "Create a basic sample project" and follow the prompts.

### 2. Configure the Network

To manage sensitive information like RPC URLs and private keys, use a `.env` file.  
Below are tabs for **Testnet** and **Devnet** network details.

#### 2.1 Install dotenv

```bash
npm install dotenv
```

#### 2.2 Create a `.env` File

{% tabs %}
{% tab label="Testnet" %}
Create a file named `.env` with the **XRPL EVM Testnet** details:

```env
XRPL_EVM_URL=https://rpc.testnet.xrplevm.org
PRIVATE_KEY=your_testnet_private_key_here
```

Then in `hardhat.config.js`:

```js
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.24",
  networks: {
    xrplEVM: {
      url: process.env.XRPL_EVM_URL,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
};
```

Ensure your chain ID is set to `1449000` (if you use scripts that explicitly reference it).  
 {% /tab %}

{% tab label="Devnet" %}
Create a file named `.env` with the **XRPL EVM Devnet** details:

```env
XRPL_EVM_URL=https://rpc.devnet.xrplevm.org
PRIVATE_KEY=your_devnet_private_key_here
```

Then in `hardhat.config.js`:

```js
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.24",
  networks: {
    xrplEVM: {
      url: process.env.XRPL_EVM_URL,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
};
```

Ensure your chain ID is set to `1440002` (if you use scripts that explicitly reference it).  
 {% /tab %}
{% /tabs %}

> **Warning**: Never share your private key publicly. Use environment variables to manage sensitive information.

### 3. Write Your Smart Contract

- Create a new file in the `contracts` folder, e.g., `HelloWorld.sol`.
- Write or paste your Solidity smart contract (e.g., the `HelloWorld` contract shown above).

### 4. Compile the Contract

- Run the following command to compile your contract:
  ```bash
  npx hardhat compile
  ```

### 5. Create the Ignition Module

To deploy the contract, you must first create an Ignition module for your `HelloWorld` contract. This module specifies the contract to deploy and its parameters.

1. **Create the Ignition Directory and Module File:**

   ```bash
   mkdir -p ignition/modules && touch ignition/modules/HelloWorld.js
   ```

2. **Define the Module in `HelloWorld.js`:**

   ```js
   const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");

   module.exports = buildModule("HelloWorldModule", (m) => {
     const initialMessage = m.getParameter("initialMessage");
     const helloWorld = m.contract("HelloWorld", [initialMessage]);

     return { helloWorld };
   });
   ```

   - The `initialMessage` parameter will be passed to the `HelloWorld` contract constructor.
   - The contract instance is returned for further use.

---

### 6. Create the Deployment Script

The deployment script handles the asynchronous logic and interacts with the Ignition module.

1. **Create the Script File:**

   ```bash
   mkdir scripts && touch scripts/deploy.js
   ```

2. **Write the Deployment Script:**

   ```js
   const HelloWorldModule = require("../ignition/modules/HelloWorld");

   async function getInitialMessage() {
     // Mock function to simulate an asynchronous operation
     return "Hello, XRPL EVM!";
   }

   async function main() {
     const initialMessage = await getInitialMessage();

     if (initialMessage) {
       const { helloWorld } = await hre.ignition.deploy(HelloWorldModule, {
         parameters: { HelloWorldModule: { initialMessage } },
       });

       console.log(`HelloWorld deployed to: ${await helloWorld.getAddress()}`);

       // Fetch the initial message from the contract
       const message = await helloWorld.message();
       console.log("Initial message in contract:", message);
     } else {
       console.log("Initial message is undefined, skipping deployment.");
     }
   }

   main().catch(console.error);
   ```

   - **`getInitialMessage`** simulates an asynchronous operation (e.g., API call) to fetch the initial message.
   - The script checks if the `initialMessage` is valid before proceeding with deployment.

3. **Run the Deployment Script**

- Deploy the contract using:
  ```bash
  npx hardhat run scripts/deploy.js --network xrplEVM
  ```

### 7. Verify Deployment

- Check the contract address in the terminal output.
- View the deployed contract on the **XRPL EVM Explorer** ([Testnet](https://explorer.testnet.xrplevm.org) or [Devnet](https://explorer.devnet.xrplevm.org)) using the contract address.

---

## Conclusion

Both **Remix IDE** and **Hardhat** provide powerful tools for deploying smart contracts on the XRPL EVM. Remix is ideal for quick testing and learning, while Hardhat offers a robust framework for complex projects. Use the **Testnet** or **Devnet** RPC and chain details where appropriate, and start building innovative decentralized applications on the XRPL EVM sidechain.

---
