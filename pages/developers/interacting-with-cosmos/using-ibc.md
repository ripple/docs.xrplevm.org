# Using IBC

The Inter-Blockchain Communication Protocol (IBC) is an open-source protocol designed to handle authentication and the transport of data between blockchains. IBC enables heterogeneous blockchains to communicate trustlessly, facilitating the exchange of data, messages, and tokens.

As a Cosmos-based blockchain, the XRPL EVM implements IBC, allowing it to connect seamlessly with other Cosmos chains or any blockchain that supports the IBC protocol. This interoperability enables the native bridging of tokens, messages, and accounts between chains.

For more detailed information on IBC, refer to the [IBC documentation](https://ibc.cosmos.network/).

## Available IBC Channels

The XRPL EVM has established IBC channels with various Cosmos-based chains, enabling cross-chain token transfers and communication.

### Mainnet Channels

{% partial file="/snippets/_ibc-channels-mainnet.md" /%}

### Testnet Channels

{% partial file="/snippets/_ibc-channels-testnet.md" /%}

These channels enable seamless token transfers and message passing between XRPL EVM and the connected Cosmos chains.
