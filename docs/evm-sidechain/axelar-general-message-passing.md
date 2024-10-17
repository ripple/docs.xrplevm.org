---
html: axelar-general-message-passing.html
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

![xrpl-evm-sidechain-axelar-gmp](../img/evm-sidechain-axelar-gmp.png)

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
```json
{
    TransactionType: "Payment",
    Account: "YOUR_XRPL_ADDRESS",
    Amount: "1000000", // = 1 XRP - the amount of XRP you want to bridge, in drops
    Destination: "rP9iHnCmJcVPtzCwYJjU1fryC2pEcVqDHv", // Axelar Multisig address on XRPL
    Memos: [
        {
            // Destination address on XRPL EVM Sidechain
            Memo: {
                MemoData: "YOUR_SC_DESTINATION_ADDRESS", // your ETH recipient address, without the 0x prefix
                MemoType: "64657374696E6174696F6E5F61646472657373", // hex("destination_address")
            },
        },
        {
            Memo: {
                MemoData: "7872706C2D65766D2D73696465636861696E", // hex("xrpl-evm-sidechain")
                MemoType: "64657374696E6174696F6E5F636861696E", // hex("destination_chain")
            },
        },
        {
            Memo: {
                MemoData: "YOUR_PAYLOAD_HASH",  // The hash of the payload you want to call on XRPL EVM Sidechain
                MemoType: "7061796C6F61645F68617368", // hex("payload_hash")
            },
        },
    ],
    ...
}
```

3. Submit the transaction to the XRPL Ledger. Within a few minutes, the relayer should submit validator signatures of the XRPL Testnet deposit transaction to the XRPL EVM Sidechain `AxelarAmplifierGateway` contract, which records the approval of the payload hash and emits a `ContractCallApproved` event. 
4. Once the transaction is confirmed, call the `execute` function on the XRPL EVM Sidechain `AxelarExecutable` smart contract.
```
AXELAR_EXECUTABLE= # your `AxelarExecutable` contract
COMMAND_ID= # the `commandId` that was emitted in the `ContractCallApproved` event
SOURCE_ADDRESS= # the XRPL address that performed the `Payment` deposit transaction
PAYLOAD= # abi.encode(['string', 'uint256', 'bytes'], [symbol, amount, gmpPayload])
cast send $AXELAR_EXECUTABLE 'function execute(bytes32 commandId, string calldata sourceChain, string calldata sourceAddress, bytes calldata payload)' $COMMAND_ID xrpl $SOURCE_ADDRESS $PAYLOAD --private-key $PRIVATE_KEY --rpc-url $SEPOLIA_RPC_URL
```
