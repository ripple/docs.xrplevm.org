# Developers 💻

Welcome! These pages will guide you through building on the **XRPL EVM**, from writing your first smart contract to creating cross-chain applications that tap into the broader Cosmos and Axelar ecosystems. Whether you’re a newcomer to blockchain development or an experienced Solidity engineer, you’ll find everything you need right here.

Below is a quick overview of all the available guides and resources in the developer section. Feel free to explore in whichever order suits you best!

---

## Developing Smart Contracts

1. **[Develop a Smart Contract](./developing-smart-contracts/develop-a-smart-contract.md)**
   Start with the basics: learn about smart contracts, the benefits of building on XRPL EVM, and how to write Solidity contracts that leverage the XRPL’s speed and low fees.

2. **[Deploy the Smart Contract](./developing-smart-contracts/deploy-the-smart-contract.md)**
   Step-by-step deployment guide using Remix or Hardhat.

3. **[Interact with the Smart Contract](./developing-smart-contracts/interact-with-the-smart-contract.md)**
   Call contract functions and integrate using `web3.js` or `ethers.js`.

4. **[Verify the Smart Contract](./developing-smart-contracts/verify-the-smart-contract.md)**
   Learn how to verify code on the XRPL EVM Explorer.

5. **[Next Steps](./developing-smart-contracts/next-steps.md)**
   Explore advanced topics like token standards, GMP, and full-stack dApps.

---

## Interacting with Axelar

1. **[Introduction to Axelar](./making-a-cross-chain-dapp/introduction.md)**
   Learn how Axelar powers secure cross-chain communication and opens the door to fully composable dApps between XRPL EVM and other ecosystems.

2. **[Send Tokens Across Chains](./making-a-cross-chain-dapp/send-tokens.md)**
   Use Axelar's Interchain Token Service (ITS) to bridge tokens to/from XRPL EVM.

3. **[Send Messages Across Chains](./making-a-cross-chain-dapp/send-messages.md)**
   Execute smart contract logic on remote chains using Axelar General Message Passing (GMP).

4. **[Integrate the Squid Widget](./making-a-cross-chain-dapp/swap-with-squid-widget.md)**
   Add the [Squid Widget](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/getting-started) to enable token swaps from any chain into XRP on XRPL EVM. Perfect for onboarding users directly from Ethereum, Arbitrum, Polygon, etc.

---

## Interacting with Cosmos

1. **[Introduction](./interacting-with-cosmos/introduction.md)**
   Understand how XRPL EVM leverages the Cosmos SDK for performance and modularity.

2. **[Using IBC](./interacting-with-cosmos/using-ibc.md)**
   Transfer assets to and from other Cosmos SDK chains using IBC.

3. **[Using the API](./interacting-with-cosmos/using-the-api.md)**
   Query network data using gRPC, REST, or CometBFT endpoints.

4. **[Address Translation](./interacting-with-cosmos/address-translation.md)**
   Convert addresses between Cosmos Bech32 and EVM-style `0x` formats.

5. **[Integrate the Skip Widget](./interacting-with-cosmos/swap-with-skip-widget.md)**
   Add the [Skip Widget](https://docs.skip.build/go/widget/getting-started) to enable users to swap from Cosmos chains like Osmosis or Injective directly into XRP on XRPL EVM.

---

## Use Oracle Data

1. **[Overview of Oracles](./use-oracle-data/band-protocol.md)**
   Learn how oracles provide external data feeds to smart contracts.

2. **[Integrate Band Protocol Oracle](./use-oracle-data/band-protocol.md)**
   Use Band’s oracle contracts to fetch real-time price data on-chain.

3. **[Query Available Price Feeds](./use-oracle-data/band-protocol.md)**
   Explore available assets (XRP, BTC, ETH, etc.) with secure on-chain access.

---

## Use Goldsky Indexers

1. **[Introduction to Goldsky](./use-goldsky-indexer/goldsky-overview.md)**
   Goldsky offers real-time data indexing for XRPL EVM, helping you track contract events, logs, and transactions without running your own infrastructure.

2. **[Set Up Your Indexer](./use-goldsky-indexer/setup-indexer.md)**
   Learn how to create and deploy an indexer for your XRPL EVM smart contracts using [Goldsky Studio](https://goldsky.com/).

3. **[Query Indexed Events](./use-goldsky-indexer/query-events.md)**
   Use GraphQL to query your data and integrate insights into your frontend.

4. **[Best Practices](./use-goldsky-indexer/best-practices.md)**
   Learn how to design indexers that are efficient, reliable, and compatible with frequent contract changes or upgrades.

---

## Advanced Guides

1. **[Using XRP as a Wrapped ERC-20](./advanced-guides/using-xrp-as-wrapped-erc20.md)**
   Discover why you *don’t* need to wrap XRP on XRPL EVM, how to work with the 18-decimal native ERC-20 at  
   `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`, and how to migrate from wXRP to the canonical token.

---

## Developer Resources

1. **[Block Explorers](./resources/block-explorers.md)**
   Track and inspect blocks, transactions, and contracts.

2. **[Public APIs](./resources/public-apis.md)**
   Access XRPL EVM via public RPC and REST endpoints.

---

## Getting Started & Next Steps

* Begin with deploying and verifying your first contract.
* Dive into Axelar and Cosmos sections to explore cross-chain logic and liquidity flows.
* Use Goldsky to index and query your contract data efficiently.
* Join the XRPL EVM developer community on [Discord](https://discord.gg/xrplevm) and [GitHub](https://github.com/xrplevm).

We're excited to see what you build next on XRPL EVM. 🚀