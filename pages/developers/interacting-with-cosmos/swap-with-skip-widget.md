# Integrate the Skip Widget

The **Skip Widget** enables token swaps from Cosmos chains (like Osmosis, Injective, Neutron) into **XRP on XRPL EVM**, using IBC and Skip Protocol's advanced routing.

## Why Use the Skip Widget?

* Tap into Cosmos-native liquidity
* Supports swaps via IBC directly into XRPL EVM
* Simple UI for fast user onboarding

## Integration Steps

1. **Install the widget**:

```bash
npm install @skip-router/widget-react
```

2. **Embed in your React App**:

```tsx
import { SwapWidget } from '@skip-router/widget-react';

function App() {
  return (
    <SwapWidget
      defaultSourceChainId="osmosis-1"
      defaultDestinationChainId="xrpl-evm-1"
      defaultDestinationAssetDenom="uxrp"
    />
  );
}
```

3. **Resources**:

* Full docs: [Skip Widget Docs](https://docs.skip.build/go/widget/getting-started)

---
