# Using **XRP as a Wrapped ERC‑20** on XRPL EVM

> **Audience:** First‑time XRPL EVM builders who need an ERC‑20 representation of XRP for AMMs, vaults, and DeFi flows.
>
> **Key fact:** Native XRP is automatically exposed **as a wrapped ERC‑20** at the sentinel address `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` – no custom wXRP contract required.

---

## 1 / Why this matters

| Typical Ethereum flow    | XRPL EVM flow                |
| ------------------------ | ---------------------------- |
| ETH → wrap → WETH → pool | **XRP (pre‑wrapped) → pool** |

Because XRPL EVM embeds the wrapping logic in the node, you skip the `wrap/unwrap` calls, saving gas and preventing fragmented liquidity.

---

## 2 / Sentinel address & decimals

* **ERC‑20 address (hard‑coded):** `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`
* **Decimals:** `18` (mirrors the common ERC‑20 standard)
* **Total supply:** Mirrors the XRPL ledger in real‑time.

> 📝 **Tip:** Hard‑code the address; there’s no bytecode to query and no proxy to upgrade.

---

## 3 / Quick‑start code (TypeScript + ethers v6)

```ts
import { ethers } from "ethers";

// 1) Connect to Devnet or Mainnet
const provider = new ethers.JsonRpcProvider("https://rpc.devnet.xrpl.org");
const signer   = provider.getSigner();

// 2) Declare the wrapped‑XRP contract
export const XRP_WRAPPED = "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE";
export const erc20Abi = [
  "function transfer(address to, uint256 amount) external",
  "function mint(address to, uint256 amount) external",
  "function burn(uint256 amount) external",
  "function burnFrom(address account, uint256 amount) external",
  "function approve(address spender, uint256 amount) external returns (bool)",
  "function balanceOf(address account) external view returns (uint256)",
  "function increaseAllowance(address spender, uint256 addedValue) external returns (bool)",
  "function allowance(address owner, address spender) external view returns (uint256)"
];
const xrp = new ethers.Contract(XRP_WRAPPED, erc20Abi, signer);

// 3) Send 5 XRP
await xrp.transfer("0xRecipient...", ethers.parseUnits("5", 18));

// 4) Deposit into an AMM (example)
await xrp.approve(ammAddress, ethers.parseUnits("250", 18));
await amm.addLiquidity(XRP_WRAPPED, otherToken, ethers.parseUnits("250", 18), minLp);
```

> **Remember:** Users still need a small XRP balance in their account to pay for gas, even though your contract interactions use the wrapped ERC‑20.

---

## 4 / Integration checklist

* [ ] Replace any custom **wXRP** addresses with `XRP_WRAPPED`.
* [ ] Delete `wrap()` / `unwrap()` helper functions.
* [ ] Update subgraphs & indexers to track the sentinel address.
* [ ] Re‑deploy pools or vaults that expected 6‑decimal tokens.
* [ ] Smoke‑test on **Devnet** → then ship to **Mainnet**.

---

## 5 / FAQ

**Q:** *Can I deploy my own wXRP contract?*
**A:** You can, but you’ll split liquidity and confuse wallets. Stick to the built‑in wrapped ERC‑20.

**Q:** *Is **`decimals()`** 18?*
**A:** Yes — XRPL EVM represents XRP with 18 decimals so math libraries and UIs designed for ETH‐style tokens work seamlessly.

**Q:** *What happens if I accidentally send XRP to another address?*
**A:** Standard ERC‑20 rules apply; there is no unwrap contract to rescue funds, so double‑check addresses.

---

## 6 / Need help?

* **Discord:** [https://discord.gg/xrplevm](https://discord.gg/xrplevm)
* **Docs:** [https://docs.xrplevm.org/]([https://xrpl.org/evm](https://docs.xrplevm.org/))

Happy building! 🚀
