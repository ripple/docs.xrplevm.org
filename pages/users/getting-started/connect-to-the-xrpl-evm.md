# Connect MetaMask to the XRPL EVM

This guide will walk you through configuring MetaMask to connect to the XRPL EVM.

## Adding XRPL EVM to MetaMask

To interact with the XRPL EVM, you need to manually add it as a custom network in MetaMask.

1. **Open MetaMask:**

   - Click the MetaMask icon in your browser to open the wallet.

2. **Access Network Settings:**

   - In the MetaMask interface, click the network dropdown at the top (default is "Ethereum Mainnet").
   - Select **"Add Network."**

   ![Add Network to MetaMask](../images/addNetwork.png)

{% tabs %}
{% tab label="Mainnet" %}

3. **Enter Network Details:**

   - Fill in the following information:
     - **Network Name:** XRPL EVM
     - **New RPC URL:** [https://rpc.xrplevm.org/](https://rpc.xrplevm.org/)
     - **Chain ID:** 1440000
     - **Currency Symbol:** XRP
     - **Block Explorer URL:** [https://explorer.xrplevm.org](https://explorer.xrplevm.org)

   ![Add Network MetaMask Form](../images/AddXRPLEVMMainnetToMetaMask.png)

4. **Save Network:**

   - Click **"Save."** The XRPL EVM will now be available in the network dropdown.

5. **Switch Networks:**

   - Select **"XRPL EVM"** from the network dropdown to start interacting with the network.

   ![Select XRPL EVM Network](../images/AddedXRPLEVMMainnetToMetaMask.png)
{% /tab %}
{% tab label="Testnet" %}

3. **Enter Network Details:**

   - Fill in the following information:
     - **Network Name:** XRPL EVM Testnet
     - **New RPC URL:** [https://rpc.testnet.xrplevm.org/](https://rpc.testnet.xrplevm.org/)
     - **Chain ID:** 1449000
     - **Currency Symbol:** XRP
     - **Block Explorer URL:** [https://explorer.testnet.xrplevm.org](https://explorer.testnet.xrplevm.org)

   ![Add Network MetaMask Form](../images/addXRPLEVMTestnetToMetaMask.png)

4. **Save Network:**

   - Click **"Save."** The XRPL EVM Testnet will now be available in the network dropdown.

5. **Switch Networks:**

   - Select **"XRPL EVM Testnet"** from the network dropdown to start interacting with the network.

   ![Select XRPL EVM Network](../images/AddedXRPLEVMTestnetToMetaMask.png)
{% /tab %}
{% /tabs %}

## Verify the Connection

To ensure that MetaMask is properly connected to the XRPL EVM:

1. Open MetaMask and confirm that the selected network is displayed as the active network.
2. Click **"Account Details"** to view your wallet address.
3. Verify that the balance and token details appear correctly (if you have already received test tokens).

If you encounter any issues, ensure that the RPC URL, Chain ID, and other network details are entered correctly. For further assistance, refer to the XRPL EVM documentation or visit our [support channels](https://discord.gg/xrplevm).
