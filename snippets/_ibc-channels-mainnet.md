| Destination | From XRPL EVM (Source → Dest) | To XRPL EVM (Dest → Source) |
| ----------- | ----------------------------- | --------------------------- |
| Injective   | `channel-0`                   | `channel-436`               |
| Osmosis     | `channel-3`                   | `channel-104325`            |

{% admonition type="warning" name="Inactive channels" %}
The channels to Elys Network (`channel-1`), Cosmos Hub (`channel-2`) and Noble (`channel-4`) still exist on-chain, but their light clients have expired, so transfers over them fail. Do not use them. You can check the status of any channel's client with `GET https://cosmos-api.xrplevm.org/ibc/core/client/v1/client_status/<client-id>`.
{% /admonition %}
