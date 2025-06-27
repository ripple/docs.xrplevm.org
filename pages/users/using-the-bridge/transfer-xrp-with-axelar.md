# Transfer XRP with Axelar

The Axelar Bridge connects multiple blockchain networks, enabling secure asset transfers across the XRPL ecosystem and beyond. This guide explains how the bridge works and highlights the key differences between **Mainnet**, and **Testnet** setups.

## Understanding the Axelar Bridge

- **Cross-Chain Transfers:** Seamlessly move assets (e.g., XRP, IOUs, ERC20 tokens) between XRPL, XRPL EVM, and other connected chains.  
- **Interoperability:** Unlock new possibilities for dApps, services, and users by allowing access to assets and functionalities across different blockchains.

---

{% tabs %}

   {% tab label="Mainnet" %}

   ## Bridging on Mainnet

   ### 1. Install MetaMask
   Download and [install MetaMask](../getting-started/install-metamask.md) for your browser.

   ### 2. Install an XRPL Wallet
   Choose one of the following XRPL wallets:

   - **XRPL Snap**  
      - Install the [XRPL Snap](https://snap.xrplevm.org) for seamless integration with MetaMask.  
      - Follow the [Quick Start Guide](https://snap-docs.xrplevm.org/getting-started/quick-start) for setup instructions.
   
   - **Crossmark**  
      - Use the [Crossmark](https://crossmark.io) wallet for a browser-first, non-custodial XRPL experience.

   ### 3. Using the Squid Bridge

   1. **Connect Your Source Chain Wallet**  
      - Click **Connect**.  
      - Select your XRPL wallet (either the XRPL Snap or Crossmark).  
      - Approve the connection request to allow **SquidRouter** to interact with your XRPL wallet.  
      - Once connected, you’ll see your XRPL address (e.g., `rfLm...bYVY`).

      ![UsingTheBridge](../images/ConnectXRPLWallet1.png)

   2. **Select the Amount to Swap**  
      - Enter the amount of XRP you want to bridge.  
      - Ensure you have sufficient XRP to cover transaction fees.

      ![UsingTheBridge](../images/AmountXRPLWallet2.png)

   3. **Set the Receiver**  
      - Click **Add recipient** to specify your XRPL EVM address.  
      - Connect your XRPL EVM account using any WalletConnect-compatible wallet **or** paste the XRPL EVM receiver address directly.

      ![UsingTheBridge](../images/SetRecepientXRPLWallet1.png)

   4. **Initiate the Swap**  
      - Click **SWAP** to begin the cross-chain transfer.

      ![UsingTheBridge](../images/SwapXRPLWallet.png)
      - Review and **Approve** the transaction in your XRPL wallet.

      ![UsingTheBridge](../images/SwapApproveXRPLWallet.png)

   5. **View Your Assets on the XRPL EVM**  
      - Add the [XRPL EVM](../getting-started/connect-to-the-xrpl-evm.md#adding-xrpl-evm-to-metamask) network to MetaMask if you haven’t already.  
      - Once the transaction completes, your XRP (or other assets) will be visible in your XRPL EVM wallet.

   {% /tab %}

   {% tab label="Testnet" %}

   ## Bridging on Testnet

   ### 1. Install MetaMask
   Download and [install MetaMask](../getting-started/install-metamask.md) for your browser.

   ### 2. Install an XRPL Wallet
   Choose one of the following XRPL wallets:

   - **XRPL Snap**  
      - Install the [XRPL Snap](https://snap.xrplevm.org) for seamless integration with MetaMask.  
      - Follow the [Quick Start Guide](https://snap-docs.xrplevm.org/getting-started/quick-start) for setup instructions.
   
   - **Crossmark**  
      - Use the [Crossmark](https://crossmark.io) wallet for a browser-first, non-custodial XRPL experience.

   ### 3. Use the Testnet Bridge

   1. **Connect Your Source Chain Wallet**  
      - Click **Connect**.  
      - Select your XRPL wallet (either the XRPL Snap or Crossmark).  
      - Approve the connection request to allow **SquidRouter** to interact with your XRPL wallet.  
      - Once connected, you’ll see your XRPL address (e.g., `rfLm...bYVY`).

      ![UsingTheBridge](../images/ConnectXRPLWallet1.png)

   2. **Select the Amount to Swap**  
      - Enter the amount of XRP you want to bridge.  
      - Ensure you have sufficient testnet XRP to cover transaction fees.

      ![UsingTheBridge](../images/AmountXRPLWallet2.png)

   3. **Set the Receiver**  
      - Click **Add recipient** to specify your XRPL EVM Testnet address.  
      - Connect your XRPL EVM account using any WalletConnect-compatible wallet **or** paste the XRPL EVM receiver address directly.

      ![UsingTheBridge](../images/SetRecepientXRPLWallet1.png)

   4. **Initiate the Swap**  
      - Click **SWAP** to begin the cross-chain transfer.

      ![UsingTheBridge](../images/SwapXRPLWallet.png)
      - Review and **Approve** the transaction in your XRPL wallet.

      ![UsingTheBridge](../images/SwapApproveXRPLWallet.png)

   5. **View Your Assets on the XRPL EVM Testnet**  
      - Add the [XRPL EVM Testnet network](../getting-started/connect-to-the-xrpl-evm.md#adding-xrpl-evm-to-metamask) to MetaMask if you haven’t already.  
      - Once the transaction completes, your XRP (or other assets) will be visible in your XRPL EVM Testnet wallet.

   {% /tab %}
{% /tabs %}

---

## Summary

By following these steps, you can securely swap assets across XRPL and XRPL EVM, and other Axelar-connected chains. Whether you’re a developer testing dApps or a user exploring cross-chain interoperability, the Axelar Bridge streamlines asset transfers and unlocks new possibilities across the XRPL ecosystem.