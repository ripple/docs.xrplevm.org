---
blurb: Send cross-chain messages from XRPL EVM smart contracts with Wormhole.
labels:
  - Interoperability
---

# Send Cross-Chain Messages with Wormhole

Wormhole's **Core Contract** lets any smart contract on the XRPL EVM publish arbitrary messages that can be verified and consumed on 40+ connected chains. This guide shows the raw messaging flow in Solidity: publishing a message on the source chain and verifying its **VAA** (Verifiable Action Approval) on the destination chain.

{% admonition type="warning" name="Delivery on Mainnet uses the Executor" %}
On **XRPL EVM Mainnet** there is no legacy `IWormholeRelayer` deployment, so tutorials built on `sendPayloadToEvm` only work on **Testnet**. For production delivery, publish through the Core Contract and request delivery through the [Executor framework](https://wormhole.com/docs/protocol/infrastructure/relayers/executor-framework/) — the [Wormhole TypeScript SDK](https://wormhole.com/docs/products/messaging/get-started/) manages Executor quoting and execution for you.
{% /admonition %}

## Prerequisites

- A funded deployer account on [XRPL EVM Mainnet or Testnet](../../users/getting-started/connect-to-the-xrpl-evm.md)
- [Foundry](https://getfoundry.sh/) (or Hardhat) for contract development
- The Wormhole Solidity SDK:

```bash
forge install wormhole-foundation/wormhole-solidity-sdk@v1.0.0
```

Use tagged releases of the SDK — the `main` branch is a nightly build.

## Publish a Message (Source Chain)

Call `publishMessage` on the Core Contract (`0xaBf89de706B583424328B54dD05a8fC986750Da8` on both XRPL EVM Mainnet and Testnet), paying the current message fee:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import "wormhole-solidity-sdk/interfaces/IWormhole.sol";

contract HelloWormhole {
    IWormhole public immutable wormhole;

    constructor(address wormholeCore) {
        wormhole = IWormhole(wormholeCore);
    }

    function sendMessage(
        string memory message
    ) external payable returns (uint64 sequence) {
        uint256 wormholeFee = wormhole.messageFee();
        require(msg.value >= wormholeFee, "insufficient message fee");

        sequence = wormhole.publishMessage{value: wormholeFee}(
            0,                  // nonce: free integer field
            abi.encode(message),
            1                   // consistencyLevel: finality required before attestation
        );
    }
}
```

Once the transaction finalizes, the Guardian network observes the emitted message and produces a signed VAA. You can fetch it from the [WormholeScan API](https://docs.wormholescan.io) or track it at `https://wormholescan.io/#/tx/<txHash>` (append `?network=Testnet` for Testnet).

## Verify and Consume the VAA (Destination Chain)

On the destination chain, pass the encoded VAA to `parseAndVerifyVM` on that chain's Core Contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import "wormhole-solidity-sdk/interfaces/IWormhole.sol";

contract HelloWormholeReceiver {
    IWormhole public immutable wormhole;
    // Trusted emitter (source contract) per Wormhole chain ID
    mapping(uint16 => bytes32) public registeredEmitters;
    // Replay protection
    mapping(bytes32 => bool) public consumedMessages;

    constructor(address wormholeCore) {
        wormhole = IWormhole(wormholeCore);
    }

    function receiveMessage(bytes memory encodedVaa) public {
        (
            IWormhole.VM memory vm,
            bool valid,
            string memory reason
        ) = wormhole.parseAndVerifyVM(encodedVaa);

        require(valid, reason);
        require(
            registeredEmitters[vm.emitterChainId] == vm.emitterAddress,
            "unknown emitter"
        );
        require(!consumedMessages[vm.hash], "message already consumed");
        consumedMessages[vm.hash] = true;

        string memory message = abi.decode(vm.payload, (string));
        // Your application logic here
    }
}
```

### Safety Checks

Wormhole verifies Guardian signatures, but your contract is responsible for application-level checks:

1. **Emitter validation**: Only accept VAAs whose `emitterChainId` and `emitterAddress` match contracts you trust. XRPL EVM's Wormhole chain ID is `57`.
2. **Replay protection**: Store the VAA digest (`vm.hash`) and reject duplicates.
3. **Finality**: Choose a `consistencyLevel` appropriate for your use case when publishing.

## Message Delivery

Publishing a message does not deliver it — someone must submit the VAA to the destination chain. Your options on XRPL EVM:

- **Executor (recommended)**: Request permissionless execution with an off-chain quote; independent relay providers deliver the VAA. The [Wormhole SDK](https://wormhole.com/docs/products/messaging/get-started/) handles quotes and execution requests. The Executor contract on XRPL EVM is listed in [Deployed Contracts](./deployed-contracts.md).
- **Self-relay**: Fetch the signed VAA from the WormholeScan API and submit it to your destination contract yourself — useful for testing and for flows where your backend already submits transactions.
- **Legacy Wormhole Relayer (Testnet only)**: `IWormholeRelayer.sendPayloadToEvm` works on XRPL EVM **Testnet** (`0x362fca37E45fe1096b42021b543f462D49a5C8df`) and is the model used by the official [cross-chain contracts tutorial](https://wormhole.com/docs/products/messaging/tutorials/cross-chain-contracts/). Do not ship Mainnet integrations against it.

## Next Steps

- [Wormhole Core Contracts guide](https://wormhole.com/docs/products/messaging/guides/core-contracts/)
- [Executor framework](https://wormhole.com/docs/protocol/infrastructure/relayers/executor-framework/)
- [Transfer Tokens with Wormhole](./transfer-tokens.md)
