# Develop a Smart Contract

Smart contracts are self-executing programs that run on blockchain networks, enabling ---

## Next Steps

With the foundational knowledge and tools outlined here, you're ready to:

1. **Write Your First Contract:** [Start](./deploy-the-smart-contract.md) with basic contracts like token creation or a simple voting system.
2. **Build Native dApps:** Focus on the core functionality of your decentralized application using the XRPL EVM Sidechain's performance advantages.
3. **Explore Cross-Chain Options:** When needed, dive into available bridge solutions and [Cosmos IBC](../interacting-with-cosmos/using-ibc.md) for additional functionality.
4. **Contribute to the Ecosystem:** [Share](https://discord.gg/xrplevm) your contracts, tools, and ideas with the growing XRPL EVM Sidechain developer community.

Check out the following guides to continue your journey:

- [Deploy the Smart Contract](./deploy-the-smart-contract.md)
- [Verify the Smart Contract](./verify-the-smart-contract.md)
- [Interact with the Smart Contract](./interact-with-the-smart-contract.md)

On the **XRPL EVM Sidechain**, developers can write, deploy, verify, and interact with Solidity-based smart contracts, leveraging fast finality, low transaction costs, and the complete Ethereum development ecosystem. This page introduces the foundational concepts and steps required to start developing smart contracts on the XRPL EVM Sidechain, aimed at developers looking to build high-performance native dApps.

{% partial file="/snippets/_evm-compatibility-notice.md" /%}

---

## What Are Smart Contracts?

Smart contracts are programs stored on the blockchain that execute automatically when predefined conditions are met. These contracts eliminate the need for intermediaries, making processes transparent, efficient, and secure. With the XRPL EVM Sidechain, you can deploy Ethereum-compatible smart contracts while benefiting from:

- **Fast Transactions:** Blocks every ~5–6 seconds with deterministic finality.
- **Low Fees:** Significantly lower costs compared to Ethereum mainnet.
- **Native Performance:** Run as a standalone Layer 1 blockchain without external dependencies.
- **Optional Interoperability:** Add cross-chain functionality when needed via multiple bridge options and Cosmos IBC.

---

## Technologies You Need

To develop, deploy, and interact with smart contracts on the XRPL EVM Sidechain, you'll need:

1. **Solidity:** The primary programming language for Ethereum smart contracts.
2. **Development Tools:**
   - **Remix IDE** for quick prototyping and testing.
   - **Hardhat** for advanced project management and automation.
3. **Wallet:**
   - **MetaMask** to interact with the XRPL EVM Sidechain network.

---

## Development Workflow

### 1. Write the Smart Contract

Start by learning Solidity, the language used to write smart contracts. Use Remix IDE for simple contracts or Hardhat for more complex projects. Focus on security best practices and efficient gas usage.

### 2. Deploy the Smart Contract

Refer to the [Deploy the Smart Contract](./deploy-the-smart-contract.md) page for a step-by-step guide to deploying your smart contract using Remix or Hardhat. You'll:

- Set up MetaMask to connect to the XRPL EVM Sidechain.
- Compile your contract.
- Deploy the contract on the XRPL EVM Sidechain network.

### 3. Verify the Smart Contract

After deployment, verify your smart contract's source code on the XRPL EVM Explorer. This ensures transparency and allows others to interact with your contract confidently. Follow the guide on [Verify the Smart Contract](./verify-the-smart-contract.md).

### 4. Interact with the Smart Contract

Once deployed and verified, you can interact with your smart contract using libraries like **web3.js** or **ethers.js**. Learn how to:

- Connect to the XRPL EVM Sidechain.
- Call functions from your deployed contract.
- Handle responses programmatically or via a frontend. Details are provided in [Interact with the Smart Contract](./interact-with-the-smart-contract.md).

---

## Future Developments: Cross-Chain Interoperability

The XRPL EVM is not just about isolated smart contract functionality; it’s a gateway to cross-chain innovation. Through Cosmos IBC and Axelar’s advanced tools, you can:

- **Build Cross-Chain dApps:** Enable seamless communication and asset transfers across blockchains.
- **Utilize Axelar GMP:** Execute smart contract calls between XRPL EVM and other EVM/Cosmos chains.
- **Leverage Axelar ITS:** Move assets like tokens and NFTs between chains with ease.

### Example:

A decentralized exchange (DEX) on the XRPL EVM could interact with liquidity pools on Ethereum, Binance Smart Chain, or Cosmos Hub, creating a unified cross-chain trading experience.

---

## Next Steps

With the foundational knowledge and tools outlined here, you’re ready to:

1. **Write Your First Contract:** [Start](./deploy-the-smart-contract.md) small with basic contracts like token creation or a simple voting system.
2. **Explore Cross-Chain Opportunities:** Dive into [Axelar GMP](../../bridge/general-message-passing.md) and [Cosmos IBC](../interacting-with-cosmos/using-ibc.md) for next-gen decentralized applications.
3. **Contribute to the XRPL EVM Ecosystem:** [Share](https://discord.gg/xrplevm) your contracts, tools, and ideas with the growing XRPL EVM developer community.

Check out the following guides to continue your journey:

- [Deploy the Smart Contract](./deploy-the-smart-contract.md)
- [Verify the Smart Contract](./verify-the-smart-contract.md)
- [Interact with the Smart Contract](./interact-with-the-smart-contract.md)
