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

To complete your understanding of dApp development, create a frontend application that connects to the XRPL EVM. Below is a step-by-step guide to help you start building a **Next.js TypeScript Starter Kit** that integrates Wallet Connect, MetaMask, and XRPL EVM functionality.

### Setup Instructions

#### 1. Clone the Starter Kit

Start with a pre-configured template for building blockchain frontends:

```bash
git clone https://github.com/kenyiu/web3_starter_kit.git
cd nextjs-dapp-starter
```

#### 2. Configure the Environment

Create a `.env.local` file with the following variable:

```plaintext
NEXT_PUBLIC_PROJECT_ID=<YOUR_WALLETCONNECT_PROJECT_ID>
```

Replace `<YOUR_WALLETCONNECT_PROJECT_ID>` with your actual WalletConnect project ID from [WalletConnect Cloud](https://cloud.walletconnect.com/).

#### 3. Install Dependencies

Run the following command to install all required packages:

```bash
npm install
```

#### 4. Add XRPL EVM Network Configuration

Update the `src/chains/xrplEvmChain.ts` file with XRPL EVM-specific details:

```typescript
import { defineChain, type Chain } from "viem";

export const xrplEvmChain = defineChain({
  id: 1440002,
  name: "XRPL EVM",
  nativeCurrency: {
    name: "XRP",
    symbol: "XRP",
    decimals: 18,
  },
  rpcUrls: {
    default: { http: ["https://rpc.xrplevm.org"] },
  },
  blockExplorers: {
    default: { name: "Blockscout", url: "https://explorer.xrplevm.org" },
  },
} as const satisfies Chain);
```

#### 5. Run the Development Server

Launch the app with the following command:

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) to interact with your dApp.

---

### Example Features

The Starter Kit includes basic functionality to interact with smart contracts and the XRPL EVM. Below are some key features:

#### Connect Wallet

Use Wallet Connect or MetaMask to connect to the XRPL EVM network:

```tsx
<Button onClick={handleConnect}>Connect Wallet</Button>
```

#### Sign Messages

Allow users to sign messages programmatically:

```tsx
<Button onClick={() => signMessage({ message: "Hello, XRPL EVM!" })}>
  Sign Message
</Button>
```

#### Send Transactions

Send transactions with custom amounts:

```tsx
<Button
  onClick={() =>
    sendTransaction({
      to: "0xRecipientAddress",
      value: parseEther("10"),
    })
  }
>
  Send 10 XRP
</Button>
```

---

## Learning Resources

To further refine your skills, explore the following resources:

- **Solidity Documentation**: Comprehensive guide to Solidity ([Read More](https://docs.soliditylang.org/)).
- **Ethers.js Documentation**: Official guide for Ethers.js library ([Read More](https://docs.ethers.io/v5/)).
- **Web3.js Documentation**: Learn how to use Web3.js to interact with blockchains ([Read More](https://web3js.readthedocs.io/)).
- **Remix IDE Documentation**: Guide to using Remix for Solidity development ([Read More](https://remix-ide.readthedocs.io/)).
- **Hardhat Documentation**: A robust framework for Ethereum smart contract development ([Read More](https://hardhat.org/docs)).
- **Next.js Documentation**: Learn how to build scalable web applications using Next.js ([Read More](https://nextjs.org/docs)).
- **WAGMI Documentation**: Integrate blockchain functionality into React applications ([Read More](https://wagmi.sh/react/getting-started)).
- **Tailwind CSS**: Style your dApp with utility-first CSS ([Read More](https://tailwindcss.com/docs)).
- **Viem Documentation**: Learn about Viem for seamless on-chain interactions ([Read More](https://viem.sh/docs/getting-started)).
- **React Documentation**: Build dynamic user interfaces for your dApps ([Read More](https://react.dev/)).
- **XRPL EVM Documentation**: Explore more about XRPL EVM ([Read More](https://docs.xrplevm.org)).

---

## Deployment Options

Once your dApp is ready, deploy it using platforms like:

- **[Vercel](https://vercel.com)** for seamless Next.js hosting.
- **[Juno](https://juno.build)** for decentralized Web3 deployments.

---

With these tools and resources, you're well-equipped to create next-generation decentralized applications on the XRPL EVM. Keep building, innovating, and contributing to the ecosystem!
