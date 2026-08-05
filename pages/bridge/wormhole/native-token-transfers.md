---
blurb: Deploy a multichain-native token on XRPL EVM with Wormhole NTT.
labels:
  - Interoperability
---

# Native Token Transfers (NTT)

Wormhole's **Native Token Transfers (NTT)** framework lets token issuers deploy their token natively on the XRPL EVM and other chains — no wrapped assets involved. Your project keeps full ownership and upgrade authority of the token contracts on every chain, while NTT handles cross-chain movement of supply.

NTT is available on XRPL EVM **Mainnet and Testnet**.

## Transfer Modes

- **Burn-and-mint**: Tokens are burned on the source chain and minted on the destination, distributing total supply across chains. Best for new tokens designed to be multichain-native.
- **Hub-and-spoke**: Tokens are locked on a central "hub" chain and minted as equivalents on "spoke" chains, keeping total supply on the hub. Best for existing tokens whose contracts cannot be modified.

## Key Components

- **NttManager**: The per-chain contract that manages transfers, rate limits, and access control.
- **Transceivers**: Pluggable message transport; the default transceiver routes through the Wormhole Core Contract.
- **Rate limiting**: Configurable per chain and per period, inbound and outbound.
- **Global Accountant**: Guardian-enforced invariant that tokens burned or transferred out never exceed tokens minted.

## NTT vs. WTT

|                    | NTT                                                                         | WTT                                         |
| ------------------ | --------------------------------------------------------------------------- | ------------------------------------------- |
| Asset model        | Native token on every chain                                                 | Wrapped representation                      |
| Contract ownership | Your project                                                                | Wormhole contracts                          |
| Token requirements | `mint`/`burn` (burn-and-mint mode)                                          | Any ERC-20                                  |
| Best for           | Stablecoins, governance tokens, institutional assets, full-control projects | Consumer apps, games, fast managed bridging |

If you just need to move an existing ERC-20 across chains, use [Wrapped Token Transfers](./transfer-tokens.md). If you are launching a token that should be canonical on XRPL EVM and other chains, use NTT.

## Deploy NTT to XRPL EVM

The [NTT CLI](https://wormhole.com/docs/products/token-transfers/native-token-transfers/get-started/) drives the deployment. The XRPL EVM's chain name in Wormhole tooling is **`XRPLEVM`**.

### 1. Install the CLI

```bash
curl -fsSL https://raw.githubusercontent.com/wormhole-foundation/native-token-transfers/main/cli/install.sh | bash
ntt --version
```

### 2. Create and Initialize a Project

```bash
ntt new my-ntt-project
cd my-ntt-project
ntt init Mainnet   # or: ntt init Testnet
```

This creates a `deployment.json` that tracks your NTT configuration across chains.

### 3. Deploy to Your Chains

Export a funded private key, then add each chain. For burn-and-mint mode on XRPL EVM plus Ethereum:

```bash
export ETH_PRIVATE_KEY=INSERT_PRIVATE_KEY

ntt add-chain XRPLEVM --latest --mode burning --token INSERT_YOUR_TOKEN_ADDRESS
ntt add-chain Ethereum --latest --mode burning --token INSERT_YOUR_TOKEN_ADDRESS
```

For hub-and-spoke, deploy the hub chain with `--mode locking` and the spokes with `--mode burning`.

### 4. Grant Mint Authority

On burn-and-mint chains, your token must expose `mint(address,uint256)` and `burn(uint256)` (see the [`INttToken` interface](https://wormhole.com/docs/products/token-transfers/native-token-transfers/guides/deploy-to-evm/)), and the NttManager needs minting rights:

```bash
cast send $TOKEN_ADDRESS "setMinter(address)" $NTT_MANAGER_ADDRESS \
  --private-key $ETH_PRIVATE_KEY --rpc-url https://rpc.xrplevm.org
```

Hub chains only need a standard ERC-20.

### 5. Verify and Sync Configuration

```bash
ntt status   # checks deployment.json against on-chain state
ntt pull     # syncs local config from on-chain state
ntt push     # applies local config changes (e.g. rate limits) on-chain
```

## Next Steps

- [NTT overview](https://wormhole.com/docs/products/token-transfers/native-token-transfers/overview/)
- [Deploy NTT to EVM chains](https://wormhole.com/docs/products/token-transfers/native-token-transfers/guides/deploy-to-evm/)
- [Connect Widget](./connect-widget.md): give users a UI for your NTT token.
