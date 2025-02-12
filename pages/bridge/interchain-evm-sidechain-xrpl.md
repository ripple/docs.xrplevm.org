---
html: interchain-transfer-evm-sidechain-xrpl.html
blurb: Axelar interchain transfer to XRPL.
labels:
  - Interoperability
status: not_enabled
---

# Axelar Interchain Transfer from XRPL

You can bridge native XRP from XRPL to XRPL EVM sidechain using Axelar Portal. In this example, we will transfer 5 XRP to an account in the XRPL EVM Sidechain.

1. Set XRPL as the source chain and XRPL EVM sidechain as the destination chain.
   ![portal set chains](./img/axelar-xrpl-set-chains.png)

2. Select the faucet account on the XRPL chain.

![portal set chains](./img/axelar-xrpl-select-faucet.png)

3. Connect your wallet using Metamask for the XRPL EVM Sidechain.

![portal connect metamask](./img/axelar-xrpl-connect-metamask.png)

4. Choose the token you want to transfer and set the desired amount (currently, only XRP is supported).

Click on `Transfer` and confirm the transaction in your wallet.

![portal transfer](./img/axelar-xrpl-wait-faucet-transaction.png)

![portal transaction executed](./img/axelar-xrpl-await-transaction.png)

Once the transaction is executed, you can see the transaction details in the following modal.

![portal transaction list](./img/axelar-xrpl-tx-success.png)

To confirm the transfer, you can go to the XRPL EVM Sidechain explorer and search for the transaction hash.

![portal transaction list](./img/axelar-xrpl-verify-destination-transfer.png)
