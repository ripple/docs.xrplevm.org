# Send and receive tokens from XRPL

Transferring tokens across chains is a critical aspect of cross-chain dApps. This process involves secure and efficient movement of assets between ecosystems, enabling interoperability. The XRP Ledger (XRPL) is no exception, but its inability to execute native smart contracts makes cross-chain communication even more crucial.

To facilitate token transfers, XRPL integrates with the Axelar cross-chain communication protocol. Axelar offers two primary solutions for this purpose: [Gateway Tokens](https://docs.axelar.dev/dev/general-message-passing/gmp-tokens-with-messages/) and the [Interchain Token Service (ITS)](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/).

For XRPL and XRPL EVM, the Interchain Token Service is the preferred choice. ITS is a modern solution that preserves native token qualities while providing advanced features for managing token properties and supply across chains. In a few words, each connected chain has its own ITS address that will be used as the entry point for token transfers. This address is a smart contract in the XRPL EVM and an account in the XRPL Ledger. For comprehensive understanding of ITS, check the [official documentation](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/). Additionally, Axelar's [step-by-step guide](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/developer-guides/programmatically-create-a-token/) provides detailed guidance on how to create an interchain token with its corresponding properties.

For practical guidance on using ITS to send and receive tokens, refer to the guides below. These cover how to transfer XRP and [IOU](https://xrpl.org/docs/concepts/tokens/fungible-tokens)/[ERC20](https://docs.openzeppelin.com/contracts/4.x/erc20) tokens between the XRP Ledger and XRPL EVM. The examples are written in Typescript and leverage the [xrpl.js](https://js.xrpl.org/index.html) and [ethers](https://docs.ethers.org/v6/) libraries to interact with the XRP Ledger and XRPL EVM, respectively.

## Sending assets from XRP Ledger to XRPL EVM

Sending assets from the XRP Ledger to the XRPL EVM or other chains is straightforward. The process involves executing a standard payment transaction, specifying the following key parameters:

- [`Amount`](https://js.xrpl.org/interfaces/Payment.html#Amount): Specifies the quantity of the asset to be transferred. The format and value depend on the type of asset being sent (e.g., XRP or IOUs).
- [`Destination`](https://js.xrpl.org/interfaces/Payment.html#Destination): Refers to the Interchain Token Service address on the XRP Ledger.
- [`Memos`](https://js.xrpl.org/interfaces/Payment.html#Memos): Contains additional data required for the transfer, including:
  - The **destination chain ID** on the Axelar network.
  - The **recipient's address** on the destination chain.
  - A **payload hash**, which is only relevant for [GMP messages](https://docs.axelar.dev/dev/general-message-passing/overview/) and ignored in token transfers.

**Important**: All fields within [`Memos`](https://js.xrpl.org/interfaces/Payment.html#Memos) must be encoded in hexadecimal format before submission.

### Sending XRP from XRP Ledger to XRPL EVM

To send XRP - the native token of the XRPL Ledger and XRPL EVM - to the XRPL EVM, the payment must specify the [`Amount`](https://js.xrpl.org/interfaces/Payment.html#Amount) as the number of XRP in drops to be transferred to the other chain. This process is similar to an on-chain payment on the XRPL.

The example bellow shows how to send 100 XRP from the XRPL to the XRPL EVM.

```ts
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: "100000000", // 100 XRP in drops
  Destination: "rP9iHnCmJcVPtzCwYJjU1fryC2pEcVqDHv", // ITS address in the XRP Ledger
  Memos: [
    {
      Memo: {
        // hex(destination_address)
        MemoType: "64657374696E6174696F6E5F61646472657373",
        // Destination contract address (hexadecimal, without 0x prefix)
        MemoData: "9159C650E1D7E10A17C450EB3D50778ABA593D61",
      },
    },
    {
      Memo: {
        // hex(destination_chain)
        MemoType: "64657374696E6174696F6E5F636861696E",
        // The destination chain ID on the Axelar network (hexadecimal)
        MemoData: convertStringToHex("xrpl-evm-sidechain"),
      },
    },
    {
      Memo: {
        // hex(payload_hash)
        MemoType: "7061796C6F61645F68617368",
        // Set to 32-byte zero value since it is not used in token transfers
        MemoData:
          "0000000000000000000000000000000000000000000000000000000000000000",
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
```

### Sending IOU tokens from XRP Ledger to XRPL EVM

Sending IOU tokens from the XRP Ledger to the XRPL EVM is similar to sending XRP, with one key difference: the `Amount` field is no longer a `string`. Instead, it is an [`IssuedCurrencyAmount`](https://js.xrpl.org/interfaces/IssuedCurrencyAmount.html) object which contains the following fields:

- `currency`: The currency code of the token.
- `issuer`: The address of the token's issuer.
- `value`: The amount of the token to transfer (with decimals).

The example bellow demonstrates how to send 100 RLUSD from the XRPL to the XRPL EVM.

```ts
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: {
    currency: "524C555344000000000000000000000000000000", // RLUSD (non-standard currency code)
    Issuer: "rMxCKbEDwqr76QuheSUMdEGf4B9xJ8m5De", // RLUSD issuer address
    value: "100", // Amount to transfer
  },
  Destination: "rP9iHnCmJcVPtzCwYJjU1fryC2pEcVqDHv", // ITS address in the XRP Ledger
  Memos: [
    {
      Memo: {
        // hex(destination_address)
        MemoType: "64657374696E6174696F6E5F61646472657373",
        // Destination contract address (hexadecimal, without 0x prefix)
        MemoData: "9159C650E1D7E10A17C450EB3D50778ABA593D61",
      },
    },
    {
      Memo: {
        // hex(destination_chain)
        MemoType: "64657374696E6174696F6E5F636861696E",
        // The destination chain ID on the Axelar network(hexadecimal)
        MemoData: convertStringToHex("xrpl-evm-sidechain"),
      },
    },
    {
      Memo: {
        // hex(payload_hash)
        MemoType: "7061796C6F61645F68617368",
        // Set to 32-byte zero value since it is not used in token transfers
        MemoData:
          "0000000000000000000000000000000000000000000000000000000000000000",
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
```

## Sending assets from XRPL EVM to XRP Ledger

Sending assets from the XRPL EVM to the XRP Ledger is achieved by calling the [`interchainTransfer`](https://github.com/axelarnetwork/interchain-token-service/blob/9edc4318ac1c17231e65886eea72c0f55469d7e5/contracts/interfaces/IInterchainTokenStandard.sol#L19) method of the [ITS contract](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/) with the following parameters:

- `tokenId`: The token ID of the asset to be transferred. This is the ID assigned to the asset in the Axelar network when the token is created.
- `destinationChain`: The chain ID in the Axelar network to which the asset will be transferred.
- `destinationAddress`: The recipient's address on the destination chain.
- `amount`: The amount of the asset to transfer, represented as an integer without decimals.

### ITS Contract Instantiation

Before performing a transfer, you need to instantiate the [ITS contract](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/) using the [ethers library](https://docs.ethers.org/v6/). The example below demonstrates how to create an instance of the ITS contract.

```ts
import { Contract } from "ethers";

// The signer initialization is omitted for brevity

// Instantiate the ITS contract
const its = new Contract(
  "0x43F2ccD4E27099b5F580895b44eAcC866e5F7Bb1", // The ITS address in the XRPL EVM
  ITS_ABI, // ABI for the ITS contract
  signer
);
```

### Sending XRP from XRPL EVM to XRP Ledger

To transfer XRP from the XRPL EVM back to the XRP Ledger, the [`interchainTransfer`](https://github.com/axelarnetwork/interchain-token-service/blob/9edc4318ac1c17231e65886eea72c0f55469d7e5/contracts/interfaces/IInterchainTokenStandard.sol#L19) method of the [ITS contract](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/) has to be called with the parameters specified above.

The example bellow shows how to send 100 XRP from the XRPL EVM to the XRP Ledger.

```ts
import ethers from "ethers";

// Call the interchainTransfer method
await its.interchainTransfer(
  "0xc2bb311dd03a93be4b74d3b4ab8612241c4dd1fd0232467c54a03b064f8583b6", // XRP token ID
  "xrpl", // Destination chain ID
  "rP9iHnCmJcVPtzCwYJjU1fryC2pEcVqDHv", // Destination address
  "100000000000000000000", // 100 XRP in wei
  "0x", // Metadata (not used)
  ethers.BigNumber.from("0") // Gas value (not used)
);
```

### Sending ERC20 tokens from XRPL EVM to XRP Ledger

Transferring ERC20 tokens from the XRPL EVM to the XRP Ledger involves a slightly more complex process, requiring two additional steps:

- **Set a Trust Line**: The recipient address on the XRP Ledger must establish a [trust line](https://xrpl.org/docs/concepts/tokens/fungible-tokens#trust-lines) to be able to receive the token.
- **Approve Token Transfer**: The sender on the XRPL EVM must call the [`approve`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/4c3ef87cf57b448a0b5fc68b8ce6604a31b60814/contracts/token/ERC20/ERC20.sol#L127) method of the ERC20 token to authorize the ITS contract to transfer tokens on their behalf.

Below is a step-by-step example demonstrating how to send 100 RLUSD from the XRPL EVM to the XRP Ledger.

First, to receive tokens on the XRP Ledger, the recipient must establish a trust line by submitting a [`TrustSet`](https://xrpl.org/trustset.html) transaction:

```ts
import { Wallet, Client, TrustSet } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the trust set transaction object
const trustSet: TrustSet = {
  TransactionType: "TrustSet",
  Account: wallet.address,
  LimitAmount: {
    currency: "524C555344000000000000000000000000000000", // RLUSD (non-standard currency code)
    issuer: "rMxCKbEDwqr76QuheSUMdEGf4B9xJ8m5De", // RLUSD issuer address
    value: "100", // Any value >= the amount to be transferred
  },
};

// Autofill transaction
const transaction = await client.autofill(trustSet);

// Sign transaction
const signedTransaction = wallet.sign(transaction).tx_blob;

// Submit transaction
const result = await client.submit(signedTransaction);
```

Next, the sender must call the [`approve`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/4c3ef87cf57b448a0b5fc68b8ce6604a31b60814/contracts/token/ERC20/ERC20.sol#L127) method on the ERC20 contract to allow the ITS contract to transfer the tokens:

```ts
import { Contract } from "ethers";

// The signer initialization is omitted for brevity

// Instantiate the ERC20 contract
const erc20 = new Contract(
  "0x20937978F265DC0C947AA8e136472CFA994FE1eD", // RLUSD ERC20 token address
  ERC20_ABI, // ABI for the ERC20 contract
  signer
);

// Call the approve method
await erc20.approve(
  "0x43F2ccD4E27099b5F580895b44eAcC866e5F7Bb1", // ITS address in the XRPL EVM
  "100000000000000000000" // Any value >= the amount to be transferred as an integer
);
```

Finally, use the [`interchainTransfer`](https://github.com/axelarnetwork/interchain-token-service/blob/9edc4318ac1c17231e65886eea72c0f55469d7e5/contracts/interfaces/IInterchainTokenStandard.sol#L19) method on the ITS contract to send the tokens:

```ts
import ethers from "ethers";

// Call the interchainTransfer method
await its.interchainTransfer(
  "0x85f75bb7fd0753565c1d2cb59bd881970b52c6f06f3472769ba7b48621cd9d23", // RLUSD token ID
  "xrpl", // Destination chain ID
  "rP9iHnCmJcVPtzCwYJjU1fryC2pEcVqDHv", // Recipient address on XRP Ledger
  "100000000000000000000", // 100 RLUSD as an integer
  "0x", // Metadata (not used)
  ethers.BigNumber.from("0") // Gas value (not used)
);
```
