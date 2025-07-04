# Deploy the Smart Contract

Deploying a smart contract on the **XRPL EVM** can be accomplished using many different tools; in this guide, we’ll focus on three of the most popular: **Remix IDE**, **Hardhat**, and **Foundry**. Remix IDE provides a web-based sandbox for rapid prototyping and interactive testing; Hardhat offers a full-featured framework with advanced testing, scripting, and plugin support; and Foundry delivers a blazing-fast, CLI-driven workflow for building, testing, and deploying at scale.

This guide will walk you through end-to-end setups for all three methods—covering wallet configuration, network setup, contract authoring, testing, deployment, and on-chain verification—so you can choose the workflow that best fits your project.

---

## Option 1: Deploy Using Remix IDE

Remix is a web-based development environment for smart contracts, perfect for quick deployments and testing.

### Steps:

### 1. Set Up Your Wallet

- **Install MetaMask**  
  If you haven't already, install the [MetaMask wallet](https://metamask.io/) extension in your browser.

- **Connect MetaMask to XRPL EVM**  
  Below are the **Mainnet**, and **Testnet** RPC details. Choose the appropriate tab for the network you want to use.

{% tabs %}
{% tab label="Mainnet" %}
**XRPL EVM MetaMask Settings**

- **Network Name**: XRPL EVM
- **RPC URL**: `https://rpc.xrplevm.org`
- **Chain ID**: `1440000`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: `https://explorer.xrplevm.org`

Make sure MetaMask is switched to **XRPL EVM** before proceeding.
{% /tab %}
{% tab label="Testnet" %}
**XRPL EVM Testnet MetaMask Settings**

- **Network Name**: XRPL EVM Testnet
- **RPC URL**: `https://rpc.testnet.xrplevm.org`
- **Chain ID**: `1449000`
- **Currency Symbol**: `XRP`
- **Block Explorer URL**: `https://explorer.testnet.xrplevm.org`

Make sure MetaMask is switched to **XRPL EVM Testnet** before proceeding.
{% /tab %}
{% /tabs %}

### 2. Open Remix IDE

- Navigate to [Remix IDE](https://remix.ethereum.org/).

### 3. Write or Import Your Smart Contract

Write your Solidity smart contract or import an existing one. For example:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

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
- Ensure MetaMask is connected to the correct network, whichever you chose above.
- Click **Deploy**, approve the transaction in MetaMask, and wait for the contract to be deployed.

### 6. Verify Deployment

- After deployment, you can interact with the contract directly in Remix or view the deployed contract on the **XRPL EVM Explorer** using the contract Read/Write contract information methods.

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
Below are tabs for **Mainnet** and **Testnet** network details.

#### 2.1 Install dotenv

```bash
npm install dotenv
```

#### 2.2 Create a `.env` File

{% tabs %}
{% tab label="Mainnet" %}
Create a file named `.env` with the **XRPL EVM** details:

```env
XRPL_EVM_URL=https://rpc.xrplevm.org
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

Ensure your chain ID is set to `1440000` (if you use scripts that explicitly reference it).  
 {% /tab %}
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
- View the deployed contract on the **XRPL EVM Explorer** ([Mainnet](https://explorer.xrplevm.org), [Testnet](https://explorer.testnet.xrplevm.org) using the contract address.

---

## Option 3: Deploy Using Foundry

Foundry is a blazing-fast CLI toolkit for Ethereum development, featuring `forge` (build, test, deploy) and `cast` (RPC utilities). This option is perfect if you prefer terminal workflows or want to integrate deployment into scripts.

### 1. Install Foundry

```bash
# Download and install Foundry’s tooling
curl -L https://foundry.paradigm.xyz | bash

# Update to the latest version
foundryup
```

---

### 2. Initialize Your Project

```bash
# Create a new project and enter it
forge init my-foundry-project
cd my-foundry-project
```

This scaffolds a `foundry.toml`, `src/`, and `test/` directory.

---

### 3. Configure Networks

Define your RPC endpoints in `foundry.toml` and use a `.env` for secrets.

```toml
# foundry.toml

# Default Solidity version
solc_version = "0.8.24"

[rpc_endpoints]
xrplevm      = "https://rpc.xrplevm.org"
```

Create a `.env` in your project root:

```dotenv
# .env
PRIVATE_KEY=your_private_key_here
RPC_URL=https://rpc.xrplevm.org
CHAIN_ID=1440000                          
```

> **Tip**: Add `.env` to your `.gitignore` to avoid leaking keys.

---

### 4. Write Your Smart Contract

Edit (or create) `src/HelloWorld.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

contract HelloWorld {
    string public message;

    constructor(string memory _message) {
        message = _message;
    }

    function setMessage(string memory _message) external {
        message = _message;
    }
}
```

---

### 5. Create Tests for Your Contract

Foundry tests are written in Solidity under `test/`. Create `test/HelloWorld.t.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

import "forge-std/Test.sol";
import "../src/HelloWorld.sol";

contract HelloWorldTest is Test {
    HelloWorld private hello;

    function setUp() public {
        // Deploy fresh instance before each test
        hello = new HelloWorld("initial");
    }

    function testInitialMessage() public {
        // Verify constructor set the message correctly
        assertEq(hello.message(), "initial");
    }

    function testSetMessage() public {
        // Change the message and verify
        hello.setMessage("updated");
        assertEq(hello.message(), "updated");
    }
}
```

---

### 6. Build and Run Tests

```bash
forge build      # compiles contracts
forge test       # runs your unit tests
```

You should see both tests passing:

```
Running 2 tests for contract test/HelloWorld.t.sol:HelloWorldTest
[PASS] testInitialMessage() (gas XXXX)
[PASS] testSetMessage()    (gas XXXX)
```

---

### 7. Deploy Your Contract

#### 7.1 Quick Deploy with `forge create`

```bash
# Load environment
source .env

forge create src/HelloWorld.sol:HelloWorld \
  --rpc-url     $RPC_URL \
  --private-key $PRIVATE_KEY \
  --chain-id    $CHAIN_ID \
  --constructor-args "Hello, XRPL EVM!"
```

On success, `forge` prints the new contract address.

#### 7.2 Scripted Deploy with `forge script`

1. **Create** `script/Deploy.s.sol`:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity 0.8.24;
   import "forge-std/Script.sol";
   import "../src/HelloWorld.sol";

   contract DeployScript is Script {
       function run() external {
           vm.startBroadcast(vm.envUint("PRIVATE_KEY"));
           new HelloWorld("Hello, XRPL EVM!");
           vm.stopBroadcast();
       }
   }
   ```

2. **Execute** the script:

   ```bash
   source .env

   forge script script/Deploy.s.sol:DeployScript \
     --rpc-url   $RPC_URL \
     --broadcast \
     --chain-id   $CHAIN_ID
   ```

You’ll see broadcast logs and the deployed address in the console.

---

### 8. Verify on Explorer

1. Copy the deployed address from `forge`.
2. Paste into the appropriate XRPL EVM Explorer:
3. Use the Read/Write tabs to call `message()` or `setMessage()`.


## Conclusion

Leverage each tool’s strengths to streamline your development workflow: use **Remix** for rapid prototyping and interactive experimentation, **Hardhat** for full-featured testing, scripting, and plugin support, and **Foundry** for lightning-fast compilation, testing, and CLI-driven deployments. Whichever stack you choose, make sure to manage your RPC endpoints and private keys securely (e.g., via environment variables), rigorously test your contracts on Testnet before moving to Mainnet, and verify your deployments through the XRPL EVM Explorer. With these toolchains in hand, you’re ready to push the boundaries of what’s possible in decentralized finance, NFTs, gaming, and beyond on the XRPL EVM sidechain.


---
