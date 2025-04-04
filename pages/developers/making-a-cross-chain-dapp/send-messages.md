# Send messages from XRPL

In addition to transferring tokens across chains, Axelar enables cross-chain communication through [Axelar's General Message Passing (GMP)](https://docs.axelar.dev/dev/general-message-passing/overview/). GMP enables developers to call any function on any other connected chain. For example, smart contracts on EVM-compatible chains, such as the XRPL EVM, can be triggered directly from the XRP Ledger (XRPL).

To send a message from one chain to another, there are some key aspects to consider:

- The destination chain must be an EVM-compatible chain, as executing messages requires a smart contract.
- The destination contract address must inherit from [`AxelarExecutable`](https://github.com/axelarnetwork/axelar-gmp-sdk-solidity/blob/main/contracts/executable/AxelarExecutable.sol)

For a deeper dive into GMP, refer to the [official documentation](https://docs.axelar.dev/dev/general-message-passing/overview/). Additionally, Axelar's [step-by-step guide](https://docs.axelar.dev/dev/general-message-passing/step-by-step/) provides a detailed walkthrough of the process.

## Sending a message from XRP Ledger to XRPL EVM

To send a message from the XRP Ledger (XRPL) to the XRPL EVM, a payment transaction is used with the following key parameters:

- [`Amount`](https://js.xrpl.org/interfaces/Payment.html#Amount): Represents the value of the asset to be sent. If the message does not include an asset transfer, this can be set to 1 drop (the smallest XRP unit).
- [`Destination`](https://js.xrpl.org/interfaces/Payment.html#Destination): The address of the Gateway on the XRP Ledger.
- [`Memos`](https://js.xrpl.org/interfaces/Payment.html#Memos): Encodes additional data required for the function call, including:
  - The **destination chain ID** on the Axelar network.
  - The **contract address** on the destination chain to which the message is sent.
  - The **payload hash**, which contains the data to be sent to the destination contract. This payload must be ABI-encoded. The [ethers AbiCoder](https://docs.ethers.org/v6/api/abi/abi-coder/#AbiCoder-encode) can be used for encoding the payload.

**Important**: All fields within [`Memos`](https://js.xrpl.org/interfaces/Payment.html#Memos) must be encoded in hexadecimal format before submission.

Below are **two examples** (Devnet and Testnet) showing how to call a smart contract function on the XRPL EVM from the XRP Ledger. Both examples use the [ethers](https://docs.ethers.org/v6/) library to encode the payload and the [xrpl.js](https://js.xrpl.org/index.html) library to interact with the XRP Ledger.

{% tabs %}
{% tab label="Devnet" %}

```ts
import { AbiCoder } from "ethers";
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// Create the ABI-encoded payload
const payload = AbiCoder.defaultAbiCoder().encode(["string"], ["Hello World"]);

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object for DEVNET
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: "10000000", // 10 Million drops = 10 XRP
  // Devnet Gateway address on the XRP Ledger
  Destination: "rGAbJZEzU6WaYv5y1LfyN7LBBcQJ3TxsKC",
  Memos: [
    {
      Memo: {
        // hex(destination_address)
        MemoType: Buffer.from("destination_address")
          .toString("hex")
          .toUpperCase(),
        // Destination contract address (hexadecimal, without 0x prefix)
        MemoData: "ECFA31764C91805B6C8E1D488941E41A86531880",
      },
    },
    {
      Memo: {
        // hex(destination_chain)
        MemoType: Buffer.from("destination_chain")
          .toString("hex")
          .toUpperCase(),
        // The destination chain ID on the Axelar network (hexadecimal)
        // for Devnet
        MemoData: Buffer.from("xrpl-evm-devnet").toString("hex").toUpperCase(),
      },
    },
    {
      Memo: {
        // hex(payload_hash)
        MemoType: "7061796C6F61645F68617368", // "payload_hash" in hex
        // ABI-encoded payload (hexadecimal, without 0x prefix)
        MemoData: payload.slice(2),
      },
    },
  ],
};

// Autofill transaction
const transaction = await client.autofill(payment);

// Sign transaction
const signedTransaction = wallet.sign(transaction).tx_blob;

// Submit transaction
const result = await client.submit(signedTransaction);
console.log(result);
```

{% /tab %}

{% tab label="Testnet" %}

```ts
import { AbiCoder } from "ethers";
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// Create the ABI-encoded payload
const payload = AbiCoder.defaultAbiCoder().encode(["string"], ["Hello World"]);

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object for TESTNET
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: "10000000", // 10 Million drops = 10 XRP
  // Testnet Gateway address on the XRP Ledger
  Destination: "rsCPY4vwEiGogSraV9FeRZXca6gUBWZkhg",
  Memos: [
    {
      Memo: {
        MemoData: Buffer.from("interchain_transfer").toString("hex").toUpperCase(),
        MemoType: Buffer.from("type").toString("hex").toUpperCase(),
      },
    },
    {
      Memo: {
        MemoData: Buffer.from(EvmDestinationAddress.slice(2)).toString("hex").toUpperCase(),
        MemoType: Buffer.from("destination_address").toString("hex").toUpperCase(),
      },
    },
    {
      Memo: {
        // Here we assume the testnet bridge chain is "xrpl-evm-testnet"
        MemoData: Buffer.from("xrpl-evm").toString("hex").toUpperCase(),
        MemoType: Buffer.from("destination_chain").toString("hex").toUpperCase(),
      },
    },
    {
      Memo: {
        MemoData: Buffer.from("1000000").toString("hex").toUpperCase(),
        MemoType: Buffer.from("gas_fee_amount").toString("hex").toUpperCase(),
      },
    },
    {
      // hex(payload_hash)
      MemoType: "7061796C6F61645F68617368", // "payload_hash" in hex
      // ABI-encoded payload (hexadecimal, without 0x prefix)
      MemoData: payload.slice(2),
    },
  ],
};

// Autofill transaction
const transaction = await client.autofill(payment);

// Sign transaction
const signedTransaction = wallet.sign(transaction).tx_blob;

// Submit transaction
const result = await client.submit(signedTransaction);
console.log(result);
```

{% /tab %}
{% /tabs %}
