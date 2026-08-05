---
html: overview.html
blurb: Overview of cross-chain bridge connectivity.
labels:
  - Interoperability
status: not_enabled
---

# Bridge 🌉

The XRPL EVM Sidechain operates as a **standalone Layer 1 blockchain** with XRP as the gas token. For applications that require cross-chain functionality, several bridge options are available to connect with other blockchain networks.

Bridges are specialized protocols designed to facilitate interoperability between distinct blockchain networks, allowing them to communicate and exchange assets or data when needed. They typically consist of smart contracts and off-chain components that validate and relay information between connected chains.

## Available Bridge Solutions

Currently, the XRPL EVM Sidechain supports multiple bridge options for connecting to other networks when cross-chain functionality is desired:

### Axelar Network

One available option for cross-chain connectivity is the Axelar Network. Axelar is a decentralized cross-chain communication protocol that provides secure connectivity across Web3 ecosystems. It leverages a proof-of-stake (PoS) consensus model and is maintained by a network of validators that ensure message integrity during interchain transactions.

[Axelar mainnet deployments config](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/mainnet.json)

[Axelar testnet deployments config](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/testnet.json)

### Inter-Blockchain Communication (IBC)

As a Cosmos SDK-based blockchain, the XRPL EVM Sidechain natively supports Inter-Blockchain Communication (IBC), the standard protocol for secure communication between independent blockchains in the Cosmos ecosystem. IBC enables trustless asset transfers and data exchange with other IBC-enabled chains, providing direct connectivity to the broader Cosmos network without requiring external bridge infrastructure.

[Using IBC](../developers/interacting-with-cosmos/using-ibc.md)

[Cosmos SDK Foundation](../developers/interacting-with-cosmos/introduction.md)


### Wormhole

[Wormhole](https://wormhole.com/), one of the leading cross-chain interoperability protocols, is **live on the XRPL EVM Sidechain**. It enables cross-chain messaging, wrapped and native token transfers, and multichain token issuance, supporting use cases across DeFi, institutional onchain finance, and real-world assets (RWAs).

[Wormhole on XRPL EVM](./wormhole/index.md) — overview, deployed contracts, and developer guides.

[Ripple Expands Multichain Interoperability Infrastructure with Wormhole Integration](https://wormhole.com/blog/ripple-expands-multichain-interoperability-infrastructure-with-wormhole)

### Other Bridge Options

The XRPL EVM Sidechain's architecture allows for integration with various bridge protocols, giving developers flexibility in choosing the cross-chain solution that best fits their needs.

## Implementation Options

For developers interested in adding cross-chain functionality to their applications, see:

- [Axelar ITS](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/send-tokens.md): Use bridge services to move tokens between XRPL EVM Sidechain and other networks.
- [Axelar GMP](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/send-messages.md): Understand message passing between different blockchains.
- [Wormhole Messaging](./wormhole/send-messages.md): Send cross-chain messages from Solidity with the Wormhole Core Contract.
- [Wormhole Token Transfers](./wormhole/transfer-tokens.md): Bridge ERC-20 tokens with the Wormhole TypeScript SDK or Portal Bridge.
- [Wormhole NTT](./wormhole/native-token-transfers.md): Deploy multichain-native tokens with the Native Token Transfers framework.
- [Wormhole Connect](./wormhole/connect-widget.md): Add a drop-in bridging UI to your dApp.
- [Cosmos IBC](../developers/interacting-with-cosmos/using-ibc.md): Technical reference for IBC implementation and available channels.
- [Sending XRP Through IBC](../users/sending-through-ibc.md): User guide for IBC transfers using Keplr wallet.
- [Skip Widget](../developers/interacting-with-cosmos/advanced-guides/cross-chain-transactions/swap-with-skip-widget.md): Add cross-chain swaps from Cosmos chains to your app.
- [Squid Widget](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/swap-with-squid-widget.md): Add cross-chain swaps from EVM chains to your app.
- [Axelar Deployed Contracts Mainnet](deployed-contracts-mainnet.md): Reference implementations for various bridge integrations.
- [Axelar Deployed Contracts Testnet](deployed-contracts-testnet.md): Reference implementations for various bridge integrations.


