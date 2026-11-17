# Networks

The XRPL EVM has three networks: Mainnet, Testnet and Devnet. This page lists their chain IDs, current node versions, genesis files, peers and upgrade history.

| Name    | Chain ID            | Current Version | Genesis                                                                                            | Peers                                                                              |
| ------- | ------------------- | --------------- | -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Mainnet | `xrplevm_1440000-1` | v11.3.0         | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json) | [Peers](https://raw.githubusercontent.com/xrplevm/networks/main/mainnet/peers.txt) |
| Testnet | `xrplevm_1449000-1` | v11.3.0         | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json) | [Peers](https://raw.githubusercontent.com/xrplevm/networks/main/testnet/peers.txt) |
| Devnet  | `xrplevm_1449900-1` | v11.0.0-rc.1    | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/devnet/genesis.json)  | [Peers](https://raw.githubusercontent.com/xrplevm/networks/main/devnet/peers.txt)  |

## Upgrade information

The tables below list every upgrade for each network. **Upgrade Name** is the on-chain upgrade plan name: use it as the Cosmovisor folder name (`cosmovisor/upgrades/<upgrade-name>`). **Binary Version** is the release to run from that height until the next upgrade.

{% tabs %}

{% tab label="Mainnet" %}

| Upgrade Name | Block Height | Upgrade Date | Binary Version                                                                                                                    | Docker image                                                                                                                                                                                                                                                                                                              |
| ------------ | ------------ | ------------ | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | 2025-04-25   | [v7.0.0](https://github.com/xrplevm/node/releases/tag/v7.0.0)                                                                     | [peersyst/exrp:v7.0.0](https://hub.docker.com/layers/peersyst/exrp/v7.0.0/images/sha256-dd77f81a2f8e349349fcd1266c465c77e580681764f0abbdd052bd4f4360c24e)                                                                                                                                                                 |
| `v8.0.0`     | 497000       | 2025-05-28   | [v8.0.2](https://github.com/xrplevm/node/releases/tag/v8.0.2)                                                                     | [peersyst/exrp:v8.0.2](https://hub.docker.com/layers/peersyst/exrp/v8.0.2/images/sha256-282ea8aba12ad507ff9df6b65eb83d7696b71dda0426bcbdfb357dcd2cd7bbc3)                                                                                                                                                                 |
| `v9.0.0`     | 4688681      | 2026-02-26   | [v9.0.3](https://github.com/xrplevm/node/releases/tag/v9.0.3)                                                                     | [peersyst/exrp:v9.0.3](https://hub.docker.com/layers/peersyst/exrp/v9.0.3/images/sha256-29825f07db1011974d25f2531eb4981258f639de7b5ad73f383eab64a65f9847)                                                                                                                                                                 |
| `v10.0.0`    | 4749000      | 2026-03-02   | [v10.0.2](https://github.com/xrplevm/node/releases/tag/v10.0.2)                                                                   | [peersyst/exrp:v10.0.2](https://hub.docker.com/layers/peersyst/exrp/v10.0.2/images/sha256-19ffb90a84639d8433b0896fedb106acec315e9b4da197d08204ffe67c5a42fb)                                                                                                                                                               |
| `v10.1.0`    | 6856000      | 2026-07-20   | [v10.1.0](https://github.com/xrplevm/node/releases/tag/v10.1.0) / [v10.2.1](https://github.com/xrplevm/node/releases/tag/v10.2.1) | [peersyst/exrp:v10.1.0](https://hub.docker.com/layers/peersyst/exrp/v10.1.0/images/sha256-cc7123107b7bc2815644b49a24f9951fa1edd7634c9d88c516e0ff5069874666) / [peersyst/exrp:v10.2.1](https://hub.docker.com/layers/peersyst/exrp/v10.2.1/images/sha256-9625b14ff7ca6b0f72ec1ff70b935127550778c4447e69c7060e9fd7f7130e06) |
| `v11.2.0`    | 7823200      | 2026-09-23   | [v11.2.0](https://github.com/xrplevm/node/releases/tag/v11.2.0)                                                                   | [peersyst/exrp:v11.2.0](https://hub.docker.com/layers/peersyst/exrp/v11.2.0/images/sha256-115e6e20a3bc7c659b93996ff44e55cf670fff5e87444cb98c0063e041f43e7a)                                                                                                                                                               |
| `v11.3.0`    | 8367050      | 2026-10-29   | [v11.3.0](https://github.com/xrplevm/node/releases/tag/v11.3.0)                                                                   | [peersyst/exrp:v11.3.0](https://hub.docker.com/layers/peersyst/exrp/v11.3.0/images/sha256-661bbc66851840bb13186ea700242cafddad6dd1a85ca244c31c43ca4ff78ba5)                                                                                                                                                               |

From block `7360029` (2026-08-23) until the `v11.2.0` upgrade, Mainnet ran the `v10.2.x` security releases in place of `v10.1.0`. They share the `v10.1.0` upgrade handler, and there was no on-chain upgrade. `v10.2.1` was the recommended binary for nodes running in that range. See [Sync from Genesis](../getting-started/sync-from-genesis.md) before replaying it.

{% /tab %}

{% tab label="Testnet" %}

| Upgrade Name | Block Height | Upgrade Date | Binary Version                                                  | Docker image                                                                                                                                                |
| ------------ | ------------ | ------------ | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | 2025-02-17   | [v6.0.0](https://github.com/xrplevm/node/releases/tag/v6.0.0)   | [peersyst/exrp:v6.0.0](https://hub.docker.com/layers/peersyst/exrp/v6.0.0/images/sha256-9d8c9f96e27c648216fddbc4bb67c10529aa5ea03d303cf56060e453c02a4ca9)   |
| `v7.0.0`     | 547100       | 2025-03-25   | [v7.0.0](https://github.com/xrplevm/node/releases/tag/v7.0.0)   | [peersyst/exrp:v7.0.0](https://hub.docker.com/layers/peersyst/exrp/v7.0.0/images/sha256-dd77f81a2f8e349349fcd1266c465c77e580681764f0abbdd052bd4f4360c24e)   |
| `v8.0.0`     | 1485600      | 2025-05-26   | [v8.0.0](https://github.com/xrplevm/node/releases/tag/v8.0.0)   | [peersyst/exrp:v8.0.0](https://hub.docker.com/layers/peersyst/exrp/v8.0.0/images/sha256-2fba5b2bef8a203b3226ff88d5c5018a67370d2e4a6838d25809424223f7f3a8)   |
| `v9.0.0`     | 3827000      | 2025-10-29   | [v9.0.3](https://github.com/xrplevm/node/releases/tag/v9.0.3)   | [peersyst/exrp:v9.0.3](https://hub.docker.com/layers/peersyst/exrp/v9.0.3/images/sha256-29825f07db1011974d25f2531eb4981258f639de7b5ad73f383eab64a65f9847)   |
| `v10.0.0`    | 5601606      | 2026-02-23   | [v10.0.1](https://github.com/xrplevm/node/releases/tag/v10.0.1) | [peersyst/exrp:v10.0.1](https://hub.docker.com/layers/peersyst/exrp/v10.0.1/images/sha256-3fb70439ee2fe3181bd4d0de6f897dd888fb0249f909b5043769ab2c37338f4e) |
| `v10.1.0`    | 7725000      | 2026-07-15   | [v10.1.0](https://github.com/xrplevm/node/releases/tag/v10.1.0) | [peersyst/exrp:v10.1.0](https://hub.docker.com/layers/peersyst/exrp/v10.1.0/images/sha256-cc7123107b7bc2815644b49a24f9951fa1edd7634c9d88c516e0ff5069874666) |
| `v11.1.0`    | 8158200      | 2026-08-14   | [v11.1.1](https://github.com/xrplevm/node/releases/tag/v11.1.1) | [peersyst/exrp:v11.1.1](https://hub.docker.com/layers/peersyst/exrp/v11.1.1/images/sha256-88466ed0cc222b35eb882a62c181923b87735970cab17059c71c166e8436934d) |
| `v11.3.0`    | 9280100      | 2026-10-28   | [v11.3.0](https://github.com/xrplevm/node/releases/tag/v11.3.0) | [peersyst/exrp:v11.3.0](https://hub.docker.com/layers/peersyst/exrp/v11.3.0/images/sha256-661bbc66851840bb13186ea700242cafddad6dd1a85ca244c31c43ca4ff78ba5) |

`v11.1.1` is a patch release of `v11.1.0` with the same `v11.1.0` upgrade handler. Testnet skipped `v11.2.0` and moved from `v11.1.1` to `v11.3.0`. The `v11.3.0` binary does not contain the `v11.1.0` handler, so keep `v11.1.1` for the range from block `8158200` to `9280100`.

{% /tab %}

{% tab label="Devnet" %}

| Upgrade Name | Block Height | Upgrade Date | Binary Version                                                            | Docker image                                                                                                                                                          |
| ------------ | ------------ | ------------ | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | 2026-01-27   | [v9.0.3](https://github.com/xrplevm/node/releases/tag/v9.0.3)             | [peersyst/exrp:v9.0.3](https://hub.docker.com/layers/peersyst/exrp/v9.0.3/images/sha256-29825f07db1011974d25f2531eb4981258f639de7b5ad73f383eab64a65f9847)             |
| `v10.0.0`    | 1198644      | 2026-05-05   | [v10.0.3](https://github.com/xrplevm/node/releases/tag/v10.0.3)           | [peersyst/exrp:v10.0.3](https://hub.docker.com/layers/peersyst/exrp/v10.0.3/images/sha256-c316fca5d6023dab60487b2a0c2ba45067df78f64452e360d512ecdf32b215f5)           |
| `v11.0.0`    | 2089191      | 2026-06-29   | [v11.0.0-rc.1](https://github.com/xrplevm/node/releases/tag/v11.0.0-rc.1) | [peersyst/exrp:v11.0.0-rc.1](https://hub.docker.com/layers/peersyst/exrp/v11.0.0-rc.1/images/sha256-69de5ab7763b94e6a3e1ea99a54273870c39d2343a4ae7585fc1be1c0f49d8d0) |

{% /tab %}
{% /tabs %}

{% admonition type="warning" name="Docker tags" %}
Always pin an explicit release tag. The `latest` tag follows the newest release, which may not support your network (a Mainnet release can lack the upgrade handler a Testnet height needs), and `main` is built from the development branch.
{% /admonition %}
