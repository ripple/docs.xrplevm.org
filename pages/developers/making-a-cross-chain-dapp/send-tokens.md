# Send and receive tokens from XRPL

Transferring tokens across chains is a critical aspect of cross-chain dApps. This process involves secure and efficient movement of assets between ecosystems, enabling interoperability. The XRP Ledger (XRPL) is no exception, but its inability to execute native smart contracts makes cross-chain communication even more crucial.

To facilitate token transfers, XRPL integrates with the Axelar cross-chain communication protocol. Axelar offers two primary solutions for this purpose: [Gateway Tokens](https://docs.axelar.dev/dev/general-message-passing/gmp-tokens-with-messages/) and the [Interchain Token Service (ITS)](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/).

For XRPL and XRPL EVM, the Interchain Token Service is the preferred choice. ITS is a modern solution that preserves native token qualities while providing advanced features for managing token properties and supply across chains. In a few words, each connected chain has its own ITS address that will be used as the entry point for token transfers. This address is a smart contract in the XRPL EVM and an account in the XRPL Ledger. For a comprehensive understanding of ITS, check the [official documentation](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/intro/). Additionally, Axelar's [step-by-step guide](https://docs.axelar.dev/dev/send-tokens/interchain-tokens/developer-guides/programmatically-create-a-token/) provides detailed guidance on how to create an interchain token with its corresponding properties.

For practical guidance on using ITS to send and receive tokens, refer to the guides below. These cover how to transfer XRP and [IOU](https://xrpl.org/docs/concepts/tokens/fungible-tokens)/[ERC20](https://docs.openzeppelin.com/contracts/4.x/erc20) tokens between the XRP Ledger and XRPL EVM. The examples are written in TypeScript and leverage the [xrpl.js](https://js.xrpl.org/index.html) and [ethers](https://docs.ethers.org/v6/) libraries to interact with the XRP Ledger and XRPL EVM, respectively.

---

## Sending assets from XRP Ledger to XRPL EVM

Sending assets from the XRP Ledger to the XRPL EVM or other chains is straightforward. The process involves executing a standard payment transaction, specifying the following key parameters:

- [`Amount`](https://js.xrpl.org/interfaces/Payment.html#Amount): Specifies the quantity of the asset to be transferred. The format and value depend on the type of asset being sent (e.g., XRP or IOUs).
- [`Destination`](https://js.xrpl.org/interfaces/Payment.html#Destination): Refers to the **Interchain Token Service** address on the XRP Ledger.
- [`Memos`](https://js.xrpl.org/interfaces/Payment.html#Memos): Contains additional data required for the transfer, including:
  - The **destination chain ID** on the Axelar network.
  - The **recipient's address** on the destination chain.
  - A **payload hash**, which is only relevant for [GMP messages](https://docs.axelar.dev/dev/general-message-passing/overview/) and ignored in token transfers.

**Important**: All fields within [`Memos`](https://js.xrpl.org/interfaces/Payment.html#Memos) must be encoded in hexadecimal format before submission.

### Sending XRP from XRP Ledger to XRPL EVM

To send **XRP** (the native token) from the XRPL to the XRPL EVM, you simply specify the number of XRP in drops in the [`Amount`](https://js.xrpl.org/interfaces/Payment.html#Amount).

{% tabs %}
{% tab label="Devnet" %}

```ts
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: "100000000", // 100 XRP in drops
  EvmDestinationAddress: "0x94E9ba2f224d98918a64260F552ba7ce5C6E46e1", // Receiver address. 
  // ITS address on the XRPL Devnet
  Destination: "rGAbJZEzU6WaYv5y1LfyN7LBBcQJ3TxsKC",
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
        MemoData: Buffer.from("xrpl-evm-devnet").toString("hex").toUpperCase(),
        MemoType: Buffer.from("destination_chain").toString("hex").toUpperCase(),
      },
    },
    {
      Memo: {
        MemoData: Buffer.from("1000000").toString("hex").toUpperCase(),
        MemoType: Buffer.from("gas_fee_amount").toString("hex").toUpperCase(),
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

{% /tab %}



{% tab label="Testnet" %}

```ts
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: "100000000", // 100 XRP in drops
  EvmDestinationAddress: "0x94E9ba2f224d98918a64260F552ba7ce5C6E46e1", // Receiver address.
  // Gateway address on the XRPL Testnet
  Destination: "rNrjh1KGZk2jBR3wPfAQnoidtFFYQKbQn2",
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
  ],
};

// Autofill transaction
const transaction = await client.autofill(payment);

// Sign transaction
const signedTransaction = wallet.sign(transaction).tx_blob;

// Submit transaction
const result = await client.submit(signedTransaction);
```


{% /tab %}
{% /tabs %}

### Sending IOU tokens from XRP Ledger to XRPL EVM

Sending IOU tokens (fungible tokens on XRPL) is similar, except the `Amount` field is an [`IssuedCurrencyAmount`](https://js.xrpl.org/interfaces/IssuedCurrencyAmount.html). Below is an example for **100 RLUSD**.

{% tabs %}
{% tab label="Devnet" %}

```ts
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: { // XRPL Devnet IOU whitelisted by Axelar (This is an example, not a real devnet IOU.)
    currency: "524C555344000000000000000000000000000000", // RLUSD (non-standard code)
    issuer: "rMxCKbEDwqr76QuheSUMdEGf4B9xJ8m5De", // RLUSD issuer address
    value: "100", // Amount to transfer
  },
  EvmDestinationAddress: "0x94E9ba2f224d98918a64260F552ba7ce5C6E46e1", // Receiver address.
  // ITS address on the XRPL Devnet
  Destination: "rGAbJZEzU6WaYv5y1LfyN7LBBcQJ3TxsKC",
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
        MemoData: Buffer.from("xrpl-evm-devnet").toString("hex").toUpperCase(),
        MemoType: Buffer.from("destination_chain").toString("hex").toUpperCase(),
      },
    },
    {
      Memo: {
        MemoData: Buffer.from("1000000").toString("hex").toUpperCase(),
        MemoType: Buffer.from("gas_fee_amount").toString("hex").toUpperCase(),
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

{% /tab %}

{% tab label="Testnet" %}


```ts
import { Wallet, Client, Payment, convertStringToHex } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the payment transaction object
const payment: Payment = {
  TransactionType: "Payment",
  Account: wallet.address, // Sender's address
  Amount: { // XRPL Testnet IOU whitelisted by Axelar (This is an example, not a real testnet IOU.)
    currency: "524C555344000000000000000000000000000000", // RLUSD (non-standard code)
    issuer: "rMxCKbEDwqr76QuheSUMdEGf4B9xJ8m5De", // RLUSD issuer address
    value: "100", // Amount to transfer
  },
  // ITS address on the XRPL Testnet
  Destination: "rNrjh1KGZk2jBR3wPfAQnoidtFFYQKbQn2",
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
  ],
};

// Autofill transaction
const transaction = await client.autofill(payment);

// Sign transaction
const signedTransaction = wallet.sign(transaction).tx_blob;

// Submit transaction
const result = await client.submit(signedTransaction);
```

{% /tab %}
{% /tabs %}

---

## Sending assets from XRPL EVM to XRP Ledger

To send assets from the XRPL EVM back to the XRPL, you’ll call the [`interchainTransfer`](https://github.com/axelarnetwork/interchain-token-service/blob/9edc4318ac1c17231e65886eea72c0f55469d7e5/contracts/interfaces/IInterchainTokenStandard.sol#L19) method of the **ITS contract** on the XRPL EVM. You must provide:

- `tokenId`: The token’s Axelar ID.
- `destinationChain`: The Axelar chain ID of the target chain (e.g., `"xrpl"` or `"xrpl-dev"`).
- `destinationAddress`: The address on the XRPL where the assets will be received (an R-address).
- `amount`: The amount to transfer, as an integer without decimals.

### ITS Contract Instantiation

Below is an example of instantiating the ITS contract in the **XRPL EVM Devnet**:

{% tabs %}
{% tab label="Devnet" %}

```ts
import { Contract } from "ethers";

// The signer initialization is omitted for brevity

// Instantiate the ITS contract
const its = new Contract(
  "0x1a7580C2ef5D485E069B7cf1DF9f6478603024d3", // ITS address in XRPL EVM Devnet
  ITS_ABI, // ABI for the ITS contract
  signer
);
```

{% /tab %}
{% tab label="Testnet" %}


```ts
import { Contract } from "ethers";

// The signer initialization is omitted for brevity

// Instantiate the ITS contract
const its = new Contract(
  "0xB5FB4BE02232B1bBA4dC8f81dc24C26980dE9e3C", // ITS address in XRPL EVM Testnet
  ITS_ABI, // ABI for the ITS contract
  signer
);
```

{% /tab %}
{% /tabs %}

### Sending XRP from XRPL EVM to XRP Ledger

{% tabs %}
{% tab label="Devnet" %}

To transfer **XRP** back to the XRPL, call `interchainTransfer` on the ITS contract. Use the **XRP token ID** for Devnet:

```ts
import { ethers } from "ethers";

await its.interchainTransfer(
  "0xbfb47d376947093b7858c1c59a4154dd291d5b2251cb56a6f7159a070f0bd518", // XRP token ID (Devnet)
  "xrpl-dev", // Destination chain ID
  "0xcdaa5ba0215e9359fa62cb5a5650a17b362817ac", // Recipient address on XRP Ledger (r9bSdiUYuAHqqoSuvczxQt5fLoEuNMDZLQ) converted to EVM address
  "100000000000000000000", // 100 XRP in wei
  "0x", // Metadata (unused)
  {
    gasLimit: 8000000,
    value: ethers.utils.parseEther("6"),
  } // Gas
);
```

{% /tab %}

{% tab label="Testnet" %}

To transfer **XRP** back to the XRPL, call `interchainTransfer` on the ITS contract. Use the **XRP token ID** for Testnet:

```ts
import { ethers } from "ethers";

await its.interchainTransfer(
  "0xba5a21ca88ef6bba2bfff5088994f90e1077e2a1cc3dcc38bd261f00fce2824f", // XRP token ID (Testnet)
  "xrpl", // Destination chain ID
  "0xcdaa5ba0215e9359fa62cb5a5650a17b362817ac", // Recipient address on XRP Ledger (r9bSdiUYuAHqqoSuvczxQt5fLoEuNMDZLQ) converted to EVM address
  "100000000000000000000", // 100 XRP in wei
  "0x", // Metadata (unused)
  {
    gasLimit: 8000000,
    value: ethers.utils.parseEther("6"),
  } // Gas
);
```

{% /tab %}
{% /tabs %}

### Sending ERC20 tokens from XRPL EVM to XRP Ledger

When sending ERC20 tokens:

1. **Trust Line**: The recipient must have a trust line for that token on the XRP Ledger.
2. **Approve**: The sender must `approve` the ITS contract to transfer tokens on their behalf.

#### 1. Establish a Trust Line on the XRPL

```ts
import { Wallet, Client, TrustSet } from "xrpl";

// wallet variable corresponds to xrpl.js Wallet instance
// client variable corresponds to xrpl.js Client instance

// Create the trust set transaction object
const trustSet: TrustSet = {
  TransactionType: "TrustSet",
  Account: wallet.address,
  LimitAmount: {
    currency: "524C555344000000000000000000000000000000", // RLUSD
    issuer: "rMxCKbEDwqr76QuheSUMdEGf4B9xJ8m5De", // RLUSD issuer
    value: "100", // >= the amount to be transferred
  },
};

// Autofill transaction
const transaction = await client.autofill(trustSet);

// Sign transaction
const signedTransaction = wallet.sign(transaction).tx_blob;

// Submit transaction
const result = await client.submit(signedTransaction);
```

#### 2. Approve the ITS Contract on the XRPL EVM

{% tabs %}
{% tab label="Devnet" %}

```ts
import { Contract } from "ethers";

// The signer initialization is omitted for brevity

// Instantiate the ERC20 contract
const erc20 = new Contract(
  "0x20937978F265DC0C947AA8e136472CFA994FE1eD", // RLUSD ERC20 token address (example)
  ERC20_ABI,
  signer
);

// Call the approve method
await erc20.approve(
  "0x1a7580C2ef5D485E069B7cf1DF9f6478603024d3", // ITS address in XRPL EVM Devnet
  "100000000000000000000" // >= the amount to be transferred
);
```

{% /tab %}
{% tab label="Testnet" %}

```ts
import { Contract } from "ethers";

// The signer initialization is omitted for brevity

// Instantiate the ERC20 contract
const erc20 = new Contract(
  "0x20937978F265DC0C947AA8e136472CFA994FE1eD", // RLUSD ERC20 token address (example)
  ERC20_ABI,
  signer
);

// Call the approve method
await erc20.approve(
  "0x3b1ca8B18698409fF95e29c506ad7014980F0193", // ITS address in XRPL EVM Testnet
  "100000000000000000000" // >= the amount to be transferred
);
```

{% /tab %}
{% /tabs %}

#### 3. Call `interchainTransfer` on the ITS Contract

{% tabs %}
{% tab label="Devnet" %}

```ts
import { ethers } from "ethers";

// Call the interchainTransfer method
await its.interchainTransfer(
  "0x85f75bb7fd0753565c1d2cb59bd881970b52c6f06f3472769ba7b48621cd9d23", // RLUSD token ID (example)
  "xrpl-dev", // Destination chain ID
  "0xcdaa5ba0215e9359fa62cb5a5650a17b362817ac", // Recipient address on XRP Ledger (r9bSdiUYuAHqqoSuvczxQt5fLoEuNMDZLQ) converted to EVM address
  "100000000000000000000", // 100 RLUSD in integer form
  "0x", // Metadata (unused)
  {
    gasLimit: 8000000,
    value: ethers.utils.parseEther("6"),
  } // Gas
);
```

{% /tab %}

{% tab label="Testnet" %}

```ts
import { ethers } from "ethers";

// Call the interchainTransfer method
await its.interchainTransfer(
  "0x85f75bb7fd0753565c1d2cb59bd881970b52c6f06f3472769ba7b48621cd9d23", // RLUSD token ID (example)
  "xrpl", // Destination chain ID
  "0xcdaa5ba0215e9359fa62cb5a5650a17b362817ac", // Recipient address on XRP Ledger (r9bSdiUYuAHqqoSuvczxQt5fLoEuNMDZLQ) converted to EVM address
  "100000000000000000000", // 100 RLUSD in integer form
  "0x", // Metadata (unused)
  {
    gasLimit: 8000000,
    value: ethers.utils.parseEther("6"),
  }// Gas
);
```

{% /tab %}
{% /tabs %}

---

Below is a high-level overview of how to convert an **XRPL classic address** (e.g., `r...`) to an **EVM‐style hex address** (`0x...`) and vice versa.

---

### Converting **rAddress → EVM (20-byte) Hex** (Needed for XRPLEVM to XRPL transfers)

1. **Base58 Decode**  
   The classic XRP Ledger address (an "rAddress") is a Base58Check‐style encoding. To decode:
   - Remove and check the **type prefix** (typically `0x00` for a normal address).  
   - Remove and verify the **4‐byte checksum** (from the end).  
   - The remaining 20 bytes are the **AccountID**.  

2. **Hex Encode**  
   Once you have the 20‐byte `AccountID`, convert those bytes to a 40‐character hexadecimal string.  

3. **Prepend `0x`** (optional)  
   In EVM contexts, an address typically includes a `0x` prefix to indicate it’s a hex string.  

**Example using `xrpl.js`:**  
```js
import { decodeAccountID } from 'xrpl';

// Suppose rAddress = "rLZ1...your address..."
const accountIDBytes = decodeAccountID(rAddress);  // returns a 20-byte Buffer
// Convert to hex, e.g. "cdaa5ba0215e9359fa62cb5a5650a17b362817ac"
const evmAddress = `0x${accountIDBytes.toString('hex')}`;
```

At this point, `evmAddress` is the EVM‐style address derived from the original XRPL classic address.

---

### Converting **EVM (20-byte) Hex → rAddress** 

1. **Strip `0x`** (if present)  
   If the address starts with `0x`, remove it, leaving just the hex string.

2. **Convert Hex → 20‐Byte Buffer**  
   This is your **AccountID** on XRPL.

3. **Add XRPL Address **Prefix** (`0x00`)**  
   Classic XRP Ledger addresses use a **1‐byte** prefix `0x00`.

4. **Compute a 4‐Byte Checksum**  
   - Perform SHA‐256 on the **prefix + 20-byte AccountID**.  
   - Perform SHA‐256 again on the result.  
   - Take the first 4 bytes of that second SHA‐256 as the checksum.

5. **Concatenate**  
   `(Prefix + 20 bytes of AccountID + 4‐byte checksum)`

6. **Base58‐Encode** using the XRPL alphabet  
   The specific alphabet is:  
   ```
   "rpshnaf39wBUDNEGHJKLM4PQRST7VWXYZ2bcdeCg65jkm8oFqi1tuvAxyz"
   ```

7. **Result**  
   The result is a valid **`rAddress`** that starts with `r` and is typically 25–35 characters long (including its internal checksum).

**Example using `xrpl.js`:**  
```js
import { encodeAccountID } from 'xrpl';

// Suppose evmAddress = "0xcdaa5ba0215e9359fa62cb5a5650a17b362817ac"
const noPrefix = evmAddress.startsWith('0x') ? evmAddress.slice(2) : evmAddress;
const accountIDBytes = Buffer.from(noPrefix, 'hex');

// Encode as an XRP classic address
const rAddress = encodeAccountID(accountIDBytes);  // e.g. "rLZ1..."
```

---

### Summary

- **rAddress → EVM**  
  1. Base58 decode the XRPL address → get 20 bytes  
  2. Convert those 20 bytes to hex  
  3. Optionally add `0x` prefix to get a standard EVM‐style address  

- **EVM → rAddress**  
  1. Strip `0x` prefix  
  2. Parse hex → 20 bytes  
  3. Prepend the **type prefix** (`0x00`)  
  4. Double‐SHA‐256 → first 4 bytes is checksum  
  5. Concatenate  
  6. Base58‐encode → yield the rAddress  

These transformations are **mathematically reversible**, so you can go back and forth safely as long as you do not lose or alter the 20‐byte hash.

---

With these examples, you can send and receive both **XRP** and **IOU/ERC20** tokens across the XRPL and the XRPL EVM using Axelar’s **Interchain Token Service (ITS)**.
