# Address translation

An Ethereum account can be represented in two formats:

- **Bech32 (ethm...)** for Cosmos compatibility.
- **EIP55 Hex (0x...)** for Ethereum's Web3 tooling compatibility.

## Bech32 to EIP55 Hex address

Transforming a Bech32 address to an EIP55 Hex address involves the following steps:

1. Decode the Bech32 address to Bech32 words.
2. Extract the raw bytes from the Bech32 words.
3. Convert the raw bytes into a hexadecimal string.
4. Apply the [EIP55 checksum](https://eips.ethereum.org/EIPS/eip-55) to the hexadecimal string.

## EIP55 Hex to Bech32 address

Transforming an EIP55 Hex address to a Bech32 address involves the following steps:

1. Remove the `0x` prefix from the EIP55 Hex address.
2. Extract the raw bytes from the hexadecimal string.
3. Convert the raw bytes into Bech32 words.
4. Encode the Bech32 words as a Bech32 address.

## Translate addresses using the `exrpd` CLI

Addresses can be translated using the [`exrpd` CLI](../../operators/guides/interacting-with-the-node-cli.md).

```bash
exrpd debug addr ethm1akwntffy4us9nhgcmgjxdg78v5w3xtwletyjmv
```

or

```bash
exrpd debug addr 0xed9D35A524AF2059dd18Da2466A3C7651D132Ddf
```

Example output:

```raw
Address: [237, 157,  53, 165,  36, 175, 32,  89, 221,  24, 218,  36, 102, 163, 199, 101,  29,  19, 45, 223]
Address (hex): ED9D35A524AF2059DD18DA2466A3C7651D132DDF
Bech32 Acc: ethm1akwntffy4us9nhgcmgjxdg78v5w3xtwletyjmv
Bech32 Val: ethmvaloper1akwntffy4us9nhgcmgjxdg78v5w3xtwlkmw7r3
```

## Translate addresses programmatically

The address translation can be implemented in a programming language. Below is an example in Typescript, using the [`bech32`](https://github.com/bitcoinjs/bech32) and [`js-sha3`](https://github.com/emn178/js-sha3) libraries.

```typescript
import { bech32 } from "bech32";
import { keccak256 } from "js-sha3";

// Bech32 to EIP55 Hex
function bech32ToEIP55(bech32Address: string): string {
  const decoded = bech32.decode(bech32Address);
  const data = bech32.fromWords(decoded.words);
  const hexAddress = Buffer.from(data).toString("hex");

  return toChecksumAddress("0x" + hexAddress);
}

// EIP55 Hex to Bech32
function eip55ToBech32(hexAddress: string): string {
  const cleanHex = hexAddress.substring(2).toLowerCase();
  const rawBytes = Buffer.from(cleanHex, "hex");
  const words = bech32.toWords(rawBytes);

  return bech32.encode("ethm", words);
}

// EIP55 Checksum Calculation
function toChecksumAddress(address: string): string {
  const lowerAddress = address.toLowerCase().replace(/^0x/, "");
  const hash = keccak256(lowerAddress);

  let checksumAddress = "0x";

  for (let i = 0; i < lowerAddress.length; i++) {
    if (parseInt(hash[i], 16) >= 8) {
      checksumAddress += lowerAddress[i].toUpperCase();
    } else {
      checksumAddress += lowerAddress[i];
    }
  }

  return checksumAddress;
}
```
