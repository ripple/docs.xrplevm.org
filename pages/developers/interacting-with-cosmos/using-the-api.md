# Using the Cosmos API

Cosmos SDK exposes three primary interfaces for interacting with a node. Each interface is accessible through a different port:

- **gRPC**: Port `9090`
- **REST**: Port `1317`
- **CometBFT RPC**: Port `26657`

Nodes also expose other endpoints, such as the CometBFT P2P endpoint, or the [Prometheus endpoint](https://docs.cometbft.com/v1.0/explanation/core/metrics), which are not directly related to the Cosmos SDK. For detailed instructions on configuring these additional CometBFT endpoints, refer to the [CometBFT Configuration Manual](https://docs.cometbft.com/v1.0/references/config/).

## gRPC

The Cosmos SDK uses [Protobuf](https://protobuf.dev/) as its primary encoding library. This allows seamless integration with [gRPC](https://grpc.io/), a modern, open-source, high performance RPC framework that supports multiple languages.

### Querying gRPC

Below is an example of querying the [gRPC](https://grpc.io/) server for the list of governance proposals using the [grpcurl](https://github.com/fullstorydev/grpcurl) command-line tool:

```bash
grpcurl -plaintext cosmos.xrplevm.org:9090 cosmos.gov.v1.Query/Proposals
```

<details>

<summary>Response</summary>

For brevity, only the most recent proposal is shown here.

```json
    {
        "proposals": [
            {
                "id": "49",
                "messages": [
                    {
                    "@type": "/ethermint.evm.v1.MsgUpdateParams",
                    "authority": "ethm10d07y265gmmuvt4z0w9aw880jnsr700jpva843",
                    "params": {
                        "evmDenom": "axrp",
                        "extraEips": [
                        "ethereum_3855"
                        ],
                        "chainConfig": {
                        "homesteadBlock": "0",
                        "daoForkBlock": "0",
                        "daoForkSupport": true,
                        "eip150Block": "0",
                        "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
                        "eip155Block": "0",
                        "eip158Block": "0",
                        "byzantiumBlock": "0",
                        "constantinopleBlock": "0",
                        "petersburgBlock": "0",
                        "istanbulBlock": "0",
                        "muirGlacierBlock": "0",
                        "berlinBlock": "0",
                        "londonBlock": "0",
                        "arrowGlacierBlock": "0",
                        "grayGlacierBlock": "0",
                        "mergeNetsplitBlock": "0",
                        "shanghaiBlock": "0",
                        "cancunBlock": "0"
                        },
                        "accessControl": {
                        "create": {},
                        "call": {}
                        },
                        "activeStaticPrecompiles": [
                        "0x0000000000000000000000000000000000000100",
                        "0x0000000000000000000000000000000000000400",
                        "0x0000000000000000000000000000000000000804",
                        "0x0000000000000000000000000000000000000805"
                        ]
                    }
                    }
                ],
                "status": "PROPOSAL_STATUS_PASSED",
                "finalTallyResult": {
                    "yesCount": "16000000",
                    "abstainCount": "0",
                    "noCount": "0",
                    "noWithVetoCount": "0"
                },
                "submitTime": "2024-12-30T09:11:09.219483970Z",
                "depositEndTime": "2025-01-01T09:11:09.219483970Z",
                "totalDeposit": [
                    {
                    "denom": "axrp",
                    "amount": "250000000000000000000"
                    }
                ],
                "votingStartTime": "2024-12-30T09:11:09.219483970Z",
                "votingEndTime": "2024-12-31T09:11:09.219483970Z",
                "metadata": "ipfs://CID",
                "title": "Replace the 0x0 Address with 0x805 in active_precompiles",
                "summary": "A bug in active_precompiles is causing failures in both the precompiles and the x/evm module due to incorrect parameter initialization. This proposal addresses the issue by substituting the 0x0 address with 0x805, ensuring stable and proper functionality for the precompiles and the x/evm module.",
                "proposer": "ethm1akwntffy4us9nhgcmgjxdg78v5w3xtwletyjmv",
                "expedited": true
            }
        ],
    },
    "pagination": {
        "total": "49"
    }
```

</details>

For more details on the Cosmos SDK gRPC server, refer to the [Cosmos SDK gRPC Server documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#grpc-server).

The full gRPC server specification is available at [buf.build/cosmos/cosmos-sdk](https://buf.build/cosmos/cosmos-sdk).

## REST API

The Cosmos SDK also provides a REST API, allowing users to interact with the blockchain using HTTP requests. This interface is particularly useful for applications or users who prefer a straightforward RESTful approach to querying data.

The following example demonstrates how to query the list of governance proposals using [curl](https://github.com/curl/curl):

```bash
curl -X GET "http://cosmos.xrplevm.org:1317/cosmos/gov/v1/proposals" -H "accept: application/json"
```

<details>

<summary>Response</summary>

For brevity, only the most recent proposal is shown here.

```json
    {
        "proposals": [
            {
                "id": "49",
                "messages": [{
                    "@type": "/ethermint.evm.v1.MsgUpdateParams",
                    "authority": "ethm10d07y265gmmuvt4z0w9aw880jnsr700jpva843",
                    "params": {
                        "evm_denom": "axrp",
                        "extra_eips": ["ethereum_3855"],
                        "chain_config": {
                            "homestead_block": "0",
                            "dao_fork_block": "0",
                            "dao_fork_support": true,
                            "eip150_block": "0",
                            "eip150_hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
                            "eip155_block": "0",
                            "eip158_block": "0",
                            "byzantium_block": "0",
                            "constantinople_block": "0",
                            "petersburg_block": "0",
                            "istanbul_block": "0",
                            "muir_glacier_block": "0",
                            "berlin_block": "0",
                            "london_block": "0",
                            "arrow_glacier_block": "0",
                            "gray_glacier_block": "0",
                            "merge_netsplit_block": "0",
                            "shanghai_block": "0",
                            "cancun_block": "0"
                        },
                        "allow_unprotected_txs": false,
                        "evm_channels": [],
                        "access_control": {
                            "create": {
                                "access_type": "ACCESS_TYPE_PERMISSIONLESS",
                                "access_control_list": []
                            },
                            "call": {
                                "access_type": "ACCESS_TYPE_PERMISSIONLESS",
                                "access_control_list": []
                            }
                        },
                        "active_static_precompiles": ["0x0000000000000000000000000000000000000100", "0x0000000000000000000000000000000000000400", "0x0000000000000000000000000000000000000804", "0x0000000000000000000000000000000000000805"]
                    }
                }],
                "status": "PROPOSAL_STATUS_PASSED",
                "final_tally_result": {
                    "yes_count": "16000000",
                    "abstain_count": "0",
                    "no_count": "0",
                    "no_with_veto_count": "0"
                },
                "submit_time": "2024-12-30T09:11:09.219483970Z",
                "deposit_end_time": "2025-01-01T09:11:09.219483970Z",
                "total_deposit": [{
                    "denom": "axrp",
                    "amount": "250000000000000000000"
                }],
                "voting_start_time": "2024-12-30T09:11:09.219483970Z",
                "voting_end_time": "2024-12-31T09:11:09.219483970Z",
                "metadata": "ipfs://CID",
                "title": "Replace the 0x0 Address with 0x805 in active_precompiles",
                "summary": "A bug in active_precompiles is causing failures in both the precompiles and the x/evm module due to incorrect parameter initialization. This proposal addresses the issue by substituting the 0x0 address with 0x805, ensuring stable and proper functionality for the precompiles and the x/evm module.",
                "proposer": "ethm1akwntffy4us9nhgcmgjxdg78v5w3xtwletyjmv",
                "expedited": true,
                "failed_reason": ""
            }
        ],
    },
    "pagination": {
        "next_key": null,
        "total": "49"
    }
```

</details>

Learn more about the Cosmos REST server in the [Cosmos SDK REST Server documentation](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest#rest-server).

Access the full REST API specification directly at [cosmos.xrplevm.org:1317](http://cosmos.xrplevm.org:1317/).

## CometBFT RPC

In addition to the Cosmos SDK, CometBFT independently exposes an RPC server. This server provides access to consensus-related data and includes methods for interacting with the Cosmos SDK through the abci_query endpoint.

The following example demonstrates how to query the list of governance proposals using [curl](https://github.com/curl/curl):

```bash
curl -X GET 'http://cosmos.xrplevm.org:26657/abci_query?path="/cosmos.gov.v1.Query/Proposals"' -H "accept: application/json"
```

<details>

<summary>Response</summary>

The response includes metadata about the request and the encoded result. The `value` field (which has been trimmed for brevity) contains the encoded data, that must be decoded (typically Base64) to extract the full content.

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
