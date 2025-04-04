# Using the Bridge

The Axelar Bridge connects multiple blockchain networks, enabling secure asset transfers across the XRPL ecosystem and beyond. This guide explains how the bridge works and highlights the key differences between **Testnet** and **Devnet** setups.

## Understanding the Axelar Bridge

- **Cross-Chain Transfers:** Seamlessly move assets (e.g., XRP, IOUs, ERC20 tokens) between XRPL, XRPL EVM, and other connected chains.  
- **Interoperability:** Unlock new possibilities for dApps, services, and users by allowing access to assets and functionalities across different blockchains.

## Testnet vs. Devnet

- **Testnet Bridge (SquidRouter)**  
  - **URL**: [https://testnet.xrpl.squidrouter.com/](https://testnet.xrpl.squidrouter.com/)  
  - **Features**: Connects all Axelar testnet chains.  
  - **Wallet Requirements**:  
    - **XRPL MetaMask Snap** or **Crossmark.io** for XRPL access.  
    - [Add the XRPL EVM Testnet network](./getting-started/connect-to-the-xrpl-evm.md#adding-xrpl-evm-to-metamask) to MetaMask.  

- **Devnet Bridge**  
  - **URL**: [https://bridge.devnet.xrplevm.org/](https://bridge.devnet.xrplevm.org/)  
  - **Features**:  
    - Simpler interface; acts as a faucet for obtaining test XRP.  
    - Allows bridging between XRPL Devnet and XRPL EVM Devnet.  
  - **Wallet Requirements**:  
    - Automatic XRPL Devnet wallet creation with test XRP.  
    - MetaMask connection set to XRPL EVM Devnet.  

---

{% tabs %}
   {% tab label="Testnet" %}

   ## Bridging on Testnet

   ### 1. Install MetaMask
   Download and [install MetaMask](./getting-started/install-metamask.md) for your browser.

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

      ![UsingTheBridge](./images/ConnectXRPLWallet1.png)

   2. **Select the Amount to Swap**  
      - Enter the amount of XRP you want to bridge.  
      - Ensure you have sufficient testnet XRP to cover transaction fees.

      ![UsingTheBridge](./images/AmountXRPLWallet2.png)

   3. **Set the Receiver**  
      - Click **Add recipient** to specify your XRPL EVM Testnet address.  
      - Connect your XRPL EVM account using any WalletConnect-compatible wallet **or** paste the XRPL EVM receiver address directly.

      ![UsingTheBridge](./images/SetRecepientXRPLWallet1.png)

   4. **Initiate the Swap**  
      - Click **SWAP** to begin the cross-chain transfer.

      ![UsingTheBridge](./images/SwapXRPLWallet.png)
      - Review and **Approve** the transaction in your XRPL wallet.

      ![UsingTheBridge](./images/SwapApproveXRPLWallet.png)

   5. **View Your Assets on the XRPL EVM Testnet**  
      - Add the [XRPL EVM Testnet network](./getting-started/connect-to-the-xrpl-evm.md#adding-xrpl-evm-to-metamask) to MetaMask if you haven’t already.  
      - Once the transaction completes, your XRP (or other assets) will be visible in your XRPL EVM Testnet wallet.

   {% /tab %}
   {% tab label="Devnet" %}

   ## Bridging on Devnet

   ### Overview
   - **Devnet Faucet:** Quickly obtain test XRP on XRPL without using a dedicated XRPL wallet.  
   - **Bidirectional Transfers:** Move assets between XRPL Devnet and XRPL EVM Devnet.  
   - **No Dedicated Signature for XRPL Devnet Outgoing Transactions:** XRPL Devnet accounts are automatically self-funded and do not require manual signing for outgoing transactions.

   ### Steps to Use the Axelar Bridge (Devnet)

   1. **Open the Axelar Bridge**  
      - Go to [https://bridge.devnet.xrplevm.org/](https://bridge.devnet.xrplevm.org/).

   2. **Set XRPL Devnet as the Source Chain**  
      - Select **"XRPL Devnet"** as your source chain.  
      - Select **"XRPL EVM Devnet"** as your destination chain.  
      ![UsingTheBridge](./images/usingTheBridgeAxelar1.png)

   3. **Select or Fund a Devnet Account**  
      - Click the faucet option if you need test XRP. This creates a new XRPL Devnet account funded with 100 XRP.  
      ![UsingTheBridge](./images/usingTheBridgeAxelar2.png)

   4. **Connect Your Wallets**  
      - **XRPL Devnet**: No separate signature is required for outgoing transactions.  
      - **XRPL EVM**: Connect MetaMask with the **XRPL EVM Devnet** network already added.  
      ![UsingTheBridge](./images/usingTheBridgeAxelar3.png)

   5. **Choose a Token and Amount**  
      - Pick XRP, an IOU, or any ERC20 token supported on Devnet.  
      - Enter the amount you want to bridge.  
      ![UsingTheBridge](./images/usingTheBridgeAxelar4.png)

   6. **Transfer and Confirm**  
      - **From XRPL to EVM**: No XRPL signature is needed; transactions are automatically funded.  
      - **From EVM to XRPL**: Sign the transaction using MetaMask.  
      - Track your transaction status in the [Axelar Devnet Amplifier Explorer](https://devnet-amplifier.axelarscan.io/gmp/search).  
      ![UsingTheBridge](./images/usingTheBridgeAxelar5.png)

   7. **Verify the Transaction**  
      - A success message appears once the transaction completes.  
      - Check the [XRPL EVM Devnet Explorer](https://explorer.devnet.xrplevm.org) for additional transaction details.  
      ![UsingTheBridge](./images/usingTheBridgeAxelar6.png)

   {% /tab %}
{% /tabs %}

---

## Summary

By following these steps, you can securely swap assets across XRPL, XRPL EVM, and other Axelar-connected chains in both the **Testnet** and **Devnet** environments. Whether you’re a developer testing dApps or a user exploring cross-chain interoperability, the Axelar Bridge streamlines asset transfers and unlocks new possibilities across the XRPL ecosystem.