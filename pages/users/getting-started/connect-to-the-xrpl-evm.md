# Connect MetaMask to the XRPL EVM

This guide will walk you through configuring MetaMask to connect to the XRPL EVM.

## Adding XRPL EVM to MetaMask

To interact with the XRPL EVM, you need to manually add it as a custom network in MetaMask.

1. **Open MetaMask**:

   - Click the MetaMask icon in your browser to open the wallet.

2. **Access Network Settings**:

   - In the MetaMask interface, click the network dropdown at the top (default is "Ethereum Mainnet").
   - Select **"Add Network."**

   ![Add Network to MetaMask](../images/addNetwork.png)

3. **Enter Network Details**:

   - Fill in the following information:
     - **Network Name:** XRPL EVM Devnet
     - **New RPC URL:** [https://rpc.xrplevm.org/](https://rpc.xrplevm.org/)
     - **Chain ID:** 1440002
     - **Currency Symbol:** XRP
     - **Block Explorer URL:** [https://explorer.xrplevm.org](https://explorer.xrplevm.org)

   ![Add Network MetaMask Form](../images/addNetworkForm.png)

4. **Save Network**:

   - Click **"Save."** The XRPL EVM will now be available in the network dropdown.

5. **Switch Networks**:

   - Select "XRPL EVM Devnet" from the network dropdown to start interacting with the network.

   ![Select XRPL EVM Network](../images/selectXRPLEVM.png)

---

## Verify the Connection

To ensure that MetaMask is properly connected to the XRPL EVM:

1. Open MetaMask and confirm that "XRPL EVM Devnet" is displayed as the active network.
2. Click **"Account Details"** to view your wallet address.
3. Verify that the balance and token details appear correctly (if you have already received test tokens).

If you encounter any issues, ensure that the RPC URL, Chain ID, and other network details are entered correctly. For further assistance, refer to the XRPL EVM documentation or [support channels](https://discord.gg/xrplevm).
