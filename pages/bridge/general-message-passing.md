---
html: general-message-passing.html
blurb: Learn more about Axelar general message passing.
labels:
  - Interoperability
status: not_enabled
---

# Axelar General Message Passing (GMP)

## Feature Overview

Axelar General Message Passing (GMP) is a cross-chain communication protocol that allows arbitrary data and function calls to be transmitted securely across different blockchain networks. Through GMP, smart contract execution on one chain can be triggered from another, enabling interoperability beyond simple token transfers.

For the XRPL Ledger, this integration unlocks smart contract functionality by bridging it to the XRPL EVM Sidechain. Specifically, when a transaction on XRPL needs to initiate a smart contract on the EVM Sidechain, Axelar acts as the relay layer, verifying the message, ensuring consensus, and executing the command on the EVM side. This seamless interaction empowers the XRPL with access to EVM-compatible DeFi protocols, automated workflows, and complex dApps.

## How It Works

![xrpl-evm-sidechain-axelar-gmp](./img/evm-sidechain-axelar-gmp.png)

1. **Message Construction**: On XRPL, a message is constructed with the destination chain, contract address, and the function call to be executed. This message is submitted as a XRPL Payment transaction to the Axelar Multisig account.
2. **Message Submission and Verification**: The message is submitted to the Axelar network, where it is verified and relayed to the destination chain, which corresponds to the XRPL EVM Sidechain. Once relayed, the Axelar amplifier gateway approves the message.
3. **Call smart contract function**: The Axelar relayer executes the message on the desired smart contract on the XRPL EVM Sidechain.

## Key Components

- **Axelar Multisig Account**: The Axelar Multisig account is the account that is responsible for submitting the message to the Axelar network (from XRP Ledger).
- **Axelar Amplifier Gateway**: The Axelar Amplifier Gateway is the smart contract which communicates with the Axelar network, enabling the message passing functionality.
- **XRPL EVM Sidechain**: The XRPL EVM Sidechain is the destination for the message.
- **XRPL Ledger**: The XRP Ledger is the source of the message.

## Example

The following example demonstrates how to complete a general message passing transaction from the XRP Ledger to a smart contract on the EVM Sidechain.

1. Compute the payload that you want to call on XRPL EVM Sidechain `AxelarExecutable` smart contract's `_execute` function.
2. Create an XRPL `Payment` transaction with the following fields:

## Sending a message from XRP Ledger to XRPL EVM

To send a message from the XRP Ledger (XRPL) to the XRPL EVM, a `Payment` transaction is used with the following key parameters:

- `Amount`: Represents the value of the asset to be sent. If the message does not include an asset transfer, this can be set to 1 drop (the smallest XRP unit).
- `Destination`: The address of the Gateway on the XRP Ledger.
  - [**Mainnet Address**](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/mainnet.json)
  - [**Testnet Address**](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/testnet.json#L2603)
  - [**Devnet Address**](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/devnet-amplifier.json#L985)

- `Memos`: Hex-encoded data required for the function call, including:
  - The _type_ of call to initiate.
  - The _destination chain_ on the Axelar network.
  - The _contract address_ on the destination chain to which the message is sent.
  - The _payload hash_, which contains the data to be sent to the destination contract. This payload must be ABI-encoded. The [ethers AbiCoder](https://docs.ethers.org/v6/api/abi/abi-coder/#AbiCoder-encode) can be used for encoding the payload.

See [Axelar's documentation](https://github.com/axelarnetwork/axelar-contract-deployments/tree/main/xrpl#general-message-passing) for a guide on making GMP contract calls.


3. Submit the transaction to the XRPL Ledger. Within a few minutes, the relayer should submit validator signatures of the XRPL Testnet deposit transaction to the XRPL EVM Sidechain `AxelarAmplifierGateway` contract, which records the approval of the payload hash and emits a `ContractCallApproved` event.
4. Once the transaction is confirmed, call the `execute` function on the XRPL EVM Sidechain `AxelarExecutable` smart contract.

```
AXELAR_EXECUTABLE= # your `AxelarExecutable` contract
COMMAND_ID= # the `commandId` that was emitted in the `ContractCallApproved` event
SOURCE_ADDRESS= # the XRPL address that performed the `Payment` deposit transaction
PAYLOAD= # abi.encode(['string', 'uint256', 'bytes'], [symbol, amount, gmpPayload])
cast send $AXELAR_EXECUTABLE 'function execute(bytes32 commandId, string calldata sourceChain, string calldata sourceAddress, bytes calldata payload)' $COMMAND_ID xrpl $SOURCE_ADDRESS $PAYLOAD --private-key $PRIVATE_KEY --rpc-url $SEPOLIA_RPC_URL
```
