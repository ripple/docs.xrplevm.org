# Faucet

To explore and interact with the **XRPL EVM sidechain**, you need test XRP (`XRP`) to cover transaction fees and execute smart contracts. This guide explains how to obtain test XRP for both the **Testnet** and **Devnet** environments.

{% tabs %}
{% tab label="Testnet" %}

## Testnet Faucets

1. **[XRPL EVM Faucet](https://faucet.xrplevm.org)**
   - [This](https://faucet.xrplevm.org) website supports both **Testnet** and **Devnet** .
   - Paste your **0x** (EVM) address or connect MetaMask.
   - If you don’t have [MetaMask installed](./getting-started/install-metamask.md) or the Testnet network configured, the faucet will guide you.
   - You can claim up to **90 XRP** per request.

2. **[Enigma Validator’s Faucet](https://enigma-validator.com/)**
   - Join the XRPLEVM **[Discord channel](https://discord.com/channels/1143643230492168274/1352703976050528327)** and navigate to **#faucet**.
   - Send the command:
     ```
     !faucet <YOUR_ADDRESS>
     ```
   - This faucet supports **0x... (EVM)** and **ethm... Cosmos** addresses, providing **50 XRP** per request.

3. **Bridging via [SquidRouter](https://testnet.xrpl.squidrouter.com/)**
   - If you already have some **XRPL Testnet** [funds](https://xrpl.org/resources/dev-tools/xrp-faucets), you can bridge assets from XRPL Testnet to **XRPL EVM Testnet** using the SquidRouter interface.
   - See [Using the Bridge](./using-the-bridge.md) for more detailed instructions.

### Fund Your XRPL Testnet Account
Before bridging from XRPL Testnet to XRPL EVM Testnet, you **must** fund an XRPL Testnet account with test XRP. One convenient option is:

- **[XRPL MetaMask Snap](https://snap.xrplevm.org)**  
  - Install the Snap in MetaMask.  
  - Use its built-in faucet or XRPL faucets it supports to fund your XRPL Testnet account.

Once your XRPL Testnet account is funded, follow the [bridge instructions](./using-the-bridge.md) or use [SquidRouter](https://testnet.xrpl.squidrouter.com/) to move assets to XRPL EVM Testnet.

{% /tab %}
{% tab label="Devnet" %}

## Devnet Faucet

- **[XRPL EVM Devnet Faucet @ Chains.tools](https://chains.tools/faucet/xrplevm)**  
  - For **XRPL EVM Devnet**.  
  - Each wallet can request up to **10 XRP** every 60 minutes.  
  - Alternatively, bridge from the XRPL Devnet using the [bridge guide](./using-the-bridge.md) or by manual interchain transactions.

## Bridging on Devnet (Axelar Bridge Overview)

1. **Open the Axelar Bridge**  
   - [https://bridge.devnet.xrplevm.org/](https://bridge.devnet.xrplevm.org)

2. **Set Source & Destination**  
   - **Source:** XRPL Devnet  
   - **Destination:** XRPL EVM Devnet

3. **Fund an XRPL Devnet Account**  
   - You can automatically generate and fund a Devnet account with 100 XRP through the Axelar faucet option.

4. **Connect Your Wallets**  
   - **XRPL Devnet:** No manual signature needed for outgoing transactions.  
   - **XRPL EVM Devnet:** Connect via MetaMask (ensure the XRPL EVM Devnet network is configured).

5. **Select Token & Amount**  
   - Choose **XRP** or another supported Devnet token.

6. **Confirm & Transfer**  
   - **XRPL → EVM:** No signature required on XRPL side.  
   - **EVM → XRPL:** You’ll sign in MetaMask.  
   - Track status on the [Axelar Devnet Amplifier Explorer](https://devnet-amplifier.axelarscan.io/gmp/search).

7. **Verify Balance**  
   - Check the [XRPL EVM Devnet Explorer](https://explorer.devnet.xrplevm.org) once the transfer completes.

{% /tab %}
{% /tabs %}

---

## Manual Bridging with XRPL Payment Memos

For advanced use cases or custom dApp integrations, you can trigger an interchain transfer by sending a **Payment transaction** on XRPL with specific memos (via axelar [**gpm**](https://docs.axelar.dev/dev/general-message-passing/overview/)). Detailed instructions on crafting these memos and handling cross-chain requests can be found in the [Send XRP, IOUs and ERC20s Guide](../developers/making-a-cross-chain-dapp/send-tokens.md) and [Send Cross-Chain Messages guide](../developers/making-a-cross-chain-dapp/send-messages.md).