# Integrating Reown Social Login with XRPL EVM Networks

This guide walks you through integrating Reown AppKit (formerly WalletConnect) social login functionality into your decentralized application for XRPL EVM and XRPL EVM Testnet networks.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Network Configuration](#network-configuration)
- [Installation](#installation)
- [Project Setup](#project-setup)
- [Configuration](#configuration)
- [Implementation](#implementation)
- [Social Login Features](#social-login-features)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before starting, ensure you have:

- Node.js 18+ installed
- A Reown Cloud Project ID (obtain from [cloud.reown.com](https://cloud.reown.com))
- Basic knowledge of React and Next.js
- Understanding of Web3 concepts

---

## Network Configuration

### XRPL EVM Mainnet

| Parameter           | Value                          |
| ------------------- | ------------------------------ |
| **Network Name**    | XRPL EVM                       |
| **RPC URL**         | `https://rpc.xrplevm.org/`     |
| **Chain ID**        | `1440000`                      |
| **Currency Symbol** | `XRP`                          |
| **Block Explorer**  | `https://explorer.xrplevm.org` |

### XRPL EVM Testnet

| Parameter           | Value                                  |
| ------------------- | -------------------------------------- |
| **Network Name**    | XRPL EVM Testnet                       |
| **RPC URL**         | `https://rpc.testnet.xrplevm.org/`     |
| **Chain ID**        | `1449000`                              |
| **Currency Symbol** | `XRP`                                  |
| **Block Explorer**  | `https://explorer.testnet.xrplevm.org` |

---

## Installation

### 1. Create a Next.js Project

```bash
npx create-next-app@latest my-xrpl-dapp --typescript --tailwind --app
cd my-xrpl-dapp
```

### 2. Install Required Dependencies

**Critical:** Use these exact versions for compatibility:

```bash
npm install @reown/appkit@1.7.6 \
  @reown/appkit-adapter-wagmi@1.7.6 \
  @tanstack/react-query@5.81.5 \
  wagmi@2.15.4 \
  viem@2.30.0
```

### 3. Update package.json Overrides

Add these overrides to your `package.json` to prevent dependency conflicts:

```json
{
  "overrides": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "@types/react": "^18.3.1",
    "@types/react-dom": "^18.3.1",
    "@wagmi/core": "2.17.2",
    "@wagmi/connectors": "5.8.3"
  }
}
```

---

## Project Setup

### 1. Environment Variables

Create a `.env.local` file in your project root:

```env
NEXT_PUBLIC_PROJECT_ID=your_reown_project_id_here
```

To obtain your Project ID:

1. Visit [cloud.reown.com](https://cloud.reown.com)
2. Create a new project
3. Copy your Project ID

### 2. Directory Structure

Create the following structure:

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── globals.css
├── components/
│   └── ConnectWallet.tsx
├── config/
│   └── wagmi.ts
└── contexts/
    └── ContextProvider.tsx
```

---

## Configuration

### 1. Configure Wagmi (`src/config/wagmi.ts`)

This file sets up the blockchain networks and Wagmi configuration:

```typescript
import { cookieStorage, createStorage } from "@wagmi/core";
import { WagmiAdapter } from "@reown/appkit-adapter-wagmi";
import { xrplevm, xrplevmTestnet } from "@reown/appkit/networks";

// Get the Project ID from environment variables
export const projectId = process.env.NEXT_PUBLIC_PROJECT_ID;

if (!projectId) {
  throw new Error(
    "Project ID is not defined. Please add NEXT_PUBLIC_PROJECT_ID to your .env.local file",
  );
}

// Define the XRPL EVM networks
// Use both mainnet and testnet, or just testnet for development
export const networks = [xrplevm, xrplevmTestnet];

// Create Wagmi adapter with persistent storage
export const wagmiAdapter = new WagmiAdapter({
  storage: createStorage({
    storage: cookieStorage,
  }),
  ssr: true,
  projectId,
  networks,
});

export const config = wagmiAdapter.wagmiConfig;
```

**Key Points:**

- `cookieStorage` enables persistent sessions across page reloads
- `ssr: true` is required for Next.js App Router
- Import pre-configured networks from `@reown/appkit/networks`

### 2. Create Context Provider (`src/contexts/ContextProvider.tsx`)

This sets up the Reown AppKit modal and providers:

```typescript
'use client'

import { wagmiAdapter, projectId } from '@/config/wagmi'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { createAppKit } from '@reown/appkit/react'
import { xrplevm, xrplevmTestnet } from '@reown/appkit/networks'
import React, { type ReactNode } from 'react'
import { cookieToInitialState, WagmiProvider, type Config } from 'wagmi'

// Set up React Query client
const queryClient = new QueryClient()

if (!projectId) {
  throw new Error('Project ID is not defined')
}

// Set up dApp metadata
const metadata = {
  name: 'My XRPL EVM DApp',
  description: 'A dApp with social login on XRPL EVM',
  url: 'https://my-dapp.com', // Your dApp URL
  icons: ['https://my-dapp.com/icon.png'] // Your dApp icon
}

// Create the AppKit modal
const modal = createAppKit({
  adapters: [wagmiAdapter],
  projectId,
  networks: [xrplevm, xrplevmTestnet],
  defaultNetwork: xrplevmTestnet, // Start with testnet for development
  metadata: metadata,
  features: {
    analytics: true, // Enable analytics (optional)
    email: false, // Disable email login (optional)
    socials: [
      'google',
      'x',
      'github',
      'discord',
      'apple',
      'farcaster',
    ],
    emailShowWallets: true,
  },
  featuredWalletIds: [
    'c57ca95b47569778a828d19178114f4db188b89b763c899ba0be274e97267d96', // MetaMask
  ],
})

export default function ContextProvider({
  children,
  cookies
}: {
  children: ReactNode
  cookies: string | null
}) {
  const initialState = cookieToInitialState(wagmiAdapter.wagmiConfig as Config, cookies)

  return (
    <WagmiProvider config={wagmiAdapter.wagmiConfig as Config} initialState={initialState}>
      <QueryClientProvider client={queryClient}>
        {children}
      </QueryClientProvider>
    </WagmiProvider>
  )
}
```

**Social Login Configuration:**

The `socials` array enables different social login providers:

- `google` - Google OAuth
- `x` - X (formerly Twitter)
- `github` - GitHub
- `discord` - Discord
- `apple` - Apple ID
- `farcaster` - Farcaster

You can customize this list based on your needs.

### 3. Update Root Layout (`src/app/layout.tsx`)

Wrap your app with the ContextProvider:

```typescript
import type { Metadata } from 'next'
import { headers } from 'next/headers'
import ContextProvider from '@/contexts/ContextProvider'
import './globals.css'

export const metadata: Metadata = {
  title: 'My XRPL EVM DApp',
  description: 'A dApp with social login on XRPL EVM',
}

export default async function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  const headersList = await headers()
  const cookies = headersList.get('cookie')

  return (
    <html lang="en">
      <body>
        <ContextProvider cookies={cookies}>
          {children}
        </ContextProvider>
      </body>
    </html>
  )
}
```

---

## Implementation

### Create Connect Wallet Component (`src/components/ConnectWallet.tsx`)

This component handles wallet connection and displays account information:

```typescript
'use client'

import { useAppKit, useAppKitAccount } from '@reown/appkit/react'
import { useDisconnect, useBalance, useChainId } from 'wagmi'
import { xrplevm, xrplevmTestnet } from '@reown/appkit/networks'

export default function ConnectWallet() {
  const { open } = useAppKit()
  const { address, isConnected } = useAppKitAccount()
  const { disconnect } = useDisconnect()
  const chainId = useChainId()

  // Get wallet balance
  const { data: balance, isLoading: balanceLoading } = useBalance({
    address: address as `0x${string}`
  })

  // Check if connected to correct network
  const isCorrectNetwork = chainId === xrplevmTestnet.id || chainId === xrplevm.id
  const networkName = chainId === xrplevm.id
    ? 'XRPL EVM'
    : chainId === xrplevmTestnet.id
    ? 'XRPL EVM Testnet'
    : 'Wrong Network'

  if (!isConnected) {
    return (
      <div className="p-8 bg-gray-800 rounded-xl">
        <div className="text-center space-y-6">
          <h3 className="text-xl font-semibold">Connect to Get Started</h3>
          <p className="text-sm text-gray-400">
            Use social login or connect your wallet
          </p>
          <button
            onClick={() => open()}
            className="px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-lg transition-all"
          >
            Connect Wallet
          </button>
        </div>
      </div>
    )
  }

  return (
    <div className="p-8 bg-gray-800 rounded-xl space-y-4">
      {/* Connection Status */}
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-3">
          <div className="w-3 h-3 bg-green-500 rounded-full animate-pulse" />
          <span className="font-semibold">Connected</span>
        </div>
        <button
          onClick={() => disconnect()}
          className="px-4 py-2 bg-red-500/20 hover:bg-red-500/30 text-red-400 rounded-lg text-sm"
        >
          Disconnect
        </button>
      </div>

      {/* Network Status */}
      <div className={`p-4 rounded-lg border ${
        isCorrectNetwork
          ? 'bg-green-500/10 border-green-500/30'
          : 'bg-red-500/10 border-red-500/30'
      }`}>
        <div className="flex items-center justify-between">
          <span className="text-sm">{networkName}</span>
          {!isCorrectNetwork && (
            <button
              onClick={() => open({ view: 'Networks' })}
              className="px-3 py-1 bg-blue-500/20 text-blue-400 rounded text-xs"
            >
              Switch Network
            </button>
          )}
        </div>
      </div>

      {/* Wallet Address */}
      <div>
        <label className="block text-sm text-gray-400 mb-2">
          Wallet Address
        </label>
        <div className="p-3 bg-gray-700 rounded-lg font-mono text-sm">
          {address}
        </div>
      </div>

      {/* Balance */}
      <div>
        <label className="block text-sm text-gray-400 mb-2">
          Balance
        </label>
        <div className="p-3 bg-gray-700 rounded-lg">
          {balanceLoading ? (
            <span className="text-gray-400">Loading...</span>
          ) : (
            <span className="font-semibold">
              {balance?.formatted} {balance?.symbol}
            </span>
          )}
        </div>
      </div>

      {/* Settings Button */}
      <button
        onClick={() => open({ view: 'Account' })}
        className="w-full px-4 py-3 bg-gray-700 hover:bg-gray-600 rounded-lg transition-colors"
      >
        Account Settings
      </button>
    </div>
  )
}
```

### Use the Component (`src/app/page.tsx`)

```typescript
import ConnectWallet from '@/components/ConnectWallet'

export default function Home() {
  return (
    <main className="min-h-screen bg-gray-900 text-white p-8">
      <div className="max-w-2xl mx-auto space-y-8">
        <div className="text-center space-y-4">
          <h1 className="text-4xl font-bold">My XRPL EVM DApp</h1>
          <p className="text-gray-400">
            Connect with social login or your wallet
          </p>
        </div>

        <ConnectWallet />
      </div>
    </main>
  )
}
```

---

## Social Login Features

### Available Social Providers

Reown AppKit supports the following social login providers:

1. **Google** - OAuth via Google accounts
2. **X (Twitter)** - OAuth via X accounts
3. **GitHub** - OAuth via GitHub accounts
4. **Discord** - OAuth via Discord accounts
5. **Apple** - Sign in with Apple
6. **Farcaster** - Farcaster protocol authentication

### How Social Login Works

1. User clicks "Connect Wallet"
2. Reown modal opens with social login options
3. User selects a social provider (e.g., Google)
4. User authenticates with the social provider
5. Reown creates an embedded wallet for the user
6. User can now interact with your dApp

### Advantages of Social Login

- **Lower barrier to entry** - No need to install wallet extensions
- **Familiar UX** - Users authenticate with services they already use
- **Non-custodial** - Users maintain control of their embedded wallets
- **Recovery options** - Users can recover access through their social accounts

### Customizing Social Login

To customize which social providers are available:

```typescript
features: {
  socials: [
    'google',    // Keep only the providers you want
    'github',
  ],
  email: true, // Enable email login as alternative
}
```

---

## Testing

### 1. Run Development Server

```bash
npm run dev
```

Visit `http://localhost:3000`

### 2. Test Social Login

1. Click "Connect Wallet"
2. Choose a social provider (e.g., Google)
3. Authenticate with your social account
4. Verify your wallet is connected
5. Check that you can see your address and balance

### 3. Test Network Switching

1. Connect your wallet
2. Try switching between XRPL EVM and XRPL EVM Testnet
3. Verify the network indicator updates correctly

### 4. Test with Different Providers

Test your dApp with multiple social providers to ensure compatibility:

- Google
- GitHub
- Discord
- X (Twitter)

---

## Troubleshooting

### Common Issues

#### 1. "Project ID is not defined" Error

**Solution:** Ensure your `.env.local` file contains:

```env
NEXT_PUBLIC_PROJECT_ID=your_project_id
```

Restart your development server after adding environment variables.

#### 2. Social Login Modal Not Opening

**Solution:**

- Verify you're using the correct version: `@reown/appkit@1.7.6`
- Check browser console for errors
- Ensure `createAppKit` is called before rendering

#### 3. Wrong Network After Connection

**Solution:**

- Set `defaultNetwork` in your AppKit configuration
- Use the network switcher button to guide users
- Show clear network status indicators

#### 4. Type Errors with Wagmi

**Solution:**

- Ensure you have the overrides in `package.json`
- Use exact versions specified in this guide
- Clear `node_modules` and reinstall: `rm -rf node_modules package-lock.json && npm install`

#### 5. Hydration Errors

**Solution:**

- Ensure all components using hooks are marked with `'use client'`
- Use `cookieToInitialState` in your layout
- Verify SSR is enabled in wagmiAdapter: `ssr: true`

### Debug Mode

Enable debug mode for more detailed logs:

```typescript
const modal = createAppKit({
  // ... other config
  debug: true, // Add this line
});
```

### Network Issues

If users can't connect to XRPL EVM networks:

1. Verify RPC URLs are accessible
2. Check Chain IDs are correct (1440000 for mainnet, 1449000 for testnet)
3. Test with a different RPC provider if available

---

## Best Practices

### Security

1. **Never expose private keys** - Use environment variables for sensitive data
2. **Validate transactions** - Always verify transaction details before signing
3. **Use HTTPS** - Deploy your dApp with SSL/TLS
4. **Audit dependencies** - Regularly update and audit your packages

### User Experience

1. **Show clear connection status** - Users should always know if they're connected
2. **Display network correctly** - Show which network users are on
3. **Handle errors gracefully** - Provide helpful error messages
4. **Mobile responsive** - Test on mobile devices
5. **Loading states** - Show loading indicators for async operations

### Performance

1. **Lazy load components** - Use dynamic imports where appropriate
2. **Optimize images** - Use Next.js Image component
3. **Cache data** - Use React Query caching effectively
4. **Minimize bundle size** - Only import what you need

---

## Additional Resources

### Documentation

- [Reown AppKit Docs](https://docs.reown.com/appkit/overview)
- [Wagmi Documentation](https://wagmi.sh/)
- [XRPL EVM Documentation](https://docs.xrplevm.org/)

### Tools

- [Reown Cloud](https://cloud.reown.com) - Get your Project ID
- [XRPL EVM Explorer](https://explorer.xrplevm.org) - Mainnet explorer
- [XRPL EVM Testnet Explorer](https://explorer.testnet.xrplevm.org) - Testnet explorer
- [XRPL EVM Testnet Faucet](https://faucet.xrplevm.org) - Get testnet XRP

### Community

- [XRPL EVM Discord](https://discord.gg/xrplevm)
- [Reown Discord](https://discord.com/invite/reown)

---

## Example Repository

For a complete working example, check out:

- Repository: [reown-xrplevm-dapp](https://github.com/vriveraPeersyst/reown-xrplevm-dapp)
- Live Demo: [reown-xrplevm-dapp](https://reown-xrpl-dapp.vercel.app/)

---

## Next Steps

After integrating Reown social login:

1. **Add Transaction Features** - Implement token transfers, swaps, or other Web3 interactions
2. **Customize UI** - Style the modal and components to match your brand
3. **Add Analytics** - Track user connections and interactions
4. **Deploy** - Deploy your dApp to Vercel, Netlify, or your preferred platform
5. **Test Thoroughly** - Test with real users across different devices and browsers

---

## Support

If you encounter issues:

1. Check this guide's troubleshooting section
2. Review the [Reown AppKit documentation](https://docs.reown.com/appkit)
3. Ask in the [XRPL EVM Discord](https://discord.gg/xrplevm)
4. Open an issue in the [example repository](https://github.com/vriveraPeersyst/reown-xrplevm-dapp/issues)

---

**Last Updated:** November 2025  
**AppKit Version:** 1.7.6  
**Wagmi Version:** 2.15.4
