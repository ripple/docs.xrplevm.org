# Integrate the Skip Widget

The Skip Widget (Skip Go) swaps tokens from Cosmos chains into XRP on XRPL EVM, using IBC and Skip's routing. Routes into XRPL EVM arrive over the active Osmosis IBC channel, so assets from other Cosmos chains are routed through Osmosis.

## Why Use the Skip Widget?

- Tap into Cosmos-native liquidity
- Supports swaps via IBC directly into XRPL EVM
- Simple UI for fast user onboarding

## Integration Steps

1. **Install the widget** and its peer dependencies:

```bash
npm install @skip-go/widget @tanstack/react-query wagmi viem
```

2. **Embed in your React App**:

```tsx
import { Widget } from "@skip-go/widget";

function App() {
  return (
    <Widget
      defaultRoute={{
        srcChainId: "osmosis-1",
        srcAssetDenom: "uosmo",
        destChainId: "xrplevm_1440000-1",
        destAssetDenom: "axrp",
      }}
    />
  );
}
```

On Skip, XRPL EVM Mainnet is identified by its Cosmos chain ID `xrplevm_1440000-1`, and native XRP by the denom `axrp` (18 decimals).

3. **Resources**:

- Full docs: [Skip Go Widget Docs](https://docs.skip.build/go/widget/getting-started)
- Hosted app: [go.skip.build](https://go.skip.build/)

---
