# Introduction

A cross-chain dApp is a decentralized application that operates across multiple blockchain networks. These applications enable users to seamlessly interact with different blockchains, offering a unified experience for accessing and managing assets and services across various ecosystems.

## The Role of Bridges in Cross-Chain Interoperability

Achieving seamless cross-chain compatibility requires a robust communication layer, which is where bridges come in. Bridges are protocols that facilitate interaction between dApps and multiple blockchain networks, enabling secure and efficient token transfers and message passing across chains.

Several bridge solutions are available for connecting XRPL EVM with other blockchain networks, each offering unique features and capabilities. When selecting a bridge, consider factors such as:

- **Security model**: How the bridge validates and secures cross-chain transactions
- **Supported chains**: Which blockchain networks the bridge connects to
- **Token support**: What types of assets can be transferred
- **Message passing**: Whether the bridge supports cross-chain function calls
- **Fees and speed**: Transaction costs and confirmation times

### Example: Axelar

Axelar is one cross-chain communication protocol that facilitates interaction between dApps and multiple blockchain networks. With Axelar, tokens can be transferred between chains securely and efficiently. This capability is especially valuable for bridging the XRP Ledger and XRPL EVM, as XRP serves as the native token for both. For instance, XRP can be used to pay gas fees for executing EVM-compatible smart contract calls in the XRPL EVM. To learn more about this process, refer to the [Send Tokens page](./send-tokens.md).

Beyond token transfers, Axelar supports General Message Passing (GMP), a feature that enables direct function calls on other connected chains. This functionality expands the possibilities for dApps by allowing them to trigger complex workflows and interactions across chains. For more details, visit the [Send Messages page](./send-messages.md).

For an in-depth understanding of Axelar's capabilities, explore their [official documentation](https://docs.axelar.dev/).
