# Next Steps

Congratulations on reaching this point in your journey with the **XRPL Ethereum Virtual Machine (EVM)**! You’ve explored the essential building blocks of smart contract development and deployment, and now it's time to consolidate your knowledge and dive deeper into the exciting possibilities of dApp development.

---

## Review What You've Learned

Through the previous pages, you’ve learned to:

1. **Develop a Smart Contract**:

   - Understand the fundamentals of Solidity programming and write Ethereum-compatible smart contracts optimized for the XRPL EVM.  
     _(Read more: [Develop a Smart Contract](./develop-a-smart-contract.md))_

2. **Deploy a Smart Contract**:

   - Use tools like Remix IDE and Hardhat to deploy your contracts on the XRPL EVM Sidechain.  
     _(Read more: [Deploy a Smart Contract](./deploy-the-smart-contract.md))_

3. **Verify a Smart Contract**:

   - Ensure transparency and build user trust by verifying your contract’s source code on the XRPL EVM Explorer.  
     _(Read more: [Verify a Smart Contract](./verify-the-smart-contract.md))_

4. **Interact with a Smart Contract**:
   - Execute smart contract functions, query data, and integrate contract calls into your applications using libraries like `ethers.js` or `web3.js`.  
     _(Read more: [Interact with a Smart Contract](./interact-with-the-smart-contract.md))_

---

## Explore Advanced Development Topics

Take your skills to the next level by exploring advanced use cases and emerging technologies:

1. **Cross-Chain Applications**:

   - Learn how to utilize [Axelar General Message Passing (GMP)](../../bridge/general-message-passing.md) and [Cosmos IBC](../interacting-with-cosmos/using-ibc.md) to create interoperable dApps that interact across multiple blockchains.

2. **Token Standards**:

   - Dive into token standards like [ERC-20](https://docs.openzeppelin.com/contracts/4.x/erc20) (fungible tokens) and [ERC-721](https://docs.openzeppelin.com/contracts/4.x/erc721) (non-fungible tokens) to design and implement custom tokens on the XRPL EVM.

3. **DeFi Protocols**:

   - Explore decentralized finance (DeFi) concepts such as lending, staking, and automated market makers (AMMs) to [build](https://soliditylang.org/) innovative financial solutions.

4. **AI-Driven Smart Contracts**:
   - Integrate AI tools to automate [contract testing](https://ethereum.org/en/developers/docs/smart-contracts/testing/), optimize gas usage, and generate dynamic smart contract logic.

---

## Build a Frontend dApp with XRPL EVM

Here’s a quick-start guide for building a **Next.js (App Router) dApp** with the [**new Reown AppKit**](https://docs.reown.com/appkit/next/core/installation), fully configured for both **XRPL EVM Mainnet** and **Testnet** (plus social/email login, analytics, and more). You’ll be up and running in minutes—no manual chain definitions required, just grab the XRPL EVM networks straight from AppKit’s built-in list.

---

## 1. Installation

```bash
# npm
npm install @reown/appkit @reown/appkit-adapter-wagmi wagmi viem @tanstack/react-query

# or Yarn
yarn add @reown/appkit @reown/appkit-adapter-wagmi wagmi viem @tanstack/react-query
```

---

## 2. (Optional) Scaffold with CLI

```bash
npx @reown/appkit-cli
```

Answer prompts for project name, framework (Next.js), and library (Wagmi/EVM). Then replace the dummy `projectId` in `.env.local` with your real one from Reown Cloud.

---

## 3. Cloud Setup

1. Visit [https://cloud.reown.com](https://cloud.reown.com)
2. Create a project → copy its **Project ID**.
3. In your repo root, add `.env.local`:

   ```env
   NEXT_PUBLIC_REOWN_PROJECT_ID=<YOUR_PROJECT_ID>
   ```

---

## 4. Configure Reown & Wagmi Adapter

Create `src/config/index.tsx`:

```ts
import { cookieStorage, createStorage } from "@wagmi/core";
import { WagmiAdapter }              from "@reown/appkit-adapter-wagmi";
import { xrplEvmTestnet, xrplEvm } from "@reown/appkit/networks";

export const projectId = process.env.NEXT_PUBLIC_REOWN_PROJECT_ID!;
export const networks  = {
  testnet:  [xrplEvmTestnet],
  mainnet:  [xrplEvm],
};

export const wagmiAdapter = new WagmiAdapter({
  storage:  createStorage({ storage: cookieStorage }),
  ssr:      true,
  projectId,
  networks: networks.testnet,   // swap to networks.mainnet for Mainnet
});

export const wagmiConfig = wagmiAdapter.wagmiConfig;
```

---

## 5. AppKit Context Provider

Create `src/context/AppKitProvider.tsx`:

```tsx
'use client';

import React, { ReactNode }               from "react";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { createAppKit }                     from "@reown/appkit/react";
import { cookieToInitialState, WagmiProvider, type Config } from "wagmi";
import { wagmiAdapter, projectId, networks } from "@/config";

const queryClient = new QueryClient();

const metadata = {
  name:        "my-xrpl-app",
  description: "An XRPL EVM dApp",
  url:         "https://myxrplapp.com",
  icons:       ["https://myxrplapp.com/favicon.ico"],
};

export const appKitModal = createAppKit({
  adapters:       [wagmiAdapter],
  projectId,
  networks:       networks.testnet,       // or networks.mainnet
  defaultNetwork: networks.testnet[0],
  metadata,
  features: {
    analytics:       true,
    email:           true,
    socials:         ["google", "github", "discord", "apple"],
    emailShowWallets:true,
  },
});

export function AppKitProvider({ children, cookies }: { children: ReactNode; cookies: string | null }) {
  const initialState = cookieToInitialState(wagmiConfig as Config, cookies);
  return (
    <WagmiProvider config={wagmiConfig as Config} initialState={initialState}>
      <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
    </WagmiProvider>
  );
}
```

---

## 6. Wrap Your Root Layout

In `app/layout.tsx`:

```tsx
import './globals.css';
import { headers }                 from "next/headers";
import { AppKitProvider }          from "@/context/AppKitProvider";

export default async function RootLayout({ children }: { children: React.ReactNode }) {
  const cookieHeader = await headers().get("cookie");
  return (
    <html lang="en">
      <body>
        <AppKitProvider cookies={cookieHeader}>
          {children}
        </AppKitProvider>
      </body>
    </html>
  );
}
```

---

## 7. Trigger the Wallet Modal

Anywhere in your Client Components:

```tsx
export function ConnectButton() {
  return <appkit-button />;
}
```

This web component launches AppKit’s unified wallet/social/email modal.

---

## 8. Use Wagmi Hooks

```tsx
import { useAccount, useSignMessage, useSendTransaction } from "wagmi";

export function Demo() {
  const { address } = useAccount();
  const signMessage    = useSignMessage();
  const sendTransaction = useSendTransaction();

  return (
    <div>
      <p>Connected: {address}</p>
      <button onClick={() => signMessage.signMessage({ message: "Hello, XRPL EVM!" })}>
        Sign Message
      </button>
      <button
        onClick={() =>
          sendTransaction.sendTransaction({
            to:    "0x1234…",
            value: BigInt(1e18),
          })
        }
      >
        Send 1 XRP
      </button>
    </div>
  );
}
```

---

## 9. Switching Networks

To switch between Testnet & Mainnet:

1. In `src/config/index.tsx`, change:

   ```ts
   networks: networks.mainnet,
   defaultNetwork: networks.mainnet[0],
   ```
2. Restart your server (`npm run dev`).

---

## 10. Run & Deploy

```bash
npm run dev    # for localhost; browse http://localhost:3000
npm run build  # production build
```

Deploy on **Vercel**, **Netlify**, or any platform that supports Next.js.

---

You’re all set—Reown AppKit’s unified API plus XRPL EVM out of the box! Enjoy building your dApp.

