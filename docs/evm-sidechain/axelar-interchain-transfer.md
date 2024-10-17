---
html: axelar-interchain-transfer.html
blurb: Learn more about Axelar interchain transfer.
labels:
  - Interoperability
status: not_enabled
---
# Axelar Interchain Transfer 

## Feature Overview

Axelar's Interchain Token Service (ITS) connects tokens across multiple blockchains, enabling token transfers between blockchains. ITS is designed to be a flexible, permisionless process, meaning that the way tokens are connected between chains can vary for different use cases. For further reading on the design, see: [Interchain Tokens and Interchain Token Service](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/).

## How It Works

Interchain transfers are executed through Axelar Interchain Token Service (ITS). ITS is a set of smart contracts deployed on the XRPL EVM Sidechain that are responsible for the interchain token transfer functionality. 

The following diagram shows the process of transferring a token from the XRPL to the XRPL EVM Sidechain.

![xrpl-evm-sidechain-axelar-its-transfer](../img/evm-sidechain-axelar-its-transfer.png)

1. **Payment transaction**: A payment transaction is submitted on XRPL Ledger. It contains the amount to be transferred, the destination address on the XRPL EVM Sidechain. The payment transaction is submitted to the Axelar Multisig account.
2. **Send Interchain Message through devnet-amplifier**: Once the payment transaction is submitted to the Axelar Multisig account, the devnet-amplifier will send a message to the XRPL EVM Sidechain Axelar Amplifier Gateway.
3. **Execute ITS contract on XRPL EVM Sidechain**: In the meantime, the relayer will call the ITS contract on the XRPL EVM Sidechain to start the token transfer process on the XRPL EVM Sidechain.
4. **Confirm token transfer message**: Once the relayer calls the ITS contract on the XRPL EVM Sidechain, the ITS contract will ask to the AxelarAmplifierGateway on the XRPL EVM Sidechain to confirm the token transfer message.
5. **Confirm response**: The AxelarAmplifierGateway on the XRPL EVM Sidechain will confirm the token transfer message.
6. **Transfer token**: If the AxelarAmplifierGateway confirms the token transfer message, the token will be transferred to the destination address on the XRPL EVM Sidechain.

## Key Components

- **Axelar Multisig Account**: The Axelar Multisig account is the account that is responsible for submitting the message to the Axelar network (from XRPL Ledger).
- **Axelar Amplifier Gateway**: The Axelar Amplifier Gateway is the smart contract which communicates with the Axelar network, enabling the message passing functionality.
- **ITS contracts**: The ITS contracts are the smart contracts which are responsible for the interchain token transfer functionality. You can find the ITS contracts on the [Axelar Developer Hub](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/).
- **XRPL EVM Sidechain**: The XRPL EVM Sidechain is the destination for the message.
- **XRPL Ledger**: The XRPL Ledger is the source of the message.

## Examples

Here's a list of examples of interchain transfers for different chains:

- [Axelar Interchain Transfer to XRPL](./axelar-interchain-evm-sidechain-xrpl.md)
- [Axelar Interchain Transfer to Avalanche Fuji](./axelar-interchain-evm-sidechain-avalanche.md)
