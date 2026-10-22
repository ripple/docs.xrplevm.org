# Network Parameters and Status

This page summarizes the on-chain parameters that matter when building on the XRPL EVM, the places where its EVM behaves differently from Ethereum, and recent network changes.

---

## Network parameters

| Parameter                     | Mainnet              | Testnet              | Devnet                |
| ----------------------------- | -------------------- | -------------------- | --------------------- |
| EVM chain ID                  | `1440000`            | `1449000`            | `1449900`             |
| Cosmos chain ID               | `xrplevm_1440000-1`  | `xrplevm_1449000-1`  | `xrplevm_1449900-1`   |
| Node version                  | `v11.2.0`            | `v11.1.1`            | `v11.0.0-rc.1`        |
| EVM fork                      | Prague               | Prague               | Prague                |
| Average block time            | ~5.7 s               | ~5.9 s               | ~5.3 s                |
| Block gas limit               | 21,000,000           | 21,000,000           | Unlimited             |
| Base fee / minimum gas price  | 571,000,000 wei (0.571 gwei) | 571,000,000 wei (0.571 gwei) | 571,000,000 wei (0.571 gwei) |
| Native denom                  | `axrp` (18 decimals) | `axrp` (18 decimals) | `axrp` (18 decimals)  |
| Unbonding time                | 7 days               | 7 days               | 7 days                |
| Governance voting period      | 14 days (1 day expedited) | 14 days (1 day expedited) | 5 minutes (1 minute expedited) |
| Permissionless ERC-20 registration | Disabled        | Disabled             | Enabled               |
| Interchain Accounts (ICS-27)  | Disabled             | Disabled             | Disabled              |

`eth_gasPrice` returns the base fee plus a suggested priority fee, so it is higher than the minimum gas price. Read live values with `cast gas-price --rpc-url <rpc>` or from `GET /cosmos/evm/feemarket/v1/params` on the Cosmos REST API.

---

## EVM behavior differences

The XRPL EVM runs Cosmos EVM on CometBFT, which has no blobs and no beacon chain. A few block-context values therefore behave differently from Ethereum:

| Feature                               | Behavior on XRPL EVM                                                                                     |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `block.blobbasefee` (`BLOBBASEFEE`, `0x4a`) | Not supported. Any call that executes this opcode fails, including `eth_call` and `eth_estimateGas`. Do not use it. |
| `blobhash(i)` (`BLOBHASH`)            | Always returns `0`.                                                                                      |
| `block.prevrandao` (`PREVRANDAO`)     | Returns a constant (`0xff…ff`). It is **not** a source of randomness. Use an oracle or commit-reveal scheme instead. |
| EIP-2935 history contract (`0x0000F90827F1C53a10cb7A02335B175320002935`) | Can return the hash of a different block. Use the `BLOCKHASH` opcode (`blockhash(n)`), which returns correct hashes for recent blocks. |
| `CLZ` opcode (Osaka)                  | Not available: the chain runs Prague, not Osaka. Compile with `evmVersion` set to `prague` or lower. |

See also [Precompiles](./precompiles.md) for the native modules available to contracts and the per-transaction precompile call limit.

---

## Recent changes

### Mainnet

| Date (UTC)  | Change |
| ----------- | ------ |
| 2026-09-23  | Upgrade to **v11.2.0** at block `7823200` (proposal #38): Interchain Accounts host lockdown in the upgrade handler, unbonding time set to 7 days, XRP stranded in the Elys channel escrow returned to its withdrawal address. |
| 2026-09-17  | Interchain Accounts host module disabled (proposal #36). Injective IBC light client recovered (proposal #37). |
| 2026-09-15  | Node **v10.2.1** released with a further cosmos/evm security fix (binary replacement, no governance upgrade). |
| 2026-08-23  | Preventive halt from 05:02 to 19:19 UTC (blocks `7360028` → `7360029`) after a Cosmos EVM security advisory. Block production resumed with patched node **v10.2.0**. No funds were lost. See [the incident report](https://www.xrplevm.org/blog/xrpl-evm-preventive-response-to-cosmos-evm-incident). |
| 2026-08-15  | Permissionless ERC-20 token pair registration disabled (proposal #35). |
| 2026-07-20  | Upgrade to **v10.1.0** at block `6856000` (proposal #33): cosmos/evm fix. |

### Testnet

| Date (UTC)  | Change |
| ----------- | ------ |
| 2026-09-08  | Node **v11.1.1** released (binary replacement, no governance upgrade). |
| 2026-08-14  | Upgrade to **v11.1.0** at block `8158200` (proposal #49): Interchain Accounts host lockdown in the upgrade handler, unbonding time set to 7 days. |

For node versions, upgrade heights and binaries, see the operator [Networks](../../operators/resources/networks.md) page. You can follow governance proposals on the [governance dashboards](https://governance.xrplevm.org) or with `GET /cosmos/gov/v1/proposals` on the Cosmos REST API.
