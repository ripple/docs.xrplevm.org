# Snapshots

Snapshots provide a quick way to bootstrap your node’s state by downloading a pre-synchronized version of the blockchain data. This reduces the time and resources required to sync from the genesis block. Different networks—mainnet, testnet, and devnet—offer snapshots for various use cases. Some snapshots are pruned (minimal historical data), others retain default state, and some include full historical archives for comprehensive analysis.

## Snapshots

{% tabs %}
{% tab label="Devnet" %}

| Provider     | URL                                                                                         | Type    |
| --------     | ------------------------------------------------------------------------------------------- | ------- |
| **Peersyst** | [Download](https://evm-sidechain-snapshots-devnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4) | Default |
| **Enigma**   | [Download](https://enigma-validator.com/stake-with-us/xrp-testnet#services)                 | Pruned  |
| **Polkachu** | [Download](https://polkachu.com/testnets/xrp/snapshots)                                     | Default |

---
{% /tab %}
{% tab label="Testnet" %}

| Provider | URL                                                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Peersyst**              | [Download](https://evm-sidechain-snapshots-testnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4)                   |
| **Kintsugi Nodes**        | [Download](http://kintsugi-nodes.com/ripple/snapshot)                                                          |
| **Polkachu**              | [Download](https://polkachu.com/testnets/xrp/snapshots)                                                        |
| **Cumulo**                | [Download](https://cumulo.pro/services/xrplevm/)                                                               |
| **DeFatRat (TechHubs)**   | [Download](https://xrpl-testnet-snapshots.techhubs.asia/)                                                      |
| **Enigma**                | [Download](https://services.enigma-validator.com/xrp/xrp_365040.tar.lz4)                                       |
| **ITRocket**              | [Download](https://itrocket.net/services/testnet/xrplevm/)                                                     |
| **LuckyStar**             | [Download](https://luckystar-1.gitbook.io/luckystar.asia/testnet/cosmos-eco/xrpl/snapshot)                     |
| **MictoNode**             | [Download](https://services.mictonode.com/xrpl-evm/snapshot)                                                   |
| **UTSA (lesnik)**         | [Download](https://utsa.gitbook.io/services/testnet/xrpl-evm/snapshots)                                        |
| **Node9x**                | [Download](https://service.node9x.com/testnet/xrpl-evm/service-and-snapshot)                                   |
| **Endorphine Stake**      | [Download](https://services.endorphinestake.com/testnets/XRPL)                                                 |

---
{% /tab %}
{% /tabs %}

**Note:** Always verify the authenticity and integrity of snapshots before using them. Keep in mind that third-party providers may have different update schedules, data policies, and retention periods. Refer to the provider’s documentation for instructions on how to use these snapshots and any associated terms of service.
