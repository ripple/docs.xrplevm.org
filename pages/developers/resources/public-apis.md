# Public APIs

Public APIs offer ready-to-use endpoints for accessing and interacting with the XRPL EVM network without needing to run your own node. They enable developers, wallets, and services to read blockchain data, send transactions, and query network status in real time.

{% tabs %}

{% tab label="Testnet" %}

    

    {% tabs %}

        {% tab label="RPC Endpoints" %}
        | Provider/Validator        | RPC Endpoint URL                                                                              |
        | ------------------------- | --------------------------------------------------------------------------------------------- |
        | **Peersyst**              | [https://rpc.testnet.xrplevm.org](https://rpc.testnet.xrplevm.org)                           |
        | **Grove**                 | [https://xrpl-evm-testnet.rpc.grove.city/v1/0caa84c4](https://xrpl-evm-testnet.rpc.grove.city/v1/0caa84c4) |
        | **Kintsugi Nodes**        | [https://xrpevm.kintsugi-nodes.com](https://xrpevm.kintsugi-nodes.com)                       |
        | **Polkachu**              | [https://xrp-testnet-rpc.polkachu.com/](https://xrp-testnet-rpc.polkachu.com/)               |
        | **Cumulo**                | [https://rpc.xrpl.cumulo.com.es](https://rpc.xrpl.cumulo.com.es)                             |
        | **Ember**                 | [https://xrpl-rpc.embervalidator.top/](https://xrpl-rpc.embervalidator.top/)                 |
        | **Enigma**                | [https://xrp-rpc.enigma-validator.com/](https://xrp-rpc.enigma-validator.com/)               |
        | **ITRocket**              | [https://xrplevm-testnet-rpc.itrocket.net](https://xrplevm-testnet-rpc.itrocket.net)         |
        | **LuckyStar**             | [http://xrpl-testnet-rpc.luckystar.asia/](http://xrpl-testnet-rpc.luckystar.asia/)           |
        | **Mekong Labs**           | [https://exrpd-testnet-rpc.mekonglabs.tech/](https://exrpd-testnet-rpc.mekonglabs.tech/)     |
        | **MictoNode**             | [https://xrpl-testnet-rpc.mictonode.com](https://xrpl-testnet-rpc.mictonode.com)             |
        | **NodeStake**             | [https://rpc-t.xrp.nodestake.org](https://rpc-t.xrp.nodestake.org)                           |
        | **Cosmonaut Stakes**      | [https://xrpl-testnet-rpc.cosmonautstakes.com](https://xrpl-testnet-rpc.cosmonautstakes.com)                           |

        {% /tab %}

        {% tab label="WSS / WebSocket RPC" %}
        | Provider/Validator  | WSS Endpoint URL                                                                             |
        | ------------------- | -------------------------------------------------------------------------------------------- |
        | **Peersyst**      | [ https://ws.testnet.xrplevm.org]( https://ws.testnet.xrplevm.org)      |
        | **MictoNode**       | [wss://xrpl-testnet-rpc.mictonode.com/websocket](wss://xrpl-testnet-rpc.mictonode.com/websocket) |
        {% /tab %}

        {% tab label="API Endpoints" %}
        | Provider/Validator        | API Endpoint URL                                                                 |
        | ------------------------- | ------------------------------------------------------------------------------- |
        | **Peersyst**              | [http://cosmos.testnet.xrplevm.org:1317](http://cosmos.testnet.xrplevm.org:1317)   |
        | **Kintsugi Nodes**        | [https://api-xrp.kintsugi-nodes.com](https://api-xrp.kintsugi-nodes.com)         |
        | **Polkachu**              | [https://xrp-testnet-api.polkachu.com/](https://xrp-testnet-api.polkachu.com/)   |
        | **Cumulo**                | [https://api.xrpl.cumulo.com.es](https://api.xrpl.cumulo.com.es)                |
        | **ITRocket**              | [https://xrplevm-testnet-api.itrocket.net](https://xrplevm-testnet-api.itrocket.net) |
        | **LuckyStar**             | [https://xrpl-testnet-api.luckystar.asia/](https://xrpl-testnet-api.luckystar.asia/) |
        | **Mekong Labs**           | [https://exrpd-testnet-api.mekonglabs.tech/](https://exrpd-testnet-api.mekonglabs.tech/) |
        | **MictoNode**             | [https://xrpl-testnet-api.mictonode.com](https://xrpl-testnet-api.mictonode.com)   |
        | **NodeStake**             | [https://api-t.xrp.nodestake.org](https://api-t.xrp.nodestake.org)               |
        | **UTSA (lesnik)**         | [https://t-xrpl.api.utsa.tech](https://t-xrpl.api.utsa.tech)                     |
        | **XSS**                   | [https://api-xrpl.xsslabs.tech](https://api-xrpl.xsslabs.tech)                   |
        | **Đông QN**               | [https://api.testnet.xrplevm.dongqn.com](https://api.testnet.xrplevm.dongqn.com)   |
        {% /tab %}

        {% tab label="gRPC Endpoints" %}
        | Provider/Validator        | gRPC Endpoint URL                                                                         |
        | ------------------------- | ---------------------------------------------------------------------------------------- |
        | **Peersyst**              | [http://cosmos.testnet.xrplevm.org:9090](http://cosmos.testnet.xrplevm.org:9090)         |
        | **Kintsugi Nodes**        | `grpc-xrp.kintsugi-nodes.com`                                                            |
        | **Polkachu**              | [https://polkachu.com/testnet_public_grpc](https://polkachu.com/testnet_public_grpc)      |
        | **Cumulo**                | `grpc.xrpl.cumulo.com.es`                                                                |
        | **ITRocket**              | `xrplevm-testnet-grpc.itrocket.net:443`                                                 |
        | **LuckyStar**             | [https://xrpl-testnet-grpc.luckystar.asia/](https://xrpl-testnet-grpc.luckystar.asia/)   |
        | **Mekong Labs**           | `exrpd-testnet-grpc.mekonglabs.tech:47090`                                               |
        | **MictoNode**             | `xrpl-testnet-grpc.mictonode.com:22090`                                                  |
        | **NodeStake**             | [https://grpc-t.xrp.nodestake.org:443](https://grpc-t.xrp.nodestake.org:443)                           |
        | **UTSA (lesnik)**         | `t-xrpl.grpc.utsa.tech:433`                                                              |
        | **blockitize**            | [http://xrplevm-testnet-grpc.blockitize.com/](http://xrplevm-testnet-grpc.blockitize.com/) |
        | **p10node**               | `grpc.testnet.xrplevm.p10node.com`<br>`grpc-web.testnet.xrplevm.p10node.com`              |
        | **Đông QN**               | `grpc.testnet.xrplevm.dongqn.com`                                                        |
        {% /tab %}

        {% tab label="EVM RPC Endpoints" %}
        | Provider/Validator        | EVM RPC Endpoint URL                                                                                 |
        | ------------------------- | ---------------------------------------------------------------------------------------------------- |
        | **Peersyst**              | [http://cosmos.testnet.xrplevm.org:26657](http://cosmos.testnet.xrplevm.org:26657)                   |
        | **Grove**                 | [https://xrpl-evm-testnet.rpc.grove.city/v1/0caa84c4](https://xrpl-evm-testnet.rpc.grove.city/v1/0caa84c4) |
        | **NodeStake**             | [https://evmrpc-t.xrp.nodestake.org](https://evmrpc-t.xrp.nodestake.org)                             |
        | **Cumulo**                | [https://json-rpc.xrpl.cumulo.com.es](https://json-rpc.xrpl.cumulo.com.es)                             |
        {% /tab %}

        {% tab label="Snapshot / Genesis" %}
        | Provider/Validator        | Snapshot / Genesis URL                                                                                         |
        | ------------------------- | ---------------------------------------------------------------------------------------------------------------- |
        | **Peersyst**              | [Snapshot](https://evm-sidechain-snapshots-devnet.s3.us-east-1.amazonaws.com/exrpd.tar.lz4)                      |
        | **Kintsugi Nodes**        | [http://kintsugi-nodes.com/ripple/snapshot](http://kintsugi-nodes.com/ripple/snapshot)                          |
        | **Polkachu**              | [https://polkachu.com/testnets/xrp/snapshots](https://polkachu.com/testnets/xrp/snapshots)                       |
        | **blockitize**            | [Genesis](https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json) |
        | **Cumulo**                | [https://cumulo.pro/services/xrplevm/](https://cumulo.pro/services/xrplevm/)                                    |
        | **DeFatRat (TechHubs)**   | [https://xrpl-testnet-snapshots.techhubs.asia/](https://xrpl-testnet-snapshots.techhubs.asia/)                  |
        | **Enigma**                | [Snapshot](https://services.enigma-validator.com/xrp/xrp_365040.tar.lz4) [Genesis](https://xrp-rpc.enigma-validator.com/genesis?)                                      |
        | **ITRocket**              | [https://itrocket.net/services/testnet/xrplevm/](https://itrocket.net/services/testnet/xrplevm/)                |
        | **LuckyStar**             | [Snapshot](https://luckystar-1.gitbook.io/luckystar.asia/testnet/cosmos-eco/xrpl/snapshot)<br>[Genesis](https://xrpl-testnet-services.luckystar.asia/xrpl/genesis.json) |
        | **Mekong Labs**           | [http://exrpd-testnet-snapshots.mekonglabs.tech/](http://exrpd-testnet-snapshots.mekonglabs.tech/)              |
        | **MictoNode**             | [https://services.mictonode.com/xrpl-evm/snapshot](https://services.mictonode.com/xrpl-evm/snapshot)            |
        | **UTSA (lesnik)**         | [https://utsa.gitbook.io/services/testnet/xrpl-evm/snapshots](https://utsa.gitbook.io/services/testnet/xrpl-evm/snapshots) |
        | **Node9x**                | [https://service.node9x.com/testnet/xrpl-evm/service-and-snapshot](https://service.node9x.com/testnet/xrpl-evm/service-and-snapshot) |
        | **NodeStake**             | [https://nodestake.org/xrp](https://nodestake.org/xrp) |
        {% /tab %}

        {% tab label="Documentation / Guide" %}
        | Provider/Validator        | Docs / Guide URL                                                                 |
        | ------------------------- | ------------------------------------------------------------------------------- |
        | **Grove**                 | [RPC Docs](https://docs.grove.city/xrpl-api/specs/core-api)                      |
        | **Polkachu**              | [Installation Guide](https://polkachu.com/testnets/xrp/installation)              |
        | **Cumulo**                | [Node Guide](https://cumulo.pro/services/xrplevm/)                              |
        | **Enigma**                | [Main site](https://enigma-validator.com/stake-with-us/xrp-testnet#services) |
        | **ITRocket**              | [ITRocket XRPL Docs](https://itrocket.net/services/testnet/xrplevm/installation/#installation)            |
        | **LuckyStar**             | [Node Guide](https://luckystar-1.gitbook.io/luckystar.asia/testnet/cosmos-eco/xrpl/node-setup)   |
        | **Mekong Labs**           | [Node Guide](https://github.com/Mekong-labs/NodeGuide/tree/main/XRPL)            |
        | **MictoNode**             | [Basic Guide](https://services.mictonode.com/xrpl-evm/installation)                           |
        | **Trace**                 | [Russian docs](https://trace-team.gitbook.io/xrpl)            |
        | **UTSA (lesnik)**         | [Node Installation](https://utsa.gitbook.io/services/testnet/xrpl-evm/installation)            |
        | **XSS**                   | [Installation](https://services.xsslabs.tech/xrpl-evm/installation)               |
        | **blockitize**            | [Node documentation](https://services.blockitize.com/?project=xrpl-evm) |
        | **node9x**                | [https://service.node9x.com/testnet/xrpl-evm](https://service.node9x.com/testnet/xrpl-evm) |
        | **Imperator.co**          | [https://www.imperator.co/services/chain-services/testnets/xrp](https://www.imperator.co/services/chain-services/testnets/xrp) |
        | **NodeStake**             | [https://nodestake.org/xrp](https://nodestake.org/xrp)                           |
        {% /tab %}

        {% tab label="Other / Monitoring & Tools" %}
        | Provider/Validator        | Additional Info / Tools                                                                                                                           |
        | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
        | **Cumulo**                | [RPC & API Scan](https://cumulo.pro/services/xrplevm/)                                                                        |
        | **ITRocket**              | [Validator Analytics](https://itrocket.net/services/testnet/xrplevm/analytics/validators-performance/?view=uptime&mode=full&range=allTime%20-), [Consensus Analytics](https://itrocket.net/services/testnet/xrplevm/analytics/consensus/), [Peer scanner](https://itrocket.net/services/testnet/xrplevm/public-rpc/) |
        | **node9x**                | [exrpd commands](https://service.node9x.com/testnet/xrpl-evm/command)                                                                                           |
        | **NodesHub**              | [Useful commands](https://services.nodeshub.online/testnet/xrpl/useful-commands)                                                                                        |
        {% /tab %}
    {% /tabs %}

    ---
{% /tab %}

{% tab label="Devnet" %}

## Devnet Public APIs

| Type              | URL                                                                                     |
| ----------------- | --------------------------------------------------------------------------------------- |
| Ethereum JSON RPC | [https://rpc.devnet.xrplevm.org](https://rpc.devnet.xrplevm.org)                        |
| Ethereum JSON WS  | [https://ws.devnet.xrplevm.org](https://ws.devnet.xrplevm.org)                          |
| Tendermint RPC    | [http://cosmos.devnet.xrplevm.org:26657](http://cosmos.devnet.xrplevm.org:26657)        |
| Cosmos gRPC       | [http://cosmos.devnet.xrplevm.org:9090](http://cosmos.devnet.xrplevm.org:9090)          |
| Cosmos API        | [http://cosmos.devnet.xrplevm.org:1317](http://cosmos.devnet.xrplevm.org:1317)          |

{% /tab %}
{% /tabs %}
