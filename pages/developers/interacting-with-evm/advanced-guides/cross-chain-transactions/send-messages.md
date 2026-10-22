# Send messages from XRPL

In addition to transferring tokens across chains, cross-chain bridges can enable communication between blockchains through general message passing protocols. These protocols allow developers to call functions on other connected chains. For example, smart contracts on EVM-compatible chains, such as the XRPL EVM, can be triggered directly from the XRP Ledger (XRPL), Ethereum, Polygon and other supported chains.

To send a message from one chain to another, there are some key aspects to consider:

- The destination chain must be an EVM-compatible chain, as executing messages requires a smart contract.
- The destination contract must be compatible with the bridge protocol being used (specific interface requirements vary by bridge).

## Example: Sending a message using Axelar

[Axelar's General Message Passing (GMP)](https://docs.axelar.dev/dev/general-message-passing/overview/) is one protocol that enables cross-chain message passing. With Axelar GMP, developers can call any function on any other connected chain.

### Requirements for Axelar

- The destination contract address must inherit from [`AxelarExecutable`](https://github.com/axelarnetwork/axelar-gmp-sdk-solidity/blob/main/contracts/executable/AxelarExecutable.sol)

### Sending a message from XRP Ledger to XRPL EVM

To send a message from the XRP Ledger (XRPL) to the XRPL EVM, a `Payment` transaction is used with the following key parameters:

- `Amount`: Represents the value of the asset to be sent. If the message does not include an asset transfer, this can be set to 1 drop (the smallest XRP unit).
- `Destination`: The address of the Gateway on the XRP Ledger.
  - **Mainnet**: `rfmS3zqrQrka8wVyhXifEeyTwe8AMz2Yhw`
  - **Testnet**: `rNrjh1KGZk2jBR3wPfAQnoidtFFYQKbQn2`

  These are the `xrpl` chain entries in Axelar's [mainnet](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/mainnet.json) and [testnet](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/testnet.json) configuration files.
- `Memos`: Hex-encoded data required for the function call, including:
  - The _type_ of call to initiate.
  - The _destination chain_ on the Axelar network (`xrpl-evm` for both Testnet and Mainnet).
  - The _contract address_ on the destination chain to which the message is sent.
  - The _payload hash_, which contains the data to be sent to the destination contract. This payload must be ABI-encoded. The [ethers AbiCoder](https://docs.ethers.org/v6/api/abi/abi-coder/#AbiCoder-encode) can be used for encoding the payload.

See [Axelar's documentation](https://github.com/axelarnetwork/axelar-contract-deployments/tree/main/xrpl#general-message-passing) for a guide on making GMP contract calls.
