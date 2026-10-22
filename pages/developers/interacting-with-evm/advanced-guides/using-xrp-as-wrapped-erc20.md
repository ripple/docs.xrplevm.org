# Using **XRP as an ERC-20** on XRPL EVM

> **Audience:** First-time XRPL EVM builders who need an ERC-20 representation of XRP for AMMs, vaults, and DeFi flows.
>
> **Key fact:** Native XRP is automatically exposed **as an ERC-20** at the sentinel address `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`. For most integrations, no wrapped XRP contract is required.
>
> **Limitation:** `0xEee..` is a precompile, and a single transaction can make at most **20 precompile calls** (the node returns `max calls to precompiles (20) reached` beyond that). Long or complex aggregator routes can hit this limit. For those routes, ecosystem DEXs use a third-party WETH9-style wrapper, `WXRP` (contract `WXRP9`), deployed and verified on Mainnet at [`0x7C21a90E3eCD3215d16c3BBe76a491f8f792d4Bf`](https://explorer.xrplevm.org/address/0x7C21a90E3eCD3215d16c3BBe76a491f8f792d4Bf). Reusing that existing wrapper instead of deploying another one avoids fragmenting liquidity.

---

## 1 / Why this matters

| Typical Ethereum flow    | XRPL EVM flow         |
| ------------------------ | --------------------- |
| ETH → wrap → WETH → pool | **XRP ERC-20 → pool** |

Because XRPL EVM embeds the wrapping logic in the node, you skip the `wrap/unwrap` calls, saving gas and preventing fragmented liquidity.

---

## 2 / Sentinel address & decimals

- **ERC-20 address (hard-coded):** `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`
- **Decimals:** `18` (mirrors the common ERC-20 standard)
- **Name / symbol:** `XRP` / `XRP`
- **Total supply:** The total XRP held on the XRPL EVM (the bank supply of the native `axrp` denom), not the XRP supply of the XRP Ledger.

> **Tip:** Hard-code the address. It is a precompile backed by the node, so there is no proxy to upgrade.

---

## 3 / Quick-start code (TypeScript + ethers v6)

```ts
import { ethers } from "ethers";

// 1) Connect to Testnet or Mainnet
const provider = new ethers.JsonRpcProvider("https://rpc.testnet.xrplevm.org");
const signer = new ethers.Wallet(process.env.PRIVATE_KEY!, provider);

// 2) Declare the XRP-ERC20 contract
export const XRP_ERC20 = "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE";
export const erc20Abi = [
  "function transfer(address to, uint256 amount) external returns (bool)",
  "function transferFrom(address from, address to, uint256 amount) external returns (bool)",
  "function approve(address spender, uint256 amount) external returns (bool)",
  "function allowance(address owner, address spender) external view returns (uint256)",
  "function balanceOf(address account) external view returns (uint256)",
  "function totalSupply() external view returns (uint256)",
  "function decimals() external view returns (uint8)",
];
const xrp = new ethers.Contract(XRP_ERC20, erc20Abi, signer);

// 3) Send 5 XRP
await xrp.transfer("0xRecipient...", ethers.parseUnits("5", 18));

// 4) Deposit into an AMM (example)
await xrp.approve(ammAddress, ethers.parseUnits("250", 18));
await amm.addLiquidity(
  XRP_ERC20,
  otherToken,
  ethers.parseUnits("250", 18),
  minLp,
);
```

> **Remember:** Users still need a small XRP balance in their account to pay for gas, even though your contract interactions use the XRP ERC-20.

---

## 4 / Integration checklist

- [ ] Use `XRP_ERC20` by default; keep a wrapper (`WXRP` above) only for routes that exceed the precompile call limit.
- [ ] Delete `wrap()` / `unwrap()` helper functions.
- [ ] Update subgraphs & indexers to track the sentinel address.
- [ ] Re-deploy pools or vaults that expected 6-decimal tokens.
- [ ] Smoke-test on **Testnet** (or Devnet) → then ship to **Mainnet**.

---

## 5 / FAQ

**Q:** _Can I deploy my own wXRP contract?_
**A:** You can, but you'll split liquidity and confuse wallets. Use the built-in ERC-20 by default, and the existing `WXRP` wrapper only where a route exceeds the precompile call limit.

**Q:** _Is **`decimals()`** 18?_
**A:** Yes. XRPL EVM represents XRP with 18 decimals, so math libraries and UIs built for Ethereum tokens work seamlessly.

**Q:** _What happens if I accidentally send XRP to another address?_
**A:** Standard ERC-20 rules apply; there is no unwrap contract to rescue funds, so double-check addresses.

---

## 6 / Need help?

**Discord:** [https://discord.gg/xrplevm](https://discord.gg/xrplevm)

Happy building!
