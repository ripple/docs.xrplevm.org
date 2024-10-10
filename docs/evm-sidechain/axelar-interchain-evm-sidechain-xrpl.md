---
html: axelar-interchain-transfer-evm-sidechain-xrpl.html
blurb: Axelar interchain transfer to XRPL.
labels:
  - Interoperability
status: not_enabled
---
# Axelar Interchain Transfer from XRPL

## Using Portal

You can bridge native XRP from XRPL to XRPL EVM sidechain using Axelar Portal. In this example, we will transfer 5 XRP to an account in the XRPL EVM Sidechain.

First, set XRPL as the source chain and XRPL EVM sidechain as the destination chain.
![portal set chains](../img/axelar-xrpl-set-chains.png)

Then, select the faucet account on the XRPL chain.

![portal set chains](../img/axelar-xrpl-select-faucet.png)

Then, connect your wallet using Metamask for the XRPL EVM Sidechain.

![portal connect metamask](../img/axelar-xrpl-connect-metamask.png)

Once your wallet is connected, you can start choosing the token you want to transfer and set the desired amount (currently, only XRP is supported). Click on `Transfer` and confirm the transaction in your wallet.

![portal transfer](../img/axelar-xrpl-wait-faucet-transaction.png)

Then, await the transaction to be executed.

![portal transaction executed](../img/axelar-xrpl-await-transaction.png)

Once the transaction is executed, you can see the transaction details in the following modal.

![portal transaction list](../img/axelar-xrpl-tx-success.png)

To confirm the transfer, you can go to the XRPL EVM Sidechain explorer and search for the transaction hash.

![portal transaction list](../img/axelar-xrpl-verify-destination-transfer.png)

