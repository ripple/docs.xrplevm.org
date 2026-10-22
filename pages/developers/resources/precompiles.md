# Precompiles

Precompiles are contracts implemented natively by the node at fixed addresses. On the XRPL EVM they expose standard Ethereum cryptography plus a small set of Cosmos SDK modules to Solidity.

Precompiles have no EVM bytecode, so `eth_getCode` returns `0x` for them even when they are active. To check whether one is active, call one of its functions or read the active list from the chain:

```bash
curl -s https://cosmos-api.xrplevm.org/cosmos/evm/vm/v1/params | jq .params.active_static_precompiles
```

---

## Ethereum precompiles

The XRPL EVM runs the **Prague** fork, so the standard Ethereum precompiles are available at their usual addresses, including the BLS12-381 precompiles (`0x0b`–`0x11`) introduced in Prague and the KZG point evaluation precompile (`0x0a`).

---

## Static precompiles

The following static precompiles are **active on Mainnet, Testnet and Devnet**:

| Precompile | Address                                      | Purpose                                                                 |
| ---------- | -------------------------------------------- | ----------------------------------------------------------------------- |
| P256       | `0x0000000000000000000000000000000000000100` | secp256r1 signature verification ([RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md)), for passkeys and WebAuthn |
| Bech32     | `0x0000000000000000000000000000000000000400` | Convert between `0x` hex addresses and `ethm1...` Bech32 addresses      |
| Bank       | `0x0000000000000000000000000000000000000804` | Read Cosmos bank balances and supply                                    |
| Gov        | `0x0000000000000000000000000000000000000805` | Query governance proposals and parameters                               |

The other Cosmos EVM static precompiles are **not active** on any XRPL EVM network: Staking (`0x…0800`), Distribution (`0x…0801`), ICS20 (`0x…0802`) and Slashing (`0x…0806`). The Vesting address (`0x…0803`) is reserved but has no implementation in the current node release. Calls to any of them return empty data. The XRPL EVM uses a Proof-of-Authority validator set, so there is no delegation or staking flow to expose to contracts.

{% admonition type="info" name="Proposed change" %}
An open, unreleased proposal for the next major node version ([xrplevm/node#165](https://github.com/xrplevm/node/pull/165)) would remove the Gov precompile from the active list, leaving P256, Bech32 and Bank. If you are building new contracts on the Gov precompile, follow that proposal before relying on it.
{% /admonition %}

---

## ERC-20 precompiles

Every registered token pair gets an ERC-20 precompile, so a Cosmos coin can be used from Solidity like any ERC-20 token. The most important one is native XRP:

| Token | ERC-20 address                               | Cosmos denom |
| ----- | -------------------------------------------- | ------------ |
| XRP   | `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` | `axrp`       |

See [Using XRP as an ERC-20](../interacting-with-evm/advanced-guides/using-xrp-as-wrapped-erc20.md) for details.

IBC assets received over a channel also have an ERC-20 precompile. List the registered pairs with:

```bash
curl -s "https://cosmos-api.xrplevm.org/cosmos/evm/erc20/v1/token_pairs?pagination.limit=100" | jq .token_pairs
```

Registering new token pairs is restricted to governance on Mainnet and Testnet (`permissionless_registration` is `false` since proposal #35). It remains open on Devnet.

---

## Limits

A single transaction can make at most **20 calls to precompiles** (static and ERC-20 precompiles combined). The 21st call fails with `max calls to precompiles (20) reached`. Keep this in mind for aggregators and multi-hop routes that touch `0xEeee…EeEe` many times.
