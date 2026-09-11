# Integrate the Squid Widget

The **Squid Widget** allows your users to seamlessly swap from tokens on any chain (Ethereum, Arbitrum, Polygon, etc.) directly into **XRP on XRPL EVM**, all through Axelar's secure cross-chain infrastructure.

## Why Use the Squid Widget?

- Frictionless onboarding for users with assets on other chains
- No need to manually bridge or handle gas tokens
- Embed and configure in just a few lines of code

## Before You Start

The widget requires an **Integrator ID**, which Squid issues per project. Request one through the [Squid integrator form](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/getting-started) before integrating.

## Integration Guide

1. **Install the widget package**:

```bash
npm install @0xsquid/widget
```

2. **Render the widget** in your React application:

```jsx
import { SquidWidget } from "@0xsquid/widget";

function App() {
  return (
    <SquidWidget
      config={{
        integratorId: "<your-integrator-id>",
        apiUrl: "https://apiplus.squidrouter.com",
        initialAssets: {
          from: {
            address: "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
            chainId: "1",
          },
          to: {
            address: "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
            chainId: "1440000",
          },
        },
      }}
    />
  );
}
```

`0xEeee...EEeE` is Squid's sentinel address for a chain's native token, so the example above starts on **ETH on Ethereum** (chain `1`) and lands on **XRP on XRPL EVM** (chain `1440000`).

{% admonition type="info" name="Framework setup" %}
Next.js and Vite need extra transpile/optimize configuration for the widget package. Follow the [Next.js](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/nextjs-installation) or [Vite](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/vite-installation) installation guides.
{% /admonition %}

## No-Code Alternative

If you do not need the widget embedded in your own app, Squid hosts a ready-made interface with XRPL EVM preselected:

- **Mainnet**: [XRPL to XRPL EVM on app.squidrouter.com](https://app.squidrouter.com/?fromChain=xrpl&toChain=xrpl+evm&token=xrp)
- **Testnet**: [testnet.xrpl.squidrouter.com](https://testnet.xrpl.squidrouter.com/)

## Resources

- [Installing the Widget](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/installing-the-widget)
- [React Installation](https://docs.squidrouter.com/widget-integration/add-a-widget/widget/react-installation)

---
