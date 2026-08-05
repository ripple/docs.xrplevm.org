---
blurb: Wormhole interoperability on the XRPL EVM Sidechain.
labels:
  - Interoperability
---

# Wormhole

[Wormhole](https://wormhole.com/) is a decentralized interoperability platform that connects the **XRPL EVM Sidechain** with more than 40 blockchain networks, including Ethereum, Solana, Base, Arbitrum, and the wider EVM ecosystem. Wormhole is **live on XRPL EVM**, giving EVM developers cross-chain messaging, token transfers, and multichain token issuance out of the box.

Because the XRPL EVM is a fully EVM-compatible chain, every Wormhole EVM integration path — Solidity contracts, the TypeScript SDK, and the Connect widget — works on XRPL EVM the same way it does on any other EVM network.

## How Wormhole Works

- **Guardians**: A network of 19 independent validators observes messages emitted by the Wormhole **Core Contract** on each connected chain. When at least 13 of 19 Guardians sign an observation, it becomes a **VAA**.
- **VAAs (Verifiable Action Approvals)**: The signed attestation of an observed message. A VAA is a portable cryptographic proof that can be verified by the Core Contract on any destination chain.
- **Executor**: Wormhole's shared execution framework for message delivery. Integrators request a delivery quote off-chain, publish their message, and request execution on-chain; independent relay providers then compete to deliver the VAA to the destination chain. Unless you are building a low-level integration, the Wormhole SDK manages quoting and execution for you.
- **WormholeScan**: The [Wormhole explorer](https://wormholescan.io) for tracking cross-chain messages, VAAs, and Guardian activity. Testnet transactions are available at `https://wormholescan.io/#/tx/<txHash>?network=Testnet`.

Read more in the [Wormhole architecture documentation](https://wormhole.com/docs/protocol/architecture/).

## XRPL EVM on Wormhole

| Property          | Value                                     |
| ----------------- | ----------------------------------------- |
| Wormhole chain ID | `57` (both Mainnet and Testnet)           |
| SDK chain name    | `XRPLEVM`                                 |
| EVM chain ID      | `1440000` (Mainnet) / `1449000` (Testnet) |

{% admonition type="info" name="Wormhole chain IDs are not EVM chain IDs" %}
Wormhole assigns its own chain IDs to every connected network. When calling Wormhole contracts or the SDK, use the Wormhole chain ID `57` for XRPL EVM — not the EVM chain ID (`1440000`/`1449000`) you use in MetaMask or JSON-RPC calls. See the full [Wormhole chain ID reference](https://wormhole.com/docs/products/reference/chain-ids/).
{% /admonition %}

### Supported Products

| Product                                                         | Mainnet | Testnet | Use it for                                             |
| --------------------------------------------------------------- | ------- | ------- | ------------------------------------------------------ |
| [Messaging (Core Contract)](./send-messages.md)                 | ✅      | ✅      | Arbitrary cross-chain messages between smart contracts |
| [Wrapped Token Transfers (WTT)](./transfer-tokens.md)           | ✅      | ✅      | Bridging existing ERC-20 tokens (lock-and-mint)        |
| [Native Token Transfers (NTT)](./native-token-transfers.md)     | ✅      | ✅      | Multichain-native tokens without wrapped assets        |
| [Connect widget](./connect-widget.md)                           | ✅      | ❌      | Drop-in bridging UI for your dApp                      |
| [Queries](https://wormhole.com/docs/products/queries/overview/) | ✅      | ✅      | Guardian-attested on-chain data reads (`eth_call`)     |

### XRP Ledger (XRPL) Support

Wormhole has [announced integration with the XRP Ledger mainnet](https://wormhole.com/blog/ripple-expands-multichain-interoperability-infrastructure-with-wormhole) (Wormhole chain ID `66`), covering XRP, Issued Assets (IOUs), and Multi-Purpose Tokens (MPTs). As of this writing, the XRPL (non-EVM) route is still rolling out and is not yet covered by the Wormhole product documentation — for live XRPL ↔ XRPL EVM transfers today, use the [Axelar bridge](../interchain-transfer.md).

## Developer Guides

- [Deployed Contracts](./deployed-contracts.md): Wormhole contract addresses on XRPL EVM Mainnet and Testnet.
- [Send Cross-Chain Messages](./send-messages.md): Publish and receive messages from Solidity with the Core Contract.
- [Transfer Tokens](./transfer-tokens.md): Bridge ERC-20 tokens with the TypeScript SDK or Portal Bridge.
- [Native Token Transfers](./native-token-transfers.md): Deploy a multichain-native token with the NTT framework.
- [Connect Widget](./connect-widget.md): Embed a bridging UI in your React app.
