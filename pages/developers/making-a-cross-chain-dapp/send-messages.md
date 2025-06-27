# Send messages from XRPL

In addition to transferring tokens across chains, Axelar enables cross-chain communication through [Axelar's General Message Passing (GMP)](https://docs.axelar.dev/dev/general-message-passing/overview/). GMP enables developers to call any function on any other connected chain. For example, smart contracts on EVM-compatible chains, such as the XRPL EVM, can be triggered directly from the XRP Ledger (XRPL).

To send a message from one chain to another, there are some key aspects to consider:

- The destination chain must be an EVM-compatible chain, as executing messages requires a smart contract.
- The destination contract address must inherit from [`AxelarExecutable`](https://github.com/axelarnetwork/axelar-gmp-sdk-solidity/blob/main/contracts/executable/AxelarExecutable.sol)

## Sending a message from XRP Ledger to XRPL EVM

To send a message from the XRP Ledger (XRPL) to the XRPL EVM, a `Payment` transaction is used with the following key parameters:

- `Amount`: Represents the value of the asset to be sent. If the message does not include an asset transfer, this can be set to 1 drop (the smallest XRP unit).
- `Destination`: The address of the Gateway on the XRP Ledger.
  - [**Mainnet Address**](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/mainnet.json)
  - [**Testnet Address**](https://github.com/axelarnetwork/axelar-contract-deployments/blob/main/axelar-chains-config/info/testnet.json#L2603)
- `Memos`: Hex-encoded data required for the function call, including:
  - The _type_ of call to initiate.
  - The _destination chain_ on the Axelar network.
  - The _contract address_ on the destination chain to which the message is sent.
  - The _payload hash_, which contains the data to be sent to the destination contract. This payload must be ABI-encoded. The [ethers AbiCoder](https://docs.ethers.org/v6/api/abi/abi-coder/#AbiCoder-encode) can be used for encoding the payload.

See [Axelar's documentation](https://github.com/axelarnetwork/axelar-contract-deployments/tree/main/xrpl#general-message-passing) for a guide on making GMP contract calls.
