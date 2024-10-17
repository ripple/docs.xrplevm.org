---
html: axelar-interchain-transfer-evm-sidechain-avalanche.html
blurb: Axelar interchain transfer to Avalanche Fuji.
labels:
  - Interoperability
status: not_enabled
---
# Axelar Interchain Transfer to Avalanche Fuji

You can transfer tokens from XRPL EVM sidechain to Avalanche Fuji using Axelar Portal. In this example, we will transfer 10 XRP to Avalanche Fuji as axlXRP.

1. Set XRPL EVM sidechain as the source chain and Avalanche Fuji as the destination chain.
![portal set chains](../img/axelar-set-chains.png)

2. Connect your wallet using Metamask for both chains.

![portal connect metamask](../img/axelar-connect-metamask.png)

3. Choose the token you want to transfer and set the desired amount.

![portal transfer](../img/axelar-set-amount.png)

4. Click on `Transfer` and confirm the transaction in your wallet.

![portal transfer](../img/axelar-sign-transaction.png)

Once the transaction is executed, you can see the transaction details in the following modal.

![portal transaction list](../img/axelar-tx-success.png)

To confirm the transfer, you can go to the Avalanche Fuji explorer and search for the transaction hash.

![portal transaction list](../img/axelar-verify-destination-transfer.png)

