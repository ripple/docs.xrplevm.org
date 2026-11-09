| Destination | From XRPL EVM (Source → Dest) | To XRPL EVM (Dest → Source) |
| ----------- | ----------------------------- | --------------------------- |
| Injective   | `channel-0`                   | `channel-436`               |
| Osmosis     | `channel-3`                   | `channel-104325`            |

{% admonition type="warning" name="Inactive channels" %}
Only the two channels above are active. Every other `transfer` channel on Mainnet (`channel-1`, `channel-2` and `channel-4` to `channel-9`, including older channels to Elys Network, Cosmos Hub, Noble, Celestia, Sunrise and a second Injective channel) still exists on-chain, but its light client has expired, so transfers over it fail. Do not use them.

To check a channel yourself, read its client ID and then the client status:

```bash
CLIENT=$(curl -s https://cosmos-api.xrplevm.org/ibc/core/channel/v1/channels/channel-0/ports/transfer/client_state | jq -r .identified_client_state.client_id)
curl -s https://cosmos-api.xrplevm.org/ibc/core/client/v1/client_status/$CLIENT
```

A usable channel returns `"status": "Active"`.
{% /admonition %}
