---
blurb: Transfer tokens between XRPL EVM and other chains with Wormhole.
labels:
  - Interoperability
---

# Transfer Tokens with Wormhole

Wormhole's **Wrapped Token Transfers (WTT)**, formerly known as the Token Bridge, moves ERC-20 tokens between the XRPL EVM and 30+ connected chains using a lock-and-mint model:

1. **Attestation (one-time)**: The token's metadata (symbol, name, decimals) is registered on the destination chain.
2. **Lock**: Tokens are locked in custody by the WTT contract on the source chain.
3. **Attest**: The Guardian network emits a signed VAA for the transfer.
4. **Mint / Release**: The VAA is verified on the destination chain, minting wrapped tokens (or releasing native ones on the way back). Wrapped tokens are backed 1:1.

WTT is available on XRPL EVM **Mainnet and Testnet**. Contract addresses are listed in [Deployed Contracts](./deployed-contracts.md).

## Option 1: Portal Bridge (UI)

For manual transfers, use [Portal Bridge](https://portalbridge.com), the official user-facing app built on Wormhole. Connect your wallet, pick the source and destination chains, and follow the prompts.

## Option 2: TypeScript SDK (Programmatic)

### Install

```bash
npm install @wormhole-foundation/sdk
npm install -D tsx typescript
```

### Set Up a Signer Helper

```typescript
// helper.ts
import {
  ChainAddress,
  ChainContext,
  Network,
  Signer,
  Wormhole,
  Chain,
  isTokenId,
  TokenId,
} from "@wormhole-foundation/sdk";
import evm from "@wormhole-foundation/sdk/evm";

export async function getSigner<N extends Network, C extends Chain>(
  chain: ChainContext<N, C>,
): Promise<{
  chain: ChainContext<N, C>;
  signer: Signer<N, C>;
  address: ChainAddress<C>;
}> {
  const signer = await (
    await evm()
  ).getSigner(await chain.getRpc(), process.env.EVM_PRIVATE_KEY!);

  return {
    chain,
    signer: signer as Signer<N, C>,
    address: Wormhole.chainAddress(chain.chain, signer.address()),
  };
}

export async function getTokenDecimals<N extends Network>(
  wh: Wormhole<N>,
  token: TokenId,
  chain: ChainContext<N, any>,
): Promise<number> {
  return isTokenId(token)
    ? Number(await wh.getDecimals(token.chain, token.address))
    : chain.config.nativeTokenDecimals;
}
```

### Transfer from XRPL EVM

The XRPL EVM's chain name in the Wormhole SDK is **`XRPLEVM`**:

```typescript
// transfer.ts
import { wormhole, amount, Wormhole } from "@wormhole-foundation/sdk";
import evm from "@wormhole-foundation/sdk/evm";
import { getSigner, getTokenDecimals } from "./helper";

(async function () {
  const wh = await wormhole("Mainnet", [evm]);

  const sendChain = wh.getChain("XRPLEVM");
  const rcvChain = wh.getChain("Ethereum");

  const source = await getSigner(sendChain);
  const destination = await getSigner(rcvChain);

  // Pass the address of the ERC20 token you want to bridge.
  // To bridge XRP itself, use its ERC20 representation at the
  // sentinel address 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE.
  const tokenId = Wormhole.tokenId(
    "XRPLEVM",
    "0xINSERT_YOUR_ERC20_TOKEN_ADDRESS",
  );
  const amt = "1";

  const decimals = await getTokenDecimals(wh, tokenId, sendChain);
  const transferAmount = amount.units(amount.parse(amt, decimals));

  const xfer = await wh.tokenTransfer(
    tokenId,
    transferAmount,
    source.address,
    destination.address,
    "TokenBridge", // WTT route (the SDK still uses the legacy route name)
    undefined,
  );

  console.log("Starting Transfer");
  const srcTxids = await xfer.initiateTransfer(source.signer);
  console.log("Started Transfer:", srcTxids);

  console.log("Fetching Attestation");
  const timeout = 5 * 60 * 1000;
  await xfer.fetchAttestation(timeout);

  console.log("Completing Transfer");
  const destTxids = await xfer.completeTransfer(destination.signer);
  console.log("Completed Transfer:", destTxids);

  process.exit(0);
})();
```

Run it with:

```bash
npx tsx transfer.ts
```

This is a **manual** transfer: your script initiates the transfer on XRPL EVM, waits for the Guardian attestation, and completes it on the destination chain with the destination signer. For development, switch `"Mainnet"` to `"Testnet"` and fund your account from the [faucet](../../users/faucet.md).

{% admonition type="warning" name="Do not use the native token ID for XRP" %}
The WTT contract on XRPL EVM has no native token wrapping configured (its `WETH()` slot is unset), so `Wormhole.tokenId("XRPLEVM", "native")` transfers revert on-chain. Always pass an **ERC20 token address**. XRP itself is bridgeable this way: it is natively exposed as an [ERC20 at the sentinel address](../../developers/interacting-with-evm/advanced-guides/using-xrp-as-wrapped-erc20.md) `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`, which is also how the Connect widget and Portal Bridge route XRP.
{% /admonition %}

{% admonition type="info" name="First transfer of a token" %}
A token must be **attested** on the destination chain once before it can be transferred there. If your token has never been bridged to the target chain, follow the [attestation guide](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/guides/attest-tokens/) first.
{% /admonition %}

## Tokens Available on XRPL EVM

Wormhole moves tokens through two mechanisms: WTT (wrapped assets, listed in this guide) and [NTT](./native-token-transfers.md) (native multichain deployments). The registries below reflect the on-chain state as of August 2026; any team can attest new WTT tokens or deploy new NTT tokens permissionlessly, so check [WormholeScan](https://wormholescan.io) for the current list.

{% tabs %}
{% tab label="Mainnet" %}

| Token                          | Address on XRPL EVM                                         | Mechanism                                   | Connected chains                   |
| ------------------------------ | ----------------------------------------------------------- | ------------------------------------------- | ---------------------------------- |
| XRP                            | `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` (native ERC20) | WTT                                         | Any WTT chain (attest first)       |
| RLUSD                          | `0x8d58C0C60B8D6b88Fa98B291a646dB34d0F98258`                | NTT (burn and mint)                         | Ethereum, Base, Optimism, Unichain |
| USDC.e (Wormhole-Bridged USDC) | `0xf75339EfD56B1da680BD4dE0f4086f24499275E7`                | NTT (hub on Ethereum, locks canonical USDC) | Ethereum                           |
| WETH                           | `0xccD433A96A4DE148596F4E31Ab8ad1348077590B`                | WTT (wrapped from Ethereum)                 | Ethereum                           |

The RLUSD and USDC.e deployments are mutually peered with their Ethereum managers on-chain: the RLUSD peer manages the canonical `RLUSD` contract and the USDC.e hub locks Circle's canonical `USDC`.

{% /tab %}
{% tab label="Testnet" %}

| Token                    | Address on XRPL EVM Testnet                                                                | Mechanism | Origin       |
| ------------------------ | ------------------------------------------------------------------------------------------ | --------- | ------------ |
| XRP                      | `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` (native ERC20)                                | WTT       | Native       |
| WETH                     | `0xDfC1e7C7D02C282B5C28Ad88Dd5A78dfC214E071`                                               | WTT       | Sepolia      |
| WETH                     | `0xDC403F8a55A4B6f64A298128888eBA70FcD6B683`                                               | WTT       | Base Sepolia |
| RLUSD (test deployments) | `0x3EC39dCE6Fd2FF17927FD0e4AF6C7dd9faBe80F7`, `0xA8d886aEe2d690c8D4aaCCa93861b5fe88907FBB` | NTT       | Test         |

Testnet also hosts assorted NTT test tokens. To bridge a token that is not listed, [attest it](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/guides/attest-tokens/) first or deploy it with [NTT](./native-token-transfers.md).

{% /tab %}
{% /tabs %}

## Next Steps

- [WTT overview](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/overview/)
- [WTT get-started guide](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/get-started/)
- [Native Token Transfers](./native-token-transfers.md): issue a multichain-native token instead of a wrapped one.
- [Track transfers on WormholeScan](https://wormholescan.io)
