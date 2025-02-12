# Deploy the Smart Contract

Deploying a smart contract on the **XRPL Ethereum Virtual Machine (EVM)** sidechain can be accomplished using two popular tools: **Remix IDE** and **Hardhat**. This guide will provide a complete walkthrough for both methods.

---

## Option 1: Deploy Using Remix IDE

Remix is a web-based development environment for smart contracts, perfect for quick deployments and testing.

### Steps:

### 1. Set Up Your Wallet

- **Install MetaMask**:
  If you haven't already, install the [MetaMask wallet](https://metamask.io/) extension in your browser.
- **Connect to XRPL EVM Sidechain**:
  Add the XRPL EVM network to MetaMask:
  - **Network Name**: XRPL EVM Devnet
  - **RPC URL**: [https://rpc.xrplevm.org](https://rpc.xrplevm.org)
  - **Chain ID**: 1440002
  - **Currency Symbol**: XRP
  - **Block Explorer URL**: [https://explorer.xrplevm.org](https://explorer.xrplevm.org)

### 2. Open Remix IDE

- Navigate to [Remix IDE](https://remix.ethereum.org/).

### 3. Write or Import Your Smart Contract

- Write your Solidity smart contract or import an existing one. Example:

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
- Ensure MetaMask is connected to the XRPL EVM network.
- Click **Deploy**, approve the transaction in MetaMask, and wait for the contract to be deployed.

### 6. Verify Deployment

- After deployment, you can interact with the contract directly in Remix or view the deployed contract on the [XRPL EVM Explorer](https://explorer.xrplevm.org) using the contract address.

---

## Option 2: Deploy Using Hardhat

Hardhat is a development framework for Ethereum-compatible smart contracts, ideal for larger projects and automation.

### Steps:

### 1. Set Up Your Development Environment

- **Install Node.js**:
  Download and install [Node.js](https://nodejs.org/).
- **Install Hardhat**:
  Create a new project folder and install Hardhat:
  ```bash
  mkdir my-hardhat-project
  cd my-hardhat-project
  npm init -y
  npm install --save-dev hardhat
  ```
- **Create a Hardhat Project**:
  ```bash
  npx hardhat
  ```
  Select "Create a basic sample project" and follow the prompts.

### 2. Configure the Network

To manage sensitive information like RPC URLs and private keys, use a `.env` file.

#### 2.1 Install dotenv

```bash
npm install dotenv
```

#### 2.2 Create a `.env` File

- In the root directory of your project, create a file named `.env` and add the following:

  ```env
  XRPL_EVM_URL=https://rpc.xrplevm.org
  PRIVATE_KEY=your_private_key_here
  ```

- Open the `hardhat.config.js` file and add the XRPL EVM network:

  ```javascript
  require("@nomicfoundation/hardhat-toolbox");
  require("dotenv").config();

  module.exports = {
    solidity: "0.8.28",
    networks: {
      xrplEVM: {
        url: process.env.XRPL_EVM_URL,
        accounts: [process.env.PRIVATE_KEY],
      },
    },
  };
  ```

> **Warning**: Never share your private key publicly. Use environment variables to manage sensitive information.

### 3. Write Your Smart Contract

- Create a new file in the `contracts` folder, e.g., `HelloWorld.sol`.
- Write or paste your Solidity smart contract (e.g., the `HelloWorld` contract provided earlier).

### 4. Compile the Contract

- Run the following command to compile your contract:
  ```bash
  npx hardhat compile
  ```

### 5. Create the Ignition Module

To deploy the contract you must first create an Ignition module for your `HelloWorld` contract. This module specifies the contract to deploy and its parameters.

1. **Create the Ignition Directory and Module File:**

   ```bash
   mkdir -p ignition/modules && touch ignition/modules/HelloWorld.js
   ```

2. **Define the Module in `HelloWorld.js`:**

   ```javascript
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

   ```javascript
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
- View the deployed contract on the [XRPL EVM Explorer](https://explorer.xrplevm.org) using the contract address.

---

## Conclusion

Both **Remix IDE** and **Hardhat** provide powerful tools for deploying smart contracts on the XRPL EVM. Remix is ideal for quick testing and learning, while Hardhat offers a robust framework for complex projects. Choose the method that best suits your needs and start building innovative decentralized applications (dApps) on the XRPL EVM Sidechain.
