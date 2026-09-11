---
seo:
  description: "Tokens Wormhole supports on the XRPL EVM Sidechain, including XRP, RLUSD, USDC.e and WETH, with addresses, transfer mechanism and connected chains."
---

# Wormhole Supported Tokens

Wormhole moves tokens through two mechanisms: WTT (wrapped assets, covered in the [transfer guide](../../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/transfer-tokens.md)) and [NTT](../../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/native-token-transfers.md) (native multichain deployments). The registries below reflect the on-chain state as of August 2026; any team can attest new WTT tokens or deploy new NTT tokens permissionlessly, so check [WormholeScan](https://wormholescan.io) for the current list.

{% tabs %}
{% tab label="Mainnet" %}

| Token                          | Address on XRPL EVM                                         | Mechanism                                   | Connected chains                   |
| ------------------------------ | ----------------------------------------------------------- | ------------------------------------------- | ---------------------------------- |
| XRP                            | `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` (native ERC20) | WTT                                         | Any WTT chain (attest first)       |
| RLUSD                          | `0x8d58C0C60B8D6b88Fa98B291a646dB34d0F98258`                | NTT (burn-and-mint)                         | Ethereum, Base, Optimism, Unichain |
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

Testnet also hosts assorted NTT test tokens. To bridge a token that is not listed, [attest it](https://wormhole.com/docs/products/token-transfers/wrapped-token-transfers/guides/attest-tokens/) first or deploy it with [NTT](../../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/native-token-transfers.md).

{% /tab %}
{% /tabs %}
