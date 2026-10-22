---
seo:
  description: "Wormhole contract addresses on the XRPL EVM Sidechain: Core Contract, Wrapped Token Transfers, Executor, Executor Token Bridge Relayer and Guardian Governance, per network."
---

# Wormhole Deployed Contracts

The following Wormhole contracts are deployed on the **XRPL EVM Sidechain**. Always cross-check addresses against the official [Wormhole contract address reference](https://wormhole.com/docs/reference/contract-addresses/) before sending funds or wiring them into a contract.

{% tabs %}
{% tab label="Mainnet" %}

| Contract                      | Address                                      |
| ----------------------------- | -------------------------------------------- |
| Core Contract                 | `0xaBf89de706B583424328B54dD05a8fC986750Da8` |
| Wrapped Token Transfers (WTT) | `0x47F5195163270345fb4d7B9319Eda8C64C75E278` |
| Executor                      | `0x8345E90Dcd92f5Cf2FAb0C8E2A56A5bc2c30d896` |
| Executor Token Bridge Relayer | `0x37bCc9d175124F77Bfce68589d2a8090eF846B85` |
| Executor Token Bridge Relayer (with referrer) | `0xee05C2e6075E2C86D1F5db4716Ff2A6c18889B20` |
| Guardian Governance           | `0x574B7864119C9223A9870Ea614dC91A8EE09E512` |

The **Executor Token Bridge Relayer** delivers WTT transfers automatically through the Executor framework. It is the route that Wormhole Connect and Portal Bridge use for automatic WTT delivery.

{% admonition type="warning" name="No legacy Wormhole Relayer on Mainnet" %}
The legacy `IWormholeRelayer` ("Standard Relayer") is **not part of the official Wormhole deployment on XRPL EVM Mainnet**: it is absent from the Wormhole SDK and contract registry, and no delivery provider prices routes from XRPL EVM. Integrations that follow older Wormhole tutorials built on `sendPayloadToEvm` will not be relayed on Mainnet. Use the Core Contract together with the **Executor** framework instead. See [Send Cross-Chain Messages](../../developers/interacting-with-evm/advanced-guides/cross-chain-transactions/wormhole/send-messages.md).
{% /admonition %}

{% /tab %}
{% tab label="Testnet" %}

| Contract                      | Address                                      |
| ----------------------------- | -------------------------------------------- |
| Core Contract                 | `0xaBf89de706B583424328B54dD05a8fC986750Da8` |
| Wrapped Token Transfers (WTT) | `0x7d8eBc211C4221eA18E511E4f0fD50c5A539f275` |
| Executor                      | `0x4d9525D94D275dEB495b7C8840b154Ae04cfaC2A` |
| Executor Token Bridge Relayer | `0xb00224C60fE6ab134C8544dc29350286545F8DCc` |
| Executor Token Bridge Relayer (with referrer) | `0x06Bc50Cf4768929465E07199567B36Da6C74808c` |
| Guardian Governance           | `0x574B7864119C9223A9870Ea614dC91A8EE09E512` |
| Wormhole Relayer (legacy)     | `0x362fca37E45fe1096b42021b543f462D49a5C8df` |

{% admonition type="info" name="Testnet-only legacy relayer" %}
The legacy `IWormholeRelayer` above exists **only on Testnet**. It is useful for following the standard Wormhole tutorials during development, but any code that depends on it cannot be promoted to Mainnet as-is.
{% /admonition %}

{% /tab %}
{% /tabs %}

## Chain Identifiers

| Network                | Wormhole chain ID | SDK chain name | EVM chain ID |
| ---------------------- | ----------------- | -------------- | ------------ |
| XRPL EVM Mainnet       | `57`              | `XRPLEVM`      | `1440000`    |
| XRPL EVM Testnet       | `57`              | `XRPLEVM`      | `1449000`    |
| XRPL Mainnet / Testnet | `66`              | `Xrpl`         | N/A          |

## Related References

- [Wormhole contract addresses (all chains)](https://wormhole.com/docs/reference/contract-addresses/)
- [Wormhole chain IDs and supported networks](https://wormhole.com/docs/reference/supported-networks/)
- [WormholeScan explorer](https://wormholescan.io)
