---
seo:
  description: "How Axelar relays transfers and messages between the XRP Ledger and the XRPL EVM Sidechain, and what to do when a message is not delivered."
---

# Relay messages manually

{% partial file="/snippets/_axelar-relaying-disclaimer.md" /%}

Transfers and messages between the XRP Ledger and the XRPL EVM Sidechain go through the Axelar Amplifier Gateway on each chain. Axelar relayers pick up the source transaction, get it verified by the Axelar network, approve it on the destination gateway and execute it on the destination chain. For XRP and Interchain Token Service (ITS) transfers, and for General Message Passing calls that paid enough gas, this happens without any action from you.

## Tracking a transfer or message

Search for the source transaction hash on Axelarscan to see each step (called, confirmed, approved, executed):

- **Mainnet**: [axelarscan.io/gmp/search](https://axelarscan.io/gmp/search)
- **Testnet**: [testnet.axelarscan.io/gmp/search](https://testnet.axelarscan.io/gmp/search)

## When a message is not delivered

If a message stays in an intermediate status:

- **Not approved**: the source transaction usually did not pay enough gas. Check the gas status on Axelarscan and add gas if needed. Estimate the fee with the Axelarscan API (`POST https://api.axelarscan.io/gmp/estimateITSFee` for ITS transfers, `POST https://api.axelarscan.io/gmp/estimateGasFee` for GMP calls).
- **Approved but not executed**: you can execute it yourself by calling `execute` on the destination contract with the `commandId`, source chain, source address and payload, as shown in [General Message Passing](./general-message-passing.md).

For the underlying commands, see Axelar's [XRPL integration guide](https://github.com/axelarnetwork/axelar-contract-deployments/tree/main/xrpl).
