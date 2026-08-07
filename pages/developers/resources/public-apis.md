# Public APIs

Public APIs offer ready-to-use endpoints for accessing and interacting with the XRPL EVM network without needing to run your own node. They enable developers, wallets, and services to read blockchain data, send transactions, and query network status in real time.

{% tabs %}

{% tab label="Mainnet" %}

## Mainnet Official Public APIs

| Type              | URL                                                                |
| ----------------- | ------------------------------------------------------------------ |
| Ethereum JSON RPC | [https://rpc.xrplevm.org](https://rpc.xrplevm.org)                 |
| Ethereum JSON WS  | [https://ws.xrplevm.org](https://ws.xrplevm.org)                   |
| Tendermint RPC    | [https://cosmos-rpc.xrplevm.org](https://cosmos-rpc.xrplevm.org)   |
| Cosmos gRPC       | `cosmos-grpc.xrplevm.org:443` |
| Cosmos API        | [https://cosmos-api.xrplevm.org](https://cosmos-api.xrplevm.org)   |

## Additional Endpoints

{% tabs %}

        {% tab label="Ethereum JSON RPC" %}
        | Provider/Validator  | RPC Endpoint URL                                                                              |
        | ------------------- | --------------------------------------------------------------------------------------------- |
        | **Polkachu**        | [https://xrpevm-rpc.polkachu.com/](https://xrpevm-rpc.polkachu.com/)                          |
        | **Cumulo**          | [https://json-rpc.xrpl.cumulo.org.es](https://json-rpc.xrpl.cumulo.org.es)                    |
        | **ITRocket**        | [https://xrplevm-mainnet-evm.itrocket.net](https://xrplevm-mainnet-evm.itrocket.net)          |

        {% /tab %}
        {% tab label="Ethereum JSON WS" %}
        | Provider/Validator  | WSS Endpoint URL                                                                              |
        | ------------------- | --------------------------------------------------------------------------------------------- |
        | **Cumulo**          | [https://ws.xrpl.cumulo.org.es](https://ws.xrpl.cumulo.org.es)                                |
        | **ITRocket**        | [wss://xrplevm-mainnet-rpc.itrocket.net](wss://xrplevm-mainnet-rpc.itrocket.net)              |

        {% /tab %}
        {% tab label="Tendermint RPC" %}
        | Provider/Validator  | EVM RPC Endpoint URL                                                                          |
        | ------------------- | --------------------------------------------------------------------------------------------- |
        | **Polkachu**        | [https://xrp-rpc.polkachu.com/](https://xrp-rpc.polkachu.com/)                                |
        | **Cumulo**          | [https://rpc.xrpl.cumulo.org.es/](https://rpc.xrpl.cumulo.org.es/)                            |
        | **ITRocket**        | [https://xrplevm-mainnet-rpc.itrocket.net](https://xrplevm-mainnet-rpc.itrocket.net)          |

        {% /tab %}
        {% tab label="Cosmos gRPC" %}
        | Provider/Validator  | gRPC Endpoint URL                                                                             |
        | ------------------- | --------------------------------------------------------------------------------------------- |
        | **Polkachu**        | xrp-grpc.polkachu.com:30090                                                                   |
        | **Cumulo**          | [http://grpc.xrpl.cumulo.org.es](http://grpc.xrpl.cumulo.org.es)                              |
        | **ITRocket**        | xrplevm-mainnet-grpc.itrocket.net:443                                                         |

        {% /tab %}
        {% tab label="Cosmos API" %}
        | Provider/Validator  | API Endpoint URL                                                                              |
        | ------------------- | --------------------------------------------------------------------------------------------- |
        | **Polkachu**        | [https://xrp-api.polkachu.com/](https://xrp-api.polkachu.com/)                                |
        | **Cumulo**          | [https://api.xrpl.cumulo.org.es/](https://api.xrpl.cumulo.org.es/)                            |
        | **ITRocket**        | [https://xrplevm-mainnet-api.itrocket.net](https://xrplevm-mainnet-api.itrocket.net)          |

        {% /tab %}
    {% /tabs %}

{% /tab %}

{% tab label="Testnet" %}

## Testnet Official Public APIs

| Type              | URL                                                                              |
| ----------------- | -------------------------------------------------------------------------------- |
| Ethereum JSON RPC | [https://rpc.testnet.xrplevm.org](https://rpc.testnet.xrplevm.org)               |
| Ethereum JSON WS  | [https://ws.testnet.xrplevm.org](https://ws.testnet.xrplevm.org)                 |
| Tendermint RPC    | [https://cosmos-rpc.testnet.xrplevm.org](https://cosmos-rpc.testnet.xrplevm.org) |
| Cosmos gRPC       | cosmos-grpc.testnet.xrplevm.org:443                                              |
| Cosmos API        | [https://cosmos-api.testnet.xrplevm.org](https://cosmos-api.testnet.xrplevm.org) |

## Additional Endpoints

    {% tabs %}

        {% tab label="Ethereum JSON RPC" %}
        | Provider/Validator  | RPC Endpoint URL                                                                         |
        | ------------------- | ---------------------------------------------------------------------------------------- |
        | **Cumulo**          | [https://json-rpc.xrpl.cumulo.com.es](https://json-rpc.xrpl.cumulo.com.es)               |
        | **ITRocket**        | [https://xrplevm-testnet-evm.itrocket.net](https://xrplevm-testnet-evm.itrocket.net)     |
        {% /tab %}

        {% tab label="Ethereum JSON WS" %}
        | Provider/Validator  | WSS Endpoint URL                                                                         |
        | ------------------- | ---------------------------------------------------------------------------------------- |
        | **Cumulo**          | [https://ws.xrpl.cumulo.org.es](https://ws.xrpl.cumulo.org.es)                           |
        {% /tab %}

        {% tab label="Tendermint RPC" %}
        | Provider/Validator  | EVM RPC Endpoint URL                                                                     |
        | ------------------- | ---------------------------------------------------------------------------------------- |
        | **Polkachu**        | [https://xrp-testnet-rpc.polkachu.com/](https://xrp-testnet-rpc.polkachu.com/)           |
        | **Cumulo**          | [https://rpc.xrpl.cumulo.com.es](https://rpc.xrpl.cumulo.com.es)                         |
        | **ITRocket**        | [https://xrplevm-testnet-rpc.itrocket.net](https://xrplevm-testnet-rpc.itrocket.net)     |
        {% /tab %}

        {% tab label="Cosmos gRPC" %}
        | Provider/Validator  | gRPC Endpoint URL                                                                        |
        | ------------------- | ---------------------------------------------------------------------------------------- |
        | **Polkachu**        | xrp-testnet-grpc.polkachu.com:30090                                                      |
        | **Cumulo**          | grpc.xrpl.cumulo.com.es                                                                  |
        | **ITRocket**        | xrplevm-testnet-grpc.itrocket.net:443                                                    |
        {% /tab %}

        {% tab label="Cosmos API" %}
        | Provider/Validator  | API Endpoint URL                                                                         |
        | ------------------- | ---------------------------------------------------------------------------------------- |
        | **Polkachu**        | [https://xrp-testnet-api.polkachu.com/](https://xrp-testnet-api.polkachu.com/)           |
        | **Cumulo**          | [https://api.xrpl.cumulo.com.es](https://api.xrpl.cumulo.com.es)                         |
        | **ITRocket**        | [https://xrplevm-testnet-api.itrocket.net](https://xrplevm-testnet-api.itrocket.net)     |
        {% /tab %}
    {% /tabs %}

{% /tab %}
{% /tabs %}
