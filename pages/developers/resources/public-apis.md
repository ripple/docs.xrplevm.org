# Public APIs

Public APIs offer ready-to-use endpoints for accessing and interacting with the XRPL EVM network without needing to run your own node. They enable developers, wallets, and services to read blockchain data, send transactions, and query network status in real time.

{% tabs %}

{% tab label="Mainnet" %}

## Mainnet Official Public APIs

| Type              | URL                                                                       |
| ----------------- | ------------------------------------------------------------------------- |
| Ethereum JSON RPC | [https://rpc.xrplevm.org](https://rpc.xrplevm.org)                        |
| Ethereum JSON WS  | [https://ws.xrplevm.org](https://ws.xrplevm.org)                          |
| Tendermint RPC    | [https://cosmos-rpc.xrplevm.org](https://cosmos-rpc.xrplevm.org)          |
| Cosmos gRPC       | [https://cosmos-grpc.xrplevm.org](https://cosmos-grpc.xrplevm.org)        |
| Cosmos API        | [https://cosmos-api.xrplevm.org](https://cosmos-api.xrplevm.org)          |                                                                    |

## Additional Endpoints
{% tabs %}

        {% tab label="Ethereum JSON RPC" %}
        | Provider/Validator              | RPC Endpoint URL                                                                              |
        | ------------------------------- | --------------------------------------------------------------------------------------------- |
        | **Peersyst**                    | [https://rpc.xrplevm.org](https://rpc.xrplevm.org)                                            |
        | **Cumulo**                      | [https://json-rpc.xrpl.cumulo.org.es](https://json-rpc.xrpl.cumulo.org.es)                    |
                   |
        | **polkachu**                      | [https://xrpevm-rpc.polkachu.com/](https://xrpevm-rpc.polkachu.com/)                    |
                   |
        


        {% /tab %}
        {% tab label="Ethereum JSON WS" %}
        | Provider/Validator  | WSS Endpoint URL                                                                                 |
        | ------------------- | ------------------------------------------------------------------------------------------------ |
        | **Peersyst**        | [https://ws.xrplevm.org]( https://ws.xrplevm.org)                                                |
        | **Cumulo**          | [https://ws.xrpl.cumulo.org.es](https://ws.xrpl.cumulo.org.es)                                   |


        {% /tab %}
        {% tab label="Tendermint RPC" %}
        | Provider/Validator   | EVM RPC Endpoint URL                                                                               |
        | ------------------------- | ----------------------------------------------------------------------------------------------|
        | **Peersyst**              | [https://cosmos-rpc.xrplevm.org](https://cosmos-rpc.xrplevm.org)                              |
        | **Polkachu**              | [https://xrp-rpc.polkachu.com/](https://xrp-rpc.polkachu.com/)                                |
        | **Cumulo**                | [https://rpc.xrpl.cumulo.org.es/](https://rpc.xrpl.cumulo.org.es/)                            |
        | **Stakeme**               | [https://xrpl-rpc.stakeme.pro/](https://xrpl-rpc.stakeme.pro/)                                |

        {% /tab %}
        {% tab label="Cosmos gRPC" %}
        | Provider/Validator        | gRPC Endpoint URL                                                                             |
        | ------------------------- | ----------------------------------------------------------------------------------------------|
        | **Peersyst**              | [https://cosmos-grpc.xrplevm.org](https://cosmos-grpc.xrplevm.org)                            |
        | **Cumulo**                | [http://grpc.xrpl.cumulo.org.es](http://grpc.xrpl.cumulo.org.es)                              |


        {% /tab %}
        {% tab label="Cosmos API" %}
        | Provider/Validator        | API Endpoint URL                                                                 |
        | ------------------------- | -------------------------------------------------------------------------------- |
        | **Peersyst**              | [https://cosmos-api.xrplevm.org](https://cosmos-api.xrplevm.org)                 |
        | **Polkachu**              | [https://xrp-api.polkachu.com/](https://xrp-api.polkachu.com/)                   |
        | **Cumulo**                | [https://api.xrpl.cumulo.org.es/](https://api.xrpl.cumulo.org.es/)               |
        | **Stakeme**               | [https://xrpl-rest.stakeme.pro/](https://xrpl-rest.stakeme.pro/)                 |

        {% /tab %}
    {% /tabs %}

{% /tab %}

{% tab label="Testnet" %}
                                             |

## Additional Endpoints

    {% tabs %}

        {% tab label="Ethereum JSON RPC" %}
        | Provider/Validator              | RPC Endpoint URL                                                                              |
        | ------------------------------- | --------------------------------------------------------------------------------------------- |
        | **Peersyst**                    | [https://rpc.testnet.xrplevm.org](https://rpc.testnet.xrplevm.org)                            |
        | **Cumulo**                      | [https://json-rpc.xrpl.cumulo.com.es](https://json-rpc.xrpl.cumulo.com.es)                    |
        | **ITRocket**                    | [https://xrplevm-testnet-evm.itrocket.net](https://xrplevm-testnet-evm.itrocket.net)          |
        


        {% /tab %}

        {% tab label="Ethereum JSON WS" %}
        | Provider/Validator  | WSS Endpoint URL                                                                                 |
        | ------------------- | ------------------------------------------------------------------------------------------------ |
        | **Peersyst**        | [https://ws.testnet.xrplevm.org]( https://ws.testnet.xrplevm.org)                               |
        {% /tab %}

        {% tab label="Tendermint RPC" %}
        | Provider/Validator   | EVM RPC Endpoint URL                                                                                       |
        | ------------------------- | -----------------------------------------------------------------------------------------------------------|
        | **Peersyst**              | [https://cosmos-rpc.testnet.xrplevm.org](https://cosmos-rpc.testnet.xrplevm.org)                         |
        | **Polkachu**              | [https://xrp-testnet-rpc.polkachu.com/](https://xrp-testnet-rpc.polkachu.com/)                |
        | **Cumulo**                | [https://rpc.xrpl.cumulo.com.es](https://rpc.xrpl.cumulo.com.es)                              |
        | **ITRocket**              | [https://xrplevm-testnet-rpc.itrocket.net](https://xrplevm-testnet-rpc.itrocket.net)          |

        {% /tab %}

        {% tab label="Cosmos gRPC" %}
        | Provider/Validator        | gRPC Endpoint URL                                                                           |
        | ------------------------- | ----------------------------------------------- |
        | **Peersyst**              | `cosmos-grpc.testnet.xrplevm.org:443`           |
        | **Cumulo**                | `grpc.xrpl.cumulo.com.es`                       |

        {% /tab %}

        {% tab label="Cosmos API" %}
        | Provider/Validator        | API Endpoint URL                                                                 |
        | ------------------------- | ------------------------------------------------------------------------------- |
        | **Peersyst**              | [http://cosmos-api.testnet.xrplevm.org](http://cosmos-api.testnet.xrplevm.org)   |
        | **Polkachu**              | [https://xrp-testnet-api.polkachu.com/](https://xrp-testnet-api.polkachu.com/)   |
        | **Cumulo**                | [https://api.xrpl.cumulo.com.es](https://api.xrpl.cumulo.com.es)                |
        | **ITRocket**              | [https://xrplevm-testnet-api.itrocket.net](https://xrplevm-testnet-api.itrocket.net) |
        | **BonyNode**               | [https://xrpl-testnet-api.bonynode.online/](https://xrpl-testnet-api.bonynode.online/)   |

        {% /tab %}

        {% tab label="Snapshot / Genesis" %}
        | Provider/Validator        | Snapshot / Genesis URL                                                                                         |
        | ------------------------- | ---------------------------------------------------------------------------------------------------------------- |
        | **Peersyst**              | [Snapshot](https://evm-sidechain-snapshots-testnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4)                      |
        | **Polkachu**              | [https://polkachu.com/testnets/xrp/snapshots](https://polkachu.com/testnets/xrp/snapshots)                       |
        | **blockitize**            | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json) |
        | **Cumulo**                | [Snapshot](https://cumulo.pro/services/xrplevm/snapshot)                                    |
        | **Cumulo**                | [State-Sync](https://cumulo.pro/services/xrplevm/state-sync)                                    |
        | **ITRocket**              | [https://itrocket.net/services/testnet/xrplevm/](https://itrocket.net/services/testnet/xrplevm/)                |

        {% /tab %}

        {% tab label="Documentation / Guide" %}
        | Provider/Validator        | Docs / Guide URL                                                                 |
        | ------------------------- | ------------------------------------------------------------------------------- |
        | **Polkachu**              | [Installation Guide](https://polkachu.com/testnets/xrp/installation)              |
        | **Cumulo**                | [Node Guide](https://cumulo.pro/services/xrplevm/)                              |
        | **ITRocket**              | [ITRocket XRPL Docs](https://itrocket.net/services/testnet/xrplevm/installation/#installation)            |
        | **Imperator.co**          | [https://www.imperator.co/services/chain-services/testnets/xrp](https://www.imperator.co/services/chain-services/testnets/xrp) |
        
        {% /tab %}

        {% tab label="Other / Monitoring & Tools" %}
        | Provider/Validator        | Additional Info / Tools                                                                                                                           |
        | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
        | **Cumulo**                | [RPC & API Scan](https://cumulo.pro/services/xrplevm/)                                                                        |
        | **ITRocket**              | [Validator Analytics](https://itrocket.net/services/testnet/xrplevm/analytics/validators-performance), [Consensus Analytics](https://itrocket.net/services/testnet/xrplevm/analytics/consensus/), [Peer scanner](https://itrocket.net/services/testnet/xrplevm/public-rpc/), [Decentralization Analytics](https://itrocket.net/services/testnet/xrplevm/decentralization/) |

        {% /tab %}
    {% /tabs %}

    ---
{% /tab %}
{% /tabs %}
