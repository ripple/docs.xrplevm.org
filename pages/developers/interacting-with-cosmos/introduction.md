# Introduction

The XRPL EVM is built on top of the [Cosmos SDK](https://cosmos.network/appchains/), an open-source toolkit for building blockchains. The Cosmos SDK framework provides a powerful foundation with several key benefits:

- **Modularity**: The Cosmos SDK employs a modular architecture, allowing developers to choose from predefined modules or create custom modules for various blockchain functionalities. This flexibility speeds up development while accommodating unique use cases.
- **Interoperability**: Custom blockchains built using the Cosmos SDK can natively interoperate with other blockchains through the [IBC (Inter-Blockchain Communication)](https://ibc.cosmos.network/) protocol. Refer to the [Using IBC page](./using-ibc.md) for a deeper dive into this feature.
- **Security**: The Cosmos SDK abstracts and secures low-level blockchain mechanics, enabling developers to focus on building secure and efficient interactions between modules.
- **Built-in interfaces**: With the Cosmos SDK, users can easily query network and application state information through built-in interfaces that interact directly with blockchain stores and modules. For more details see the [Using the API page](./using-the-api.md).

To learn more about the Cosmos SDK and its capabilities, visit the [official documentation](https://docs.cosmos.network/).

## Cosmos and EVM

The modular design of the Cosmos SDK enables seamless integration of the Ethereum Virtual Machine (EVM) as the execution layer. When a transaction is submitted, the execution layer processes it, updates the blockchain state, and ensures that the output is both deterministic and consistent across all nodes.

The Cosmos SDK's [multistore](https://docs.cosmos.network/v0.52/learn/intro/sdk-design#multistore) architecture plays a key role in this process by allowing modules to define and manage their own state. This means that the execution layer, represented by the EVM in this case, triggers state updates across various modules, ensuring a well-structured and modular state management system.

The XRPL EVM leverages [Evmos](https://github.com/evmos/evmos) as its EVM implementation, providing robust compatibility with Ethereum tools and smart contracts while integrating seamlessly within the Cosmos ecosystem.

The diagram below illustrates the architecture of Cosmos SDK using [Evmos](https://github.com/evmos/evmos) as the EVM execution layer and [CometBFT](https://docs.cometbft.com/v1.0/) as the consensus layer. For a deeper dive into the Cosmos SDK architecture, refer to the [official documentation](https://docs.cosmos.network/v0.52/learn/intro/sdk-app-architecture).

![Cosmos SDK architecture](https://peersyst-public-production.s3.eu-west-1.amazonaws.com/06826707-492e-4249-9802-43abff464ae2.svg)
