# Utility Contracts

Several widely used Ethereum infrastructure contracts are deployed on the XRPL EVM at their canonical addresses, so tooling that expects them (for example, Foundry, viem, Safe and ERC-4337 bundlers) works without extra configuration.

| Contract                                   | Address                                      | Mainnet | Testnet | Devnet |
| ------------------------------------------ | -------------------------------------------- | :-----: | :-----: | :----: |
| Multicall3                                 | `0xcA11bde05977b3631167028862bE2a173976CA11` |   Yes   |   Yes   |  Yes   |
| Permit2                                    | `0x000000000022D473030F116dDEE9F6B43aC78BA3` |   Yes   |   Yes   |  Yes   |
| Deterministic deployment proxy (CREATE2)   | `0x4e59b44847b379578588920cA78FbF26c0B4956C` |   Yes   |   Yes   |  Yes   |
| CreateX                                    | `0xba5Ed099633D3B313e4D5F7bdc1305d3c28ba5Ed` |   Yes   |   Yes   |   No   |
| Safe singleton factory                     | `0x914d7Fec6aaC8cd542e72Bca78B30650d45643d7` |   Yes   |   Yes   |   No   |
| Safe 1.4.1 (L2 singleton)                  | `0x29fcB43b46531BcA003ddC8FCB67FFE91900C762` |   Yes   |   Yes   |   No   |
| Safe 1.4.1 proxy factory                   | `0x4e1DCf7AD4e460CfD30791CCC4F9c8a4f820ec67` |   Yes   |   Yes   |   No   |
| ERC-4337 EntryPoint v0.8                   | `0x4337084D9E255Ff0702461CF8895CE9E3b5Ff108` |   Yes   |   Yes   |   No   |
| ERC-4337 EntryPoint v0.7                   | `0x0000000071727De22E5E9d8BAf0edAc6f37da032` |   Yes   |   No    |   No   |
| WXRP (WETH9 fork)                          | `0x7C21a90E3eCD3215d16c3BBe76a491f8f792d4Bf` |   Yes   |   No    |   No   |

ERC-4337 EntryPoint v0.6, ERC-1820 and Safe 1.3.0 are **not** deployed.

To confirm a deployment yourself, check that the address has code:

```bash
cast code 0xcA11bde05977b3631167028862bE2a173976CA11 --rpc-url https://rpc.xrplevm.org
```

For bridge contracts, see [Axelar deployed contracts](../../bridge/deployed-contracts.md) and [Wormhole deployed contracts](../../bridge/wormhole/deployed-contracts.md). For native modules exposed to Solidity, see [Precompiles](./precompiles.md).
