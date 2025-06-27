# Using the Cosmos API

Cosmos SDK exposes three primary interfaces for interacting with a node. Each interface is accessible through a different port:

- **gRPC**: Port `9090`
- **REST**: Port `1317`
- **CometBFT RPC**: Port `26657`

Nodes also expose other endpoints, such as the CometBFT P2P endpoint, or the [Prometheus endpoint](https://docs.cometbft.com/v1.0/explanation/core/metrics), which are not directly related to the Cosmos SDK. For detailed instructions on configuring these additional CometBFT endpoints, refer to the [CometBFT Configuration Manual](https://docs.cometbft.com/v1.0/references/config/).

{% tabs %}

{% tab label="Mainnet" %}

## gRPC (Mainnet)

Below is an example of querying the gRPC server for governance proposals on **Mainnet**:

```bash
grpcurl -plaintext cosmos.xrplevm.org:9090 cosmos.gov.v1.Query/Proposals
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "49",
      "messages": [
        {
          "@type": "/ethermint.evm.v1.MsgUpdateParams",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      ...
    }
  ],
  "pagination": {
    "total": "49"
  }
}
```

</details>

For more details on the Cosmos SDK gRPC server, refer to the [Cosmos SDK gRPC Server documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#grpc-server).  
The full gRPC server specification is available at [buf.build/cosmos/cosmos-sdk](https://buf.build/cosmos/cosmos-sdk).

---

## REST API (Mainnet)

Example REST API query for governance proposals on **Mainnet**:

```bash
curl -X GET "http://cosmos.xrplevm.org:1317/cosmos/gov/v1/proposals" -H "accept: application/json"
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "49",
      "messages": [
        {
          "@type": "/ethermint.evm.v1.MsgUpdateParams",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      ...
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "49"
  }
}
```

</details>

Learn more about the Cosmos REST server in the [Cosmos SDK REST Server documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#rest-server).  
Access the full REST API specification at [cosmos.xrplevm.org:1317](http://cosmos.xrplevm.org:1317).

---

## CometBFT RPC (Mainnet)

Example CometBFT RPC query for governance proposals on **Mainnet**:

```bash
curl -X GET 'http://cosmos.xrplevm.org:26657/abci_query?path="/cosmos.gov.v1.Query/Proposals"' -H "accept: application/json"
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "response": {
      "code": 0,
      "log": "",
      "info": "",
      "index": "0",
      "key": null,
      "value": "CqwCCAEShgEKKC9...dsZXR5am12cAESAhAx",
      "proofOps": null,
      "height": "13713161",
      "codespace": ""
    }
  }
}
```

</details>

Explore the [Cosmos CometBFT RPC documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#cometbft-rpc) for more details.  
Access the CometBFT RPC interface at [cosmos.xrplevm.org:26657](http://cosmos.xrplevm.org:26657).
{% /tab %}

{% tab label="Testnet" %}

## gRPC (Testnet)

Below is an example of querying the gRPC server for governance proposals on **Testnet**:

```bash
grpcurl -plaintext cosmos.testnet.xrplevm.org:9090 cosmos.gov.v1.Query/Proposals
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "49",
      "messages": [
        {
          "@type": "/ethermint.evm.v1.MsgUpdateParams",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      ...
    }
  ],
  "pagination": {
    "total": "49"
  }
}
```

</details>

For more details on the Cosmos SDK gRPC server, refer to the [Cosmos SDK gRPC Server documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#grpc-server).  
The full gRPC server specification is available at [buf.build/cosmos/cosmos-sdk](https://buf.build/cosmos/cosmos-sdk).

---

## REST API (Testnet)

Example REST API query for governance proposals on **Testnet**:

```bash
curl -X GET "http://cosmos.testnet.xrplevm.org:1317/cosmos/gov/v1/proposals" -H "accept: application/json"
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "49",
      "messages": [
        {
          "@type": "/ethermint.evm.v1.MsgUpdateParams",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      ...
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "49"
  }
}
```

</details>

Learn more about the Cosmos REST server in the [Cosmos SDK REST Server documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#rest-server).  
Access the full REST API specification at [cosmos.testnet.xrplevm.org:1317](http://cosmos.testnet.xrplevm.org:1317).

---

## CometBFT RPC (Testnet)

Example CometBFT RPC query for governance proposals on **Testnet**:

```bash
curl -X GET 'http://cosmos.testnet.xrplevm.org:26657/abci_query?path="/cosmos.gov.v1.Query/Proposals"' -H "accept: application/json"
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "response": {
      "code": 0,
      "log": "",
      "info": "",
      "index": "0",
      "key": null,
      "value": "CqwCCAEShgEKKC9...dsZXR5am12cAESAhAx",
      "proofOps": null,
      "height": "13713161",
      "codespace": ""
    }
  }
}
```

</details>

Explore the [Cosmos CometBFT RPC documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#cometbft-rpc) for more details.  
Access the CometBFT RPC interface at [cosmos.testnet.xrplevm.org:26657](http://cosmos.testnet.xrplevm.org:26657).
{% /tab %}
{% /tabs %}

With these endpoints, you can query governance proposals (and more) on the **Mainnet**, **Testnet** and **Devent** environments. The same commands can be adapted for other Cosmos SDK features, providing a versatile approach to interacting with the XRPL EVM’s Cosmos-based layer.
