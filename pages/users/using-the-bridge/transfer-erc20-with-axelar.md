---
html: transfer-erc20-with-axelar.html
blurb: Transfer ERC-20 tokens between XRPL and the XRPL EVM Sidechain with Axelar.
labels:
  - Interoperability
status: not_enabled
---

# Transfer ERC20 with Axelar

Any ERC20 token on a connected EVM chain can move to the XRPL EVM and back using [Squid Router](https://app.squidrouter.com/), which routes transfers through Axelar. In this example, we will transfer WBTC from Ethereum to the XRPL EVM.

1. **Set** Ethereum as the **source chain**, XRPL EVM as the **destination chain** and WBTC as the **asset** to transfer.
   ![portal set chains](../images/transfererc20-1.png)

2. **Connect** your **wallet**.

   ![portal connect metamask](../images/transfererc20-2.png)

3. **Set** the desired **amount**.

   ![portal transfer](../images/transfererc20-3.png)

4. **Give permission** to use the tokens, sign the approve function.

   ![portal transfer](../images/transfererc20-4.png)

5. Finally **SWAP** to execute the crosschain transfer.

   ![portal transfer](../images/transfererc20-5.png)

Once the transaction is executed, you can see the transaction details in the following modal.

![portal transaction list](../images/transfererc20-6.png)
![portal transaction list](../images/transfererc20-7.png)
