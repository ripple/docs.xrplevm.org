---
html: relay-transfer-xrpl-evm-sidechain-to-evm.html
blurb: Relay messages manually from XRPL EVM Sidechain to an EVM chain.
labels:
  - Interoperability
status: not_enabled
---

# XRPL to XRPL EVM Sidechain transfer

This guide shows how relay a transfer message manually from XRPL to an XRPL EVM Sidechain.

1. Send a `verify_messages` transaction

```bash
axelard tx wasm execute axelar13w698a6pjytxj6jzprs6pznaxhan3flhf76fr0nc7jg3udcsa07q9c7da3 '{
    verify_messages: [
        {
            user_message: {
                tx_id: TX_HASH,
                source_address: SOURCE_ADDRESS,
                destination_chain: "xrpl-evm-sidechain",
                destination_address: DESTINATION_ADDRESS,
                amount: { drops: AMOUNT_IN_DROPS },
                payload_hash: PAYLOAD_HASH,
            },
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

2. Route incoming messages for XRPL Axelar gateway.

```bash
axelard tx wasm execute axelar13w698a6pjytxj6jzprs6pznaxhan3flhf76fr0nc7jg3udcsa07q9c7da3 '{
    route_incoming_messages: [
        {
            payload: relayerRequest.userRequest.payload,
            message: {
                user_message: userMessage,
            },
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

3. Execute the ITS Hub message. To compute the payload field, you need to run the following script.

```javascript
const { ethers } = require("ethers");

const interchainTransfer = {
  // TODO: Check if this is correct
    messageType: ethers.BigNumber.from("0"), // 0 - Send transfer
    tokenId: "TOKEN_ID" // XRP token ID
    sourceAddress: "SOURCE_ADDRESS",
    destinationAddress: "DESTINATION_ADDRESS",
    amount: ethers.utils.parseUnits(dropsToXrp("AMOUNT_IN_DROPS").toString()),
    data: "TRANSACTION_DATA",
};

const abiCoder = new ethers.utils.AbiCoder();

const messageEncoded = abiCoder.encode(
    ["uint256", "bytes32", "bytes", "bytes", "uint256", "bytes"],
    [
        interchainTransfer.messageType,
        interchainTransfer.tokenId,
        interchainTransfer.sourceAddress,
        interchainTransfer.destinationAddress,
        interchainTransfer.amount,
        interchainTransfer.data,
    ],
);

const hubMessage = abiCoder.encode(
    ["uint256", "string", "bytes"],
    // TODO: Check if this is correct
    [ethers.BigNumber.from("3"), relayerRequest.userRequest.destinationChain, messageEncoded],
);
```

```bash
axelard tx wasm execute axelar1yvfcrdke7fasxfaxx2r706h7h85rnk3w68cc5f4fkmafz5j755ssl8h9p0 '{
    execute: {
        cc_id: {
            source_chain: "xrpl-evm-sidechain",
            message_id: MESSAGE_ID,
        },
        payload: CALCULATED_PAYLOAD,
    },
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

4. Route the message.

```bash
axelard tx wasm execute axelar1yvfcrdke7fasxfaxx2r706h7h85rnk3w68cc5f4fkmafz5j755ssl8h9p0'{
    route_messages: [
        {
            cc_id: {
                source_chain: "xrpl",
                message_id: MESSAGE_ID_FROM_STEP_3,
            },
            destination_chain: "axelarnet",
            destination_address: "axelar10jzzmv5m7da7dn2xsfac0yqe7zamy34uedx3e28laq0p6f3f8dzqp649fp",
            source_address: "rP9iHnCmJcVPtzCwYJjU1fryC2pEcVqDHv",
            payload_hash: PAYLOAD_HASH,
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

5. Construct transfer proof.

```bash
axelard tx wasm execute axelar19pu8hfnwgc0vjhadmvmgz3w4d2g7d7qlg6jjky9y2mf8ea4vf4usj6ramg '{
    construct_proof: [
        {
            source_chain: "axelarnet",
            message_id: MESSAGE_ID_FROM_STEP_4,
        },
    ],
}' --gas 20000000 --gas-adjustment 1.5 --gas-prices 0.00005uamplifier --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657
```

6. Query the contract state to check if the proof was completed. You can find the `multisig_session_id` in the response of the `construct_proof` transaction.

```bash
axelard q wasm contract-state smart axelar19pu8hfnwgc0vjhadmvmgz3w4d2g7d7qlg6jjky9y2mf8ea4vf4usj6ramg '{
    proof: {
        multisig_session_id: multisigSessionId,
    },
}' --chain-id devnet-amplifier --node http://devnet-amplifier.axelar.dev:26657 --output json
```

7. Send the `execute_data` from the previous step to the destinations Axelar gateway.

```javascript
const provider = new providers.JsonRpcProvider("XRPL_EVM_SIDECHAIN_RPC");
const wallet = new Wallet("YOUR_PRIVATE_KEY", provider);

const tx = await wallet.sendTransaction({
  from: wallet.address,
  to: "0x48CF6E93C4C1b014F719Db2aeF049AA86A255fE2",
  data: "EXECUTE_DATA",
  value: "0",
});
await tx.wait();
```

8. Execute the ITS transfer.

```javascript
const provider = new providers.JsonRpcProvider("XRPL_EVM_SIDECHAIN_RPC");
const wallet = new Wallet("YOUR_PRIVATE_KEY", provider);
const appContract = new Contract(
  "0x48CF6E93C4C1b014F719Db2aeF049AA86A255fE2",
  IAxelarExecutable.abi,
  wallet
);
const tx = await appContract.execute(
  "COMMAND_ID",
  "axelarnet",
  "TRANSLATED_SOURCE_ADDRESS",
  `CALCULATED_PAYLOAD`
);
await tx.wait();
```
