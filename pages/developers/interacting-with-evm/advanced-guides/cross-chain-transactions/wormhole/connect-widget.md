# Wormhole Connect Widget

**Wormhole Connect** is a drop-in React widget that gives your dApp a complete bridging UI: chain and token selection, wallet connections, quoting, and transfer tracking. On the XRPL EVM, Connect is available on **Mainnet** (Testnet is not currently supported).

## Install

```bash
npm install @wormhole-foundation/wormhole-connect
```

## Add the Widget

Configure Connect with the chains you want to expose. The XRPL EVM's chain name is `XRPLEVM`:

```tsx
import WormholeConnect, {
  type config,
  WormholeConnectTheme,
} from "@wormhole-foundation/wormhole-connect";

function App() {
  const config: config.WormholeConnectConfig = {
    network: "Mainnet",
    chains: ["XRPLEVM", "Ethereum", "Solana"],
    ui: {
      title: "XRPL EVM Bridge",
    },
  };

  const theme: WormholeConnectTheme = {
    mode: "dark",
    primary: "#32dbc7",
  };

  return <WormholeConnect config={config} theme={theme} />;
}

export default App;
```

{% admonition type="info" name="Verify chain support in your installed version" %}
Chain availability in Connect follows the [Wormhole SDK chain names](https://wormhole.com/docs/products/reference/chain-ids/) and the routes enabled for each network. If the widget rejects the `XRPLEVM` chain string, upgrade `@wormhole-foundation/wormhole-connect` to the latest release and check the [Connect supported networks](https://wormhole.com/docs/products/connect/reference/supported-networks/) table.
{% /admonition %}

## Customize

- **Custom RPCs**: Point Connect at your own XRPL EVM RPC (for example `https://rpc.xrplevm.org`) via the [data configuration](https://wormhole.com/docs/products/connect/configuration/data/).
- **Theming**: Adjust colors, fonts, and layout with the [theme configuration](https://wormhole.com/docs/products/connect/configuration/theme/).
- **Routes and tokens**: Restrict which tokens and routes appear, including NTT tokens you have deployed.

## Next Steps

- [Connect overview](https://wormhole.com/docs/products/connect/overview/)
- [Connect get-started guide](https://wormhole.com/docs/products/connect/get-started/)
- [Portal Bridge](https://portalbridge.com): the hosted app built on the same stack.
