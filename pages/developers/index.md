# Developers 💻

Welcome to developing on the **XRPL EVM Sidechain**! This guide will help you build decentralized applications on a high-performance blockchain that combines the best of both worlds.

## What is the XRPL EVM Sidechain?

The XRPL EVM Sidechain is a **Cosmos SDK-based blockchain** that provides full support for **Ethereum Virtual Machine (EVM) smart contracts**. This unique architecture gives you:

- **EVM Compatibility**: Deploy Solidity smart contracts with full Ethereum tooling support
- **Cosmos SDK Foundation**: Benefit from fast finality, low fees, and modular architecture
- **Interoperability**: Native support for IBC (Inter-Blockchain Communication) and cross-chain bridges

{% partial file="/snippets/_evm-compatibility-notice.md" /%}

## 1. Interacting with EVM

Build and deploy Ethereum-compatible smart contracts on the XRPL EVM Sidechain:

### Smart Contract Development
1. **[Develop a Smart Contract](./interacting-with-evm/develop-a-smart-contract.md)**
   Learn the fundamentals of smart contract development on XRPL EVM, including Solidity best practices and development tools.

2. **[Deploy the Smart Contract](./interacting-with-evm/deploy-the-smart-contract.md)**
   Step-by-step deployment guide using Remix IDE, Hardhat, or Foundry.

3. **[Verify the Smart Contract](./interacting-with-evm/verify-the-smart-contract.md)**
   Learn how to verify your contract source code on the XRPL EVM Explorer for transparency and trust.

4. **[Interact with the Smart Contract](./interacting-with-evm/interact-with-the-smart-contract.md)**
   Call contract functions and integrate using web3.js, ethers.js, or other Ethereum tools.

5. **[Next Steps](./interacting-with-evm/next-steps.md)**
   Explore advanced topics and full-stack dApp development patterns.

### Data Indexing
**[Use Goldsky Indexer](./interacting-with-evm/use-goldsky-indexer/goldsky-overview.md)**
Set up real-time data indexing for your smart contracts, query events with GraphQL, and implement best practices for efficient data access.

### Oracle Integration
**[Use Band Protocol](./interacting-with-evm/use-oracle-data/band-protocol.md)**
Integrate Band Protocol's oracle contracts to fetch real-time price data on-chain for assets like XRP, BTC, ETH, and more.

### Advanced Guides

#### Native XRP Integration
- **[Using XRP as Wrapped ERC-20](./interacting-with-evm/advanced-guides/using-xrp-as-wrapped-erc20.md)**
  Discover why you don't need to wrap XRP on XRPL EVM and how to work with the native 18-decimal ERC-20 at `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`.

#### Domain Resolution
- **[Resolving .xrpl Domains with ZNS](./interacting-with-evm/advanced-guides/resolve-xrpl-domains.md)**
  Integrate human-readable .xrpl domain names into your dApp or wallet. Learn how to resolve domains to addresses, perform reverse lookups, and implement efficient caching strategies using the ZNS Registry.

#### Wallet Integration
- **[Integrating Reown Social Login](./interacting-with-evm/advanced-guides/reown-dapp-example.md)**
  Add social login functionality to your dApp using Reown AppKit (formerly WalletConnect). Enable users to connect with Google, GitHub, Discord, and other social providers for a seamless onboarding experience.

#### Cross-Chain Transactions
Build applications that span multiple blockchains using Axelar's and Wormhole's cross-chain infrastructure:

1. **[Introduction to Cross-Chain Transactions](./interacting-with-evm/advanced-guides/cross-chain-transactions/introduction.md)**
   Learn the fundamentals of cross-chain development, available bridge options, and when to use different solutions for connecting XRPL EVM with other blockchain ecosystems.

2. **[Send Messages (Axelar GMP)](./interacting-with-evm/advanced-guides/cross-chain-transactions/send-messages.md)**
   Use Axelar's General Message Passing (GMP) to execute smart contract logic across chains. Enable cross-chain function calls, multi-chain governance, and complex inter-blockchain workflows.

3. **[Send Tokens (Axelar ITS)](./interacting-with-evm/advanced-guides/cross-chain-transactions/send-tokens.md)**
   Implement cross-chain token transfers using Axelar's Interchain Token Service (ITS). Move assets seamlessly between XRPL EVM and other supported networks like Ethereum, Polygon, Avalanche, and more.

4. **[Send Messages (Wormhole)](../bridge/wormhole/send-messages.md)**
   Publish and receive arbitrary cross-chain messages from Solidity with the Wormhole Core Contract, connecting XRPL EVM to 40+ networks including Solana and Sui.

5. **[Transfer Tokens (Wormhole)](../bridge/wormhole/transfer-tokens.md)**
   Bridge ERC-20 tokens programmatically with the Wormhole TypeScript SDK, deploy multichain-native tokens with [NTT](../bridge/wormhole/native-token-transfers.md), or embed the [Connect widget](../bridge/wormhole/connect-widget.md) for a drop-in bridging UI.

6. **[Swap with Squid Widget](./interacting-with-evm/advanced-guides/cross-chain-transactions/swap-with-squid-widget.md)**
   Integrate the Squid Widget to enable users to swap tokens from any supported chain directly into XRP on XRPL EVM. Perfect for onboarding users from Ethereum, Arbitrum, Polygon, and 70+ other chains.

7. **[Cross-Chain FAQs](./interacting-with-evm/advanced-guides/cross-chain-transactions/faqs.md)**
   Common questions, troubleshooting tips, and best practices for cross-chain development on XRPL EVM.

---

## 2. Interacting with Cosmos

Leverage the Cosmos SDK foundation for enhanced blockchain functionality:

1. **[Introduction](./interacting-with-cosmos/introduction.md)**
   Understand how XRPL EVM leverages the Cosmos SDK for modularity, security, and interoperability.

2. **[Using IBC](./interacting-with-cosmos/using-ibc.md)**
   Transfer assets to and from other Cosmos chains using Inter-Blockchain Communication protocol.

3. **[Using the API](./interacting-with-cosmos/using-the-api.md)**
   Query network data using gRPC, REST, or CometBFT endpoints for deeper blockchain integration.

### Advanced Guides

#### Address Management
- **[Address Translation](./interacting-with-cosmos/advanced-guides/address-translation.md)**
  Convert addresses between Cosmos Bech32 format (`ethm...`) and EVM-style `0x` formats for seamless integration across both sides of the blockchain.

#### Cross-Chain Transactions
- **[Swap with Skip Widget](./interacting-with-cosmos/advanced-guides/cross-chain-transactions/swap-with-skip-widget.md)**
  Integrate Skip Protocol's widget to enable users to swap tokens from Cosmos chains like Osmosis, Injective, or Cosmos Hub directly into XRP on XRPL EVM. Perfect for accessing liquidity from the broader Cosmos ecosystem.

---

## 3. Resources

Essential tools and references for XRPL EVM development:

- **[Block Explorers](./resources/block-explorers.md)**
  Track and inspect blocks, transactions, and contracts on both EVM and Cosmos sides.

- **[Public APIs](./resources/public-apis.md)**
  Access XRPL EVM via public RPC, REST, and WebSocket endpoints.