---
seo:
  description: "Cross-chain bridge options for the XRPL EVM Sidechain: Axelar for XRPL and EVM chains, Wormhole, and native IBC connectivity to the Cosmos ecosystem, with the bridge to use for each asset."
---

# Bridge

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

[Wormhole on XRPL EVM](./wormhole/index.md): overview, deployed contracts, and developer guides.

[Ripple Expands Multichain Interoperability Infrastructure with Wormhole Integration](https://wormhole.com/blog/ripple-expands-multichain-interoperability-infrastructure-with-wormhole)

### Other Bridge Options

The XRPL EVM Sidechain's architecture allows for integration with various bridge protocols, giving developers flexibility in choosing the cross-chain solution that best fits their needs.

## Bridges by asset

The same symbol can exist on XRPL EVM as different tokens depending on the bridge that brought it. Use the bridge that issued the token you hold, and check the contract address before integrating it.

| Asset | Bridge | Address on XRPL EVM Mainnet | Origin |
| ----- | ------ | --------------------------- | ------ |
| XRP (native) | Axelar ITS | `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` | XRP Ledger |
| USDC.xrpl | Axelar ITS | `0xDaF4556169c4F3f2231d8ab7BC8772Ddb7D4c84C` | USDC issued on the XRP Ledger |
| USDC.axl | Axelar ITS | `0xa16148c6Ac9EDe0D82f0c52899e22a575284f131` | Axelar-wrapped USDC |
| USDT | Axelar ITS | `0x9F8CF9c00fac501b3965872f4ed3271f6f4d06fF` | EVM chains via Axelar |
| WETH | Axelar ITS | `0x50498dC52bCd3dAeB54B7225A7d2FA8D536F313E` | EVM chains via Axelar |
| WBTC | Axelar ITS | `0xF8Eb4Ed0d4CF2bb707c0272F8C6827dEB6e4C0A9` | EVM chains via Axelar |
| DAI | Axelar ITS | `0xDc556F7209C48fC53a8cDf1339c033743A7e3e75` | EVM chains via Axelar |
| RLUSD | Wormhole NTT | `0x8d58C0C60B8D6b88Fa98B291a646dB34d0F98258` | Ethereum and other NTT chains |
| USDC.e | Wormhole NTT | `0xf75339EfD56B1da680BD4dE0f4086f24499275E7` | Ethereum (locks canonical USDC) |
| WETH | Wormhole WTT | `0xccD433A96A4DE148596F4E31Ab8ad1348077590B` | Ethereum |
| OSMO, INJ and other Cosmos assets | IBC | ERC-20 precompile per token pair | Osmosis (`channel-3`) and Injective (`channel-0`) |

For complete lists:

- Axelar ITS tokens on XRPL EVM: [Axelarscan](https://axelarscan.io/assets).
- Wormhole tokens: [Wormhole supported tokens](./wormhole/supported-tokens.md).
- IBC assets and their ERC-20 addresses: `GET /cosmos/evm/erc20/v1/token_pairs` (see [Precompiles](../developers/resources/precompiles.md)).

## Implementation Options

For developers interested in adding cross-chain functionality to their applications, see:

- [Axelar ITS](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/send-tokens.md): Use bridge services to move tokens between XRPL EVM Sidechain and other networks.
- [Axelar GMP](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/send-messages.md): Understand message passing between different blockchains.
- [Wormhole Messaging](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/send-messages.md): Send cross-chain messages from Solidity with the Wormhole Core Contract.
- [Wormhole Token Transfers](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/transfer-tokens.md): Bridge ERC-20 tokens with the Wormhole TypeScript SDK or Portal Bridge.
- [Wormhole NTT](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/native-token-transfers.md): Deploy multichain-native tokens with the Native Token Transfers framework.
- [Wormhole Connect](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/connect-widget.md): Add a drop-in bridging UI to your dApp.
- [Cosmos IBC](../developers/interacting-with-cosmos/using-ibc.md): Technical reference for IBC implementation and available channels.
- [Sending XRP Through IBC](../users/sending-through-ibc.md): User guide for IBC transfers using Keplr wallet.
- [Skip Widget](../developers/interacting-with-cosmos/advanced-guides/cross-chain-transactions/swap-with-skip-widget.md): Add cross-chain swaps from Cosmos chains to your app.
- [Squid Widget](../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/swap-with-squid-widget.md): Add cross-chain swaps from EVM chains to your app.
- [Axelar Deployed Contracts Mainnet](deployed-contracts-mainnet.md): Reference implementations for various bridge integrations.
- [Axelar Deployed Contracts Testnet](deployed-contracts-testnet.md): Reference implementations for various bridge integrations.
