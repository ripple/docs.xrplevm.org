---
html: axelar-relay-transfer-xrpl-evm-sidechain-to-xrpl.html
blurb: Relay messages manually from XRPL EVM Sidechain to XRPL.
labels:
  - Interoperability
status: not_enabled
---
# XRPL EVM Sidechain to XRPL transfer

This guide shows how relay a transfer message manually from an XRPL EVM Sidechain to XRPL.

1. Send a `verify_messages` transaction to XRPL EVM Sidechain's source gateway address on Axelar. 

```bash
axelard tx wasm execute '{
    verify_messages: [
        {
            cc_id: {
                source_chain: "xrpl-evm-sidechain",
                message_id: MESSAGE_ID,
            },
            destination_chain: "axelarnet",
            destination_address: "axelar10jzzmv5m7da7dn2xsfac0yqe7zamy34uedx3e28laq0p6f3f8dzqp649fp",
            source_address: "0x43F2ccD4E27099b5F580895b44eAcC866e5F7Bb1",
            payload_hash: PAYLOAD_HASH,
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

2. Route the message.

```bash
axelard tx wasm execute '{
    route_messages: [
        {
            cc_id: {
                source_chain: "xrpl-evm-sidechain",
                message_id: MESSAGE_ID,
            },
            destination_chain: "axelarnet",
            destination_address: "axelar10jzzmv5m7da7dn2xsfac0yqe7zamy34uedx3e28laq0p6f3f8dzqp649fp",
            source_address: "0x43F2ccD4E27099b5F580895b44eAcC866e5F7Bb1",
            payload_hash: PAYLOAD_HASH,
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

3. Execute the ITS Hub message. 

```bash
axelard tx wasm execute axelar1yvfcrdke7fasxfaxx2r706h7h85rnk3w68cc5f4fkmafz5j755ssl8h9p0 '{
    execute: {
        cc_id: {
            source_chain: "xrpl-evm-sidechain",
            message_id: MESSAGE_ID,
        },
        payload: TX_PAYLOAD,
    },
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

4. Construct transfer proof.

```bash
axelard tx wasm execute axelar1upqj9paf2qhhp49fjtk4ghwnv2mzcqnpglggr0hds8dy9a4stz2st0ewsh '{
    construct_proof: {
        message_id: {
            source_chain: "axelarnet",
            message_id: MESSAGE_ID_FROM_STEP_3,
        },
        payload: CALCULATED_PAYLOAD,
    },
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

5. Query the contract state to check if the proof was completed. You can find the `multisig_session_id` in the response of the `construct_proof` transaction.

```bash
axelard q wasm contract-state smart axelar1upqj9paf2qhhp49fjtk4ghwnv2mzcqnpglggr0hds8dy9a4stz2st0ewsh '{
  proof: {
      multisig_session_id: multisigSessionId,
  },
}' --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657 --output json
```

6. If the proof is completed, a XRPL transaction blob will be returned in the response. Submit the transaction blob to XRPL.

```javascript
const client = new Client("wss://s.devnet.rippletest.net:51233");
const request: SubmitRequest = {
    command: "submit",
    tx_blob: txBlob,
    fail_hard: true,
};

await client.connect();
const response = await client.request(request);

await client.disconnect();
```

Check that the transaction is successful.

7. Submit the proof to XRPL Axelar gateway.

```bash
axelard tx wasm execute axelar13w698a6pjytxj6jzprs6pznaxhan3flhf76fr0nc7jg3udcsa07q9c7da3 '{
    verify_messages: [
        {
            prover_message: SUBMITTED_TRANSACTION_BLOB_TX_HASH,
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

8. Update the transaction status on XRPL Multisig prover contract. The `signer_public_keys` can be found in the `Signers` field of the submitted transaction blob transaction.

```bash
axelard tx wasm execute axelar1upqj9paf2qhhp49fjtk4ghwnv2mzcqnpglggr0hds8dy9a4stz2st0ewsh '{
   update_tx_status: {
        multisig_session_id: MULTISIG_SESSION_ID,
        message_id: SUBMITTED_TRANSACTION_BLOB_TX_HASH,
        message_status: "succeeded_on_source_chain",
        signer_public_keys: [
            {
                ecdsa: SIGNER_PUBLIC_KEY,
            },
        ],
    },
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657

