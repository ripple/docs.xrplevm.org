# Networks

The XRPL EVM ecosystem consists of multiple networks, each serving different stages of development and production. Understanding the distinctions between these networks helps you select the right environment for testing, deployment, and scaling your applications. Below are details on the mainnet, testnet, and devnet, including chain IDs, intended purposes, and other key characteristics.

| Name   | Chain ID         | Current Version | Genesis     | Peers     |
| ------ | ---------------- | --------------- | ----------- | --------- |
| Devnet | `exrp_1440002-1` | v6.0.0          | [Genesis]() | [Peers]() |

## Upgrade information

Below is a table indicating all the hard fork upgrades for each network:

### Devnet

| Upgrade Name | Block Height | Upgrade Date       | Binary Version                                                              | Docker image                                                                                                                                                                                          |
| ------------ | ------------ | ------------------ | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Genesis      | 0            | Initial Deployment | [v1.0.0-beta.0](https://github.com/xrplevm/node/releases/tag/v1.0.0-beta.0) | [peersyst/xrp-evm-blockchain:latest](https://hub.docker.com/layers/peersyst/xrp-evm-blockchain/latest/images/sha256-de9941203bb9f199e6125e3518d9c56a8106c93211cd2840cb9b0fc7652f5416?context=explore) |
| v2           | 8912879      | 2024-06-05         | [v2.0.0](https://github.com/xrplevm/node/releases/tag/v2.0.0)               | [peersyst/exrp:v2.0.0](https://hub.docker.com/layers/peersyst/exrp/v2.0.0/images/sha256-0e7c502211696f6dae0dc2ce8ae16429bd4ee09941b00cf95e63dfad86d10407?context=explore)                             |
| v3           | 10959649     | 2024-09-04         | [v3.0.0](https://github.com/xrplevm/node/releases/tag/v3.0.0)               | [peersyst/exrp:v3.0.0](htthttps://hub.docker.com/layers/peersyst/exrp/v3.0.0/images/sha256-4c36e6a5e833fb73d692a9c1e7c146b3b7421db576c0c07c64013c089ebeea9d)                                          |
| v4           | 12803476     | 2024-11-26         | [v4.0.0](https://github.com/xrplevm/node/releases/tag/v4.0.0)               | [peersyst/exrp:v4.0.0](https://hub.docker.com/layers/peersyst/exrp/v4.0.0/images/sha256-117776dbf6dc8cf2ab77b5dfc699ad0a9180e8eb96b1123e5f8810953e1db5ad)                                             |
| v5           | 13242557     | 2024-12-18         | [v5.0.0](https://github.com/xrplevm/node/releases/tag/v5.0.0)               | [peersyst/exrp:v5.0.0](https://hub.docker.com/layers/peersyst/exrp/v5.0.0/images/sha256-3e55164718fe2d81cba50cb426bfd6fecc103201db3f02df18290e7525a4cb71)                                             |
| v6           | 14052581     | 2025-01-22         | [v6.0.0](https://github.com/xrplevm/node/releases/tag/v6.0.0)               | [peersyst/exrp:v6.0.0](https://hub.docker.com/layers/peersyst/exrp/v6.0.0/images/sha256-9d8c9f96e27c648216fddbc4bb67c10529aa5ea03d303cf56060e453c02a4ca9)                                             |
