# Using the Cosmos API

Cosmos SDK exposes three primary interfaces for interacting with a node. Each interface is accessible through a different port:

- **gRPC**: Port `9090`
- **REST**: Port `1317`
- **CometBFT RPC**: Port `26657`

Nodes also expose other endpoints, such as the CometBFT P2P endpoint, or the [Prometheus endpoint](https://docs.cosmos.network/cometbft/latest/docs/core/metrics), which are not directly related to the Cosmos SDK. For detailed instructions on configuring these additional CometBFT endpoints, refer to the [CometBFT Configuration Manual](https://docs.cosmos.network/cometbft/latest/docs/core/configuration).

{% tabs %}

{% tab label="Mainnet" %}

## gRPC (Mainnet)

Below is an example of querying the gRPC server for the latest governance proposal on **Mainnet**:

```bash
grpcurl -d '{"pagination": {"reverse": true, "limit": 1}}' cosmos-grpc.xrplevm.org:443 cosmos.gov.v1.Query/Proposals
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "38",
      "messages": [
        {
          "@type": "/ibc.core.client.v1.MsgIBCSoftwareUpgrade",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      ...
    }
  ],
  "pagination": {
    "next_key": "AAAAAAAAACU="
  }
}
```

</details>

For more details on the Cosmos SDK gRPC server, refer to the [Cosmos SDK gRPC Server documentation](https://docs.cosmos.network/sdk/v0.53/learn/advanced/grpc_rest#grpc-server).  
The full gRPC server specification is available at [buf.build/cosmos/cosmos-sdk](https://buf.build/cosmos/cosmos-sdk).

---

## REST API (Mainnet)

Example REST API query for the latest governance proposal on **Mainnet**:

```bash
curl -X GET "https://cosmos-api.xrplevm.org/cosmos/gov/v1/proposals?pagination.reverse=true&pagination.limit=1" -H "accept: application/json"
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "38",
      "messages": [
        {
          "@type": "/ibc.core.client.v1.MsgIBCSoftwareUpgrade",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      ...
    }
  ],
  "pagination": {
    "next_key": "AAAAAAAAACU=",
    "total": "0"
  }
}
```

</details>

Learn more about the Cosmos REST server in the [Cosmos SDK REST Server documentation](https://docs.cosmos.network/sdk/v0.53/learn/advanced/grpc_rest#rest-server).  
Access the full REST API specification at [cosmos-api.xrplevm.org](https://cosmos-api.xrplevm.org).

---

## CometBFT RPC (Mainnet)

Example CometBFT RPC query for governance proposals on **Mainnet**:

```bash
curl -X GET 'https://cosmos-rpc.xrplevm.org/abci_query?path="/cosmos.gov.v1.Query/Proposals"' -H "accept: application/json"
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
      "height": "8262000",
      "codespace": ""
    }
  }
}
```

</details>

Explore the [Cosmos CometBFT RPC documentation](https://docs.cosmos.network/sdk/v0.53/learn/advanced/grpc_rest#cometbft-rpc) for more details.  
Access the CometBFT RPC interface at [cosmos-rpc.xrplevm.org](https://cosmos-rpc.xrplevm.org).
{% /tab %}

{% tab label="Testnet" %}

## gRPC (Testnet)

Below is an example of querying the gRPC server for the latest governance proposal on **Testnet**:

```bash
grpcurl -d '{"pagination": {"reverse": true, "limit": 1}}' cosmos-grpc.testnet.xrplevm.org:443 cosmos.gov.v1.Query/Proposals
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "51",
      "messages": [
        {
          "@type": "/poa.MsgAddValidator",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_VOTING_PERIOD",
      ...
    }
  ],
  "pagination": {
    "next_key": "AAAAAAAAADI="
  }
}
```

</details>

For more details on the Cosmos SDK gRPC server, refer to the [Cosmos SDK gRPC Server documentation](https://docs.cosmos.network/sdk/v0.53/learn/advanced/grpc_rest#grpc-server).  
The full gRPC server specification is available at [buf.build/cosmos/cosmos-sdk](https://buf.build/cosmos/cosmos-sdk).

---

## REST API (Testnet)

Example REST API query for the latest governance proposal on **Testnet**:

```bash
curl -X GET "https://cosmos-api.testnet.xrplevm.org/cosmos/gov/v1/proposals?pagination.reverse=true&pagination.limit=1" -H "accept: application/json"
```

<details>
<summary>Response (truncated example)</summary>

```json
{
  "proposals": [
    {
      "id": "51",
      "messages": [
        {
          "@type": "/poa.MsgAddValidator",
          ...
        }
      ],
      "status": "PROPOSAL_STATUS_VOTING_PERIOD",
      ...
    }
  ],
  "pagination": {
    "next_key": "AAAAAAAAADI=",
    "total": "0"
  }
}
```

</details>

Learn more about the Cosmos REST server in the [Cosmos SDK REST Server documentation](https://docs.cosmos.network/sdk/v0.53/learn/advanced/grpc_rest#rest-server).  
Access the full REST API specification at [cosmos-api.testnet.xrplevm.org](https://cosmos-api.testnet.xrplevm.org).

---

## CometBFT RPC (Testnet)

Example CometBFT RPC query for governance proposals on **Testnet**:

```bash
curl -X GET 'https://cosmos-rpc.testnet.xrplevm.org/abci_query?path="/cosmos.gov.v1.Query/Proposals"' -H "accept: application/json"
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
      "height": "9190000",
      "codespace": ""
    }
  }
}
```

</details>

Explore the [Cosmos CometBFT RPC documentation](https://docs.cosmos.network/sdk/v0.53/learn/advanced/grpc_rest#cometbft-rpc) for more details.  
Access the CometBFT RPC interface at [cosmos-rpc.testnet.xrplevm.org](https://cosmos-rpc.testnet.xrplevm.org).
{% /tab %}
{% /tabs %}

With these endpoints, you can query governance proposals and other data on Mainnet and Testnet. The same commands work for other Cosmos SDK modules.
