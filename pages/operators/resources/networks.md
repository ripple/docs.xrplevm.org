# Networks

The XRPL EVM ecosystem consists of multiple networks, each serving different stages of development and production. Understanding the distinctions between these networks helps you select the right environment for testing, deployment, and scaling your applications. Below are details on the mainnet, and testnet, including chain IDs, intended purposes, and other key characteristics.

| Name    | Chain ID         | Current Version | Genesis                                                                                            | Peers                                                                                         |
| ------- | ---------------- | --------------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Mainnet | `xrplevm_1440000-1` | v10.1.0          | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/mainnet/genesis.json) | [Peers](https://raw.githubusercontent.com/xrplevm/networks/main/mainnet/peers.txt)            |
| Testnet | `xrplevm_1449000-1` | v10.1.0          | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json) | [Peers](https://raw.githubusercontent.com/xrplevm/networks/main/testnet/peers.txt)            |
| Devnet | `xrplevm_1449900-1` | v9.0.3          | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/devnet/genesis.json) | [Peers](https://raw.githubusercontent.com/xrplevm/networks/main/devnet/peers.txt)            |


## Upgrade information

Below is a table indicating all the hard fork upgrades for each network:

{% tabs %}

{% tab label="Mainnet" %}

| Upgrade Name | Block Height | Upgrade Date | Binary Version                                                | Docker image                                                                                                                                              |
| ------------ | ------------ | ------------ | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | 2025-04-25   | [v7.0.0](https://github.com/xrplevm/node/releases/tag/v7.0.0) | [peersyst/exrp:v7.0.0](https://hub.docker.com/layers/peersyst/exrp/v7.0.0/images/sha256:dd77f81a2f8e349349fcd1266c465c77e580681764f0abbdd052bd4f4360c24e) |
| v8           | 497000       | 2025-05-28   | [v8.0.2](https://github.com/xrplevm/node/releases/tag/v8.0.2) | [peersyst/exrp:v8.0.2](https://hub.docker.com/layers/peersyst/exrp/v8.0.2/images/sha256:282ea8aba12ad507ff9df6b65eb83d7696b71dda0426bcbdfb357dcd2cd7bbc3) |
| v9           | 4688681       | 2026-02-26   | [v9.0.3](https://github.com/xrplevm/node/releases/tag/v9.0.3) | [peersyst/exrp:v9.0.3](https://hub.docker.com/layers/peersyst/exrp/v9.0.3/images/sha256:29825f07db1011974d25f2531eb4981258f639de7b5ad73f383eab64a65f9847) |
| v10           | 4749000       | 2026-03-02   | [v10.0.2](https://github.com/xrplevm/node/releases/tag/v10.0.2) | [peersyst/exrp:v10.0.2](https://hub.docker.com/layers/peersyst/exrp/v10.0.2/images/sha256:19ffb90a84639d8433b0896fedb106acec315e9b4da197d08204ffe67c5a42fb) |


{% /tab %}

{% tab label="Testnet" %}

| Upgrade Name | Block Height | Upgrade Date | Binary Version                                                | Docker image                                                                                                                                              |
| ------------ | ------------ | ------------ | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | 2025-02-17   | [v6.0.0](https://github.com/xrplevm/node/releases/tag/v6.0.0) | [peersyst/exrp:v6.0.0](https://hub.docker.com/layers/peersyst/exrp/v6.0.0/images/sha256-9d8c9f96e27c648216fddbc4bb67c10529aa5ea03d303cf56060e453c02a4ca9) |
| v7           | 547100       | 2025-03-25   | [v7.0.0](https://github.com/xrplevm/node/releases/tag/v7.0.0) | [peersyst/exrp:v7.0.0](https://hub.docker.com/layers/peersyst/exrp/v7.0.0/images/sha256:dd77f81a2f8e349349fcd1266c465c77e580681764f0abbdd052bd4f4360c24e) |
| v8           | 1485600       | 2025-05-26   | [v8.0.0](https://github.com/xrplevm/node/releases/tag/v8.0.0) | [peersyst/exrp:v8.0.0](https://hub.docker.com/layers/peersyst/exrp/v8.0.0/images/sha256-2fba5b2bef8a203b3226ff88d5c5018a67370d2e4a6838d25809424223f7f3a8) |
| v9           | 3827000       | 2025-10-29   | [v9.0.3](https://github.com/xrplevm/node/releases/tag/v9.0.3) | [peersyst/exrp:v9.0.3](https://hub.docker.com/layers/peersyst/exrp/v9.0.3/images/sha256:29825f07db1011974d25f2531eb4981258f639de7b5ad73f383eab64a65f9847) |
| v10           | 5601606       | 2026-02-23   | [v10.0.1](https://github.com/xrplevm/node/releases/tag/v10.0.1) | [peersyst/exrp:v10.0.1](https://hub.docker.com/layers/peersyst/exrp/v10.0.1/images/sha256:4c3bfb17f3ba39680b3770f5e5ae3247643bdae6ed8f90da5c6293f11933cc01) |

{% /tab %}

{% tab label="Devnet" %}

| Upgrade Name | Block Height | Upgrade Date | Binary Version                                                | Docker image                                                                                                                                              |
| ------------ | ------------ | ------------ | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | 2026-01-27   | [v9.0.3](https://github.com/xrplevm/node/releases/tag/v9.0.3) | [peersyst/exrp:v9.0.3](https://hub.docker.com/layers/peersyst/exrp/v9.0.3/images/sha256:29825f07db1011974d25f2531eb4981258f639de7b5ad73f383eab64a65f9847) |

{% /tab %}
{% /tabs %}
