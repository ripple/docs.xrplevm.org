# Integrate the Squid Widget

The **Squid Widget** allows your users to seamlessly swap from tokens on any chain (Ethereum, Arbitrum, Polygon, etc.) directly into **XRP on XRPL EVM**, all through Axelar’s secure cross-chain infrastructure.

## Why Use the Squid Widget?

* Frictionless onboarding for users with assets on other chains
* No need to manually bridge or handle gas tokens
* Embed and configure in just a few lines of code

## Integration Guide

1. **Install the widget script** in your HTML or React app:

```html
<script src="https://widget.squidrouter.com/squid-widget.js"></script>
```

2. **Create the container** where the widget will be rendered:

```html
<div id="squid-widget"></div>
```

3. **Initialize the widget**:

```js
SquidWidget.init({
  target: '#squid-widget',
  config: {
    integratorId: 'your-app-id',
    toChainId: 'XRPL_EVM_CHAIN_ID',
    toToken: 'XRP_CONTRACT_ADDRESS_ON_EVM',
    fromTokenList: ['ETH', 'USDC', 'MATIC'],
    enableRouterPriority: true,
    appearance: 'auto'
  }
});
```

4. **Resources**:

* Full guide: [Squid Widget Docs](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/getting-started)

---
