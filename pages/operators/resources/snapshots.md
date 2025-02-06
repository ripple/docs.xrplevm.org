# Snapshots

Snapshots provide a quick way to bootstrap your node’s state by downloading a pre-synchronized version of the blockchain data. This reduces the time and resources required to sync from the genesis block. Different networks—mainnet, testnet, and devnet—offer snapshots for various use cases. Some snapshots are pruned (minimal historical data), others retain default state, and some include full historical archives for comprehensive analysis.

## Devnet Snapshots

| Provider | URL                                                                                                | Type    |
|----------|----------------------------------------------------------------------------------------------------|---------|
| Peersyst | [Download](https://evm-sidechain-snapshots-devnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4)      | Default |
| Enigma   | [Download](https://enigma-validator.com/stake-with-us/xrp-testnet#services)                        | Pruned  |
| Polkachu | [Download](https://polkachu.com/testnets/xrp/snapshots)                                            | Default |

---

**Note:** Always verify the authenticity and integrity of snapshots before using them. Keep in mind that third-party providers may have different update schedules, data policies, and retention periods. Refer to the provider’s documentation for instructions on how to use these snapshots and any associated terms of service.
