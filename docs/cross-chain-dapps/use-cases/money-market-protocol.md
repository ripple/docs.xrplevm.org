# Money Market Protocol

**Note:** The money market protocol is in development. It currently supports `ETH` and `XRP` on the Ethereum and XRP test networks. You can use the web app to test deposits, borrowing, repayments, and withdrawals.

A money market protocol is a platform that enables users to lend, borrow, and earn interest on digital assets. These protocols are decentralized and function without the need for intermediaries, such as banks. Key features of money market protocols include:

- Lending and Borrowing: Users deposit assets into the protocol to earn interest. Borrowers can then take out loans, providing another asset as collateral.
- Interest Rates: Rates are calculated algorithmically. When more people borrow assets, interest rates go up in response; more lenders make the interest rate go down.

You can access the money market protocol dapp here: https://xrpl-mm-v1-web.pages.dev/


## Steps

### Install wallet and connect to XRPL Testnet

1. Install the _GemWallet_ Chrom extension, [here](https://chromewebstore.google.com/detail/gemwallet/egebedonbdapoieedfcfkofloclfghab).
2. Switch the XRPL network to `Testnet` in the wallet.
3. Click **FUND WALLET** to create and fund your wallet with test XRP. You will have received 90 XRP (plus 10 XRP in minimum deposit for account activation).
4. Navigate to the web dapp: https://xrpl-mm-v1-web.pages.dev/
5. Click **SHARE** to share your address with the demo website.


### Deposit XRP

1. Click **Deposit** in the _Ripple_ box.
2. Enter an amount smaller than your current balance.
3. Click **Trigger Deposit on XRPL**.
4. Click **SUBMIT** in the GemWallet popup. You will see `transaction accepted` after a few seconds.
5. The modal on the web dapp will confirm a successful transaction and provide a link to it on the XRPL Explorer.
6. Wait a few minutes for the transaction to replay to the `ETH` test network.
7. The modal will confirm when the relay completes and provide a link to the transaction.
8. Click **Execute the transaction on EVM**.
9. The **My Deposit** field will update with the amount you deposited.


### Deposit ETH

1. Fund your XRP wallet and configure it to testnet.
2. Prepare an Ethereum Sepolia account with test ETH. There are several faucets you can use, such as: Chainlink, Infura, or Alchemy.
3. Open the GemWallet extension and click **ADD  TRUSTLINE**. Fill in the following fields and submit the transaction.

    - issuer:  rfEf91bLxrTVC76vw1W3Ur8Jk4Lwujskmb
    - token: ETH
    - limit: 10000000000

4. Navigate to: https://sepolia.etherscan.io/token/0x7b79995e5f793a07bc00c21412e50ecae098e7f9#writeContract.
5. Click **Connect to web 3** and select the wallet with your test ETH from Sepolia.
6. Click **2. deposit (0xd0e30db0)** and enter how much ETH you want to convert to WETH on testnet.
7. Click **write** and submit the transaction.
8. Click **1. approve (0x095ea7b3)**. Enter these values and submit the transaction.
9. This step currently requires CLI usage:
    
    ```sh
    AXELAR_GATEWAY=0xAABdd46ba1B3147d0Cf6aCc9605a74fE8668fC74 
    XRPL_DESTINATION= # your XRPL recipient address 
    PRIVATE_KEY= # your private key 
    cast send $AXELAR_GATEWAY "sendToken(string destinationChain, string 
    destinationAddress, string symbol, uint256 amount)" "xrpl" $XRPL_DESTINATION 
    "WETH" $(cast to-wei 0.1) --private-key $PRIVATE_KEY --rpc-url 
    https://rpc.sepolia.org 
    ```

10. After the transaction goes through, your XRPL account will have an increase in `WETH`.


### Borrow and Repay Assets

1. Make sure you have enough to deposit as collateral. For this demo, the collateral and borrow factor of XRP are configured as 70% and 91% respectively; WETH is 80% and 100%.

    **Example**: A `WETH` deposit of $1000 can back $728 (1000×0.8×0.91) worth of XRP borrowing.

2. Click on **Borrow** in either the `Wrapped Ethereum` or `Ripple` borrowing boxes.
3. Click **Trigger Borrow on XRPL**. Follow the prompts to submit the transaction.
4. The _Wallet Balance_ field should update by the amount you borrowed.
5. Click **Repay**.
6. Enter a value up to the amount you borrowed.
7. Click **Trigger Repay on XRPL**. Follow the prompts to submit the transaction.


### Withdraw Assets

1. Click **Withdraw**.
2. Enter an amount that is less than the amount you deposited initially.
3. Click **Trigger Withdraw on XRPL**. Follow the prompts to submit the transaction.
4. The _Wallet Balance_ field should update by the amount you withdrew.

### Manually Update Oracle

In the production version of this web dapp, oracles will automatically update the price of `WETH` and `XRP`. For the moment, you can manually update their prices.
