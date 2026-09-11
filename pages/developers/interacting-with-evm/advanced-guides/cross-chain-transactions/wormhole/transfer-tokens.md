# Transfer Tokens with Wormhole

Wormhole's **Wrapped Token Transfers (WTT)**, formerly known as the Token Bridge, moves ERC-20 tokens between the XRPL EVM and the 30+ chains where WTT is deployed, using a lock-and-mint model:

1. **Token registration (one-time)**: The token's metadata (symbol, name, decimals) is attested and registered on the destination chain.
2. **Lock**: Tokens are locked in custody by the WTT contract on the source chain.
3. **Observe and sign**: The Guardian network emits a signed VAA for the transfer.
4. **Mint / Release**: The VAA is verified on the destination chain, minting wrapped tokens (or releasing native ones on the way back). Wrapped tokens are backed 1:1.

WTT is available on XRPL EVM **Mainnet and Testnet**. Contract addresses are listed in [Deployed Contracts](../../../../../bridge/wormhole/deployed-contracts.md).

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

This is a **manual** transfer: your script initiates the transfer on XRPL EVM, waits for the Guardian attestation, and completes it on the destination chain with the destination signer. For development, switch `"Mainnet"` to `"Testnet"` and fund your account from the [faucet](../../../../../users/faucet.md).

{% admonition type="warning" name="Do not use the native token ID for XRP" %}
The WTT contract on XRPL EVM has no native token wrapping configured (its `WETH()` slot is unset), so `Wormhole.tokenId("XRPLEVM", "native")` transfers revert on-chain. Always pass an **ERC20 token address**. XRP itself is bridgeable this way: it is natively exposed as an [ERC20 at the sentinel address](../../using-xrp-as-wrapped-erc20.md) `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`, which is also how the Connect widget and Portal Bridge route XRP. Mind the sentinel address usage limits documented in that guide when batching transfers.
{% /admonition %}

{% admonition type="info" name="First transfer of a token" %}
A token must be **attested** on the destination chain once before it can be transferred there. If your token has never been bridged to the target chain, follow the [attestation guide](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/guides/attest-tokens/) first.
{% /admonition %}

## Next Steps

- [Supported Tokens on XRPL EVM](../../../../../bridge/wormhole/supported-tokens.md): the tokens currently live on each network.
- [WTT overview](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/overview/)
- [WTT get-started guide](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/get-started/)
- [Native Token Transfers](./native-token-transfers.md): issue a multichain-native token instead of a wrapped one.
- [Track transfers on WormholeScan](https://wormholescan.io)
