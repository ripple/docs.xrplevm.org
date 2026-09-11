# Resolving **.xrpl Domains** with ZNS

> **Audience:** Developers building wallets, dApps, or tools that need to resolve human-readable .xrpl domain names to Ethereum addresses.
>
> **Key fact:** The ZNS Registry on XRPL EVM provides on-chain domain resolution, reverse lookup, and caching strategies for optimal performance.
>
> **Registry Address:** `0xf180136DdC9e4F8c9b5A9FE59e2b1f07265C5D4D` (Mainnet)

---

## Why use .xrpl domains?

| Traditional flow                      | .xrpl domain flow                |
| ------------------------------------- | -------------------------------- |
| Copy/paste 42-character hex addresses | Type **alice.xrpl** → resolve    |
| Risk of typos and phishing            | Human-readable, verifiable names |
| No reverse lookup                     | Address → primary domain lookup  |

.xrpl domains make your dApp more user-friendly and reduce errors when sending transactions.

---

## Overview

The **ZNS Registry** is a smart contract that maps .xrpl domain names to Ethereum addresses. It provides:

- **Forward resolution:** domain name → address
- **Reverse resolution:** address → primary domain name
- **Token-based ownership:** Each domain is an NFT (ERC-721)
- **Expiration tracking:** Domains have expiration dates

### Key concepts

- **Domain names** are stored **without the `.xrpl` suffix** in the registry
- Each domain has a unique **token ID**
- Users can set a **primary domain** for reverse lookups
- Domains can **expire** and need renewal

---

## Quick-start code (TypeScript + Viem)

### Prerequisites

```bash
npm install viem
```

### Basic setup

```typescript
import { createPublicClient, http, getAddress } from "viem";

// Define XRPL EVM chain config
const XRPL_EVM = {
  id: 1440000,
  name: "XRPL EVM Sidechain",
  nativeCurrency: { name: "XRP", symbol: "XRP", decimals: 18 },
  rpcUrls: {
    default: { http: ["https://rpc.xrplevm.org"] },
  },
};

// ZNS Registry address (verified on mainnet)
export const ZNS_REGISTRY = "0xf180136DdC9e4F8c9b5A9FE59e2b1f07265C5D4D";

// Create a public client for read operations
export const publicClient = createPublicClient({
  chain: XRPL_EVM,
  transport: http(XRPL_EVM.rpcUrls.default.http[0]),
});
```

### Forward resolution: domain → address

```typescript
// ABI for domain lookup (returns token ID)
const ABI_DOMAIN_LOOKUP = [
  {
    inputs: [{ name: "", type: "string" }],
    name: "domainLookup",
    outputs: [{ name: "", type: "uint256" }],
    stateMutability: "view",
    type: "function",
  },
] as const;

// ABI for registry lookup by token ID
const ABI_REGISTRY_LOOKUP_BY_ID = [
  {
    inputs: [{ name: "tokenId", type: "uint256" }],
    name: "registryLookupById",
    outputs: [
      {
        name: "",
        type: "tuple",
        components: [
          { name: "owner", type: "address" },
          { name: "domainName", type: "string" },
          { name: "lengthOfDomain", type: "uint16" },
          { name: "expirationDate", type: "uint256" },
        ],
      },
    ],
    stateMutability: "view",
    type: "function",
  },
] as const;

async function resolveXrplDomain(name: string): Promise<string | null> {
  if (!name.toLowerCase().endsWith(".xrpl")) return null;

  // Strip .xrpl suffix (registry stores names without it)
  const nameWithoutTld = name.toLowerCase().replace(/\.xrpl$/, "");

  try {
    // Step 1: Get token ID for the domain
    const tokenId = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_DOMAIN_LOOKUP,
      functionName: "domainLookup",
      args: [nameWithoutTld],
    })) as bigint;

    if (!tokenId || tokenId === BigInt(0)) return null;

    // Step 2: Get registry data including owner address
    const registryData = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_REGISTRY_LOOKUP_BY_ID,
      functionName: "registryLookupById",
      args: [tokenId],
    })) as {
      owner: `0x${string}`;
      domainName: string;
      lengthOfDomain: number;
      expirationDate: bigint;
    };

    // Return checksummed address
    if (
      registryData?.owner &&
      registryData.owner !== "0x0000000000000000000000000000000000000000"
    ) {
      return getAddress(registryData.owner);
    }
  } catch (error) {
    console.error("Domain resolution failed:", error);
  }

  return null;
}

// Usage
const address = await resolveXrplDomain("alice.xrpl");
console.log(address); // 0x1234...
```

### Reverse resolution: address → domain

```typescript
// ABI for user lookup by address
const ABI_USER_LOOKUP = [
  {
    inputs: [{ name: "user", type: "address" }],
    name: "userLookupByAddress",
    outputs: [
      {
        name: "",
        type: "tuple",
        components: [
          { name: "primaryDomain", type: "uint256" },
          { name: "allOwnedDomains", type: "uint256[]" },
          { name: "numberOfReferrals", type: "uint256" },
          { name: "totalEarnings", type: "uint256" },
        ],
      },
    ],
    stateMutability: "view",
    type: "function",
  },
] as const;

async function resolveAddressToDomain(address: string): Promise<string | null> {
  // Validate address format
  if (!/^0x[a-fA-F0-9]{40}$/.test(address)) return null;

  try {
    // Step 1: Get user config (includes primary domain token ID)
    const userConfig = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_USER_LOOKUP,
      functionName: "userLookupByAddress",
      args: [address as `0x${string}`],
    })) as {
      primaryDomain: bigint;
      allOwnedDomains: readonly bigint[];
      numberOfReferrals: bigint;
      totalEarnings: bigint;
    };

    if (!userConfig.primaryDomain || userConfig.primaryDomain === BigInt(0)) {
      return null; // No primary domain set
    }

    // Step 2: Get domain name from token ID
    const registryData = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_REGISTRY_LOOKUP_BY_ID,
      functionName: "registryLookupById",
      args: [userConfig.primaryDomain],
    })) as {
      owner: `0x${string}`;
      domainName: string;
      lengthOfDomain: number;
      expirationDate: bigint;
    };

    if (registryData?.domainName) {
      // Add .xrpl suffix if not present
      return registryData.domainName.toLowerCase().endsWith(".xrpl")
        ? registryData.domainName
        : `${registryData.domainName}.xrpl`;
    }
  } catch (error) {
    console.error("Reverse lookup failed:", error);
  }

  return null;
}

// Usage
const domain = await resolveAddressToDomain("0x1234...");
console.log(domain); // alice.xrpl
```

---

## Performance optimization with caching

Since blockchain reads can be slow, implement a simple in-memory cache:

```typescript
// Simple cache with 5-minute TTL
const resolutionCache = new Map<
  string,
  {
    address: string | null;
    timestamp: number;
  }
>();

const CACHE_DURATION = 5 * 60 * 1000; // 5 minutes

// Clear expired entries periodically
setInterval(() => {
  const now = Date.now();
  for (const [key, value] of resolutionCache.entries()) {
    if (now - value.timestamp > CACHE_DURATION) {
      resolutionCache.delete(key);
    }
  }
}, CACHE_DURATION);

async function resolveXrplDomainCached(name: string): Promise<string | null> {
  const nameWithoutTld = name.toLowerCase().replace(/\.xrpl$/, "");

  // Check cache first
  const cached = resolutionCache.get(nameWithoutTld);
  if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
    return cached.address;
  }

  // Perform resolution (implementation from section 3)
  const address = await resolveXrplDomain(name);

  // Cache result (including null to avoid repeated failed lookups)
  resolutionCache.set(nameWithoutTld, {
    address,
    timestamp: Date.now(),
  });

  return address;
}
```

> **Tip:** For production apps, consider using Redis or another distributed cache for better scalability.

---

## Alternative resolution method

The ZNS Registry also provides `registryLookupByName` for direct name-to-owner lookup:

```typescript
const ABI_REGISTRY_LOOKUP_BY_NAME = [
  {
    inputs: [{ name: "domainName", type: "string" }],
    name: "registryLookupByName",
    outputs: [
      {
        name: "",
        type: "tuple",
        components: [
          { name: "owner", type: "address" },
          { name: "domainName", type: "string" },
          { name: "lengthOfDomain", type: "uint16" },
          { name: "expirationDate", type: "uint256" },
        ],
      },
    ],
    stateMutability: "view",
    type: "function",
  },
] as const;

async function resolveDirectly(name: string): Promise<string | null> {
  const nameWithoutTld = name.toLowerCase().replace(/\.xrpl$/, "");

  try {
    const info = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_REGISTRY_LOOKUP_BY_NAME,
      functionName: "registryLookupByName",
      args: [nameWithoutTld],
    })) as {
      owner: `0x${string}`;
      domainName: string;
      lengthOfDomain: number;
      expirationDate: bigint;
    };

    if (
      info?.owner &&
      info.owner !== "0x0000000000000000000000000000000000000000"
    ) {
      return getAddress(info.owner);
    }
  } catch (error) {
    console.error("Direct lookup failed:", error);
  }

  return null;
}
```

> **Note:** The `domainLookup` → `registryLookupById` approach is typically faster as it uses indexed token IDs.

---

## Complete implementation with fallbacks

Here's a production-ready implementation combining all strategies:

```typescript
import { createPublicClient, http, getAddress } from "viem";

export const ZNS_REGISTRY = "0xf180136DdC9e4F8c9b5A9FE59e2b1f07265C5D4D";

// In-memory cache
const resolutionCache = new Map<
  string,
  {
    address: `0x${string}` | null;
    timestamp: number;
  }
>();
const CACHE_DURATION = 5 * 60 * 1000;

// ABIs (from sections above)
const ABI_DOMAIN_LOOKUP = [/* ... */];
const ABI_REGISTRY_LOOKUP_BY_ID = [/* ... */];
const ABI_REGISTRY_LOOKUP_BY_NAME = [/* ... */];

export const publicClient = createPublicClient({
  chain: {
    id: 1440000,
    name: "XRPL EVM Sidechain",
    nativeCurrency: { name: "XRP", symbol: "XRP", decimals: 18 },
    rpcUrls: {
      default: { http: ["https://rpc.xrplevm.org"] },
    },
  },
  transport: http("https://rpc.xrplevm.org"),
});

export async function resolveXrplNameToAddress(
  name: string,
): Promise<`0x${string}` | null> {
  if (!name.toLowerCase().endsWith(".xrpl")) return null;

  const nameWithoutTld = name.toLowerCase().replace(/\.xrpl$/, "");
  if (!nameWithoutTld.trim()) return null;

  // Check cache
  const cached = resolutionCache.get(nameWithoutTld);
  if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
    return cached.address;
  }

  let resolvedAddress: `0x${string}` | null = null;

  // Method 1: domainLookup + registryLookupById (faster)
  try {
    const tokenId = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_DOMAIN_LOOKUP,
      functionName: "domainLookup",
      args: [nameWithoutTld],
    })) as bigint;

    if (tokenId && tokenId > BigInt(0)) {
      const registryData = (await publicClient.readContract({
        address: ZNS_REGISTRY,
        abi: ABI_REGISTRY_LOOKUP_BY_ID,
        functionName: "registryLookupById",
        args: [tokenId],
      })) as any;

      if (
        registryData?.owner &&
        registryData.owner !== "0x0000000000000000000000000000000000000000"
      ) {
        resolvedAddress = getAddress(registryData.owner);
      }
    }
  } catch (error) {
    console.debug("Primary resolution failed, trying fallback:", error);
  }

  // Method 2: registryLookupByName (fallback)
  if (!resolvedAddress) {
    try {
      const info = (await publicClient.readContract({
        address: ZNS_REGISTRY,
        abi: ABI_REGISTRY_LOOKUP_BY_NAME,
        functionName: "registryLookupByName",
        args: [nameWithoutTld],
      })) as any;

      if (
        info?.owner &&
        info.owner !== "0x0000000000000000000000000000000000000000"
      ) {
        resolvedAddress = getAddress(info.owner);
      }
    } catch (error) {
      console.debug("Fallback resolution failed:", error);
    }
  }

  // Cache result
  resolutionCache.set(nameWithoutTld, {
    address: resolvedAddress,
    timestamp: Date.now(),
  });

  return resolvedAddress;
}

export function isValidAddress(address: string): boolean {
  return /^0x[a-fA-F0-9]{40}$/.test(address);
}

export async function resolveAddressToXrplName(
  address: string,
): Promise<string | null> {
  if (!isValidAddress(address)) return null;

  try {
    const userConfig = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_USER_LOOKUP,
      functionName: "userLookupByAddress",
      args: [address as `0x${string}`],
    })) as any;

    if (!userConfig.primaryDomain || userConfig.primaryDomain === BigInt(0)) {
      return null;
    }

    const registryData = (await publicClient.readContract({
      address: ZNS_REGISTRY,
      abi: ABI_REGISTRY_LOOKUP_BY_ID,
      functionName: "registryLookupById",
      args: [userConfig.primaryDomain],
    })) as any;

    if (registryData?.domainName) {
      return registryData.domainName.toLowerCase().endsWith(".xrpl")
        ? registryData.domainName
        : `${registryData.domainName}.xrpl`;
    }
  } catch (error) {
    console.debug("Reverse lookup failed:", error);
  }

  return null;
}
```

---

## Integration checklist

- [ ] Install `viem` dependency
- [ ] Configure XRPL EVM chain in your app
- [ ] Implement forward resolution for wallet/dApp inputs
- [ ] Add reverse resolution for displaying user identities
- [ ] Implement caching strategy (in-memory or distributed)
- [ ] Handle edge cases (expired domains, unset primary domains)
- [ ] Test on testnet before mainnet deployment
- [ ] Add error handling for network failures

---

## Common patterns

### Wallet integration

```typescript
// Accept both addresses and .xrpl domains in send forms
async function validateRecipient(input: string): Promise<string | null> {
  // Check if it's already a valid address
  if (isValidAddress(input)) {
    return input;
  }

  // Try resolving as .xrpl domain
  if (input.toLowerCase().endsWith(".xrpl")) {
    return await resolveXrplNameToAddress(input);
  }

  return null;
}
```

### Display user-friendly names

```typescript
// Show domain instead of address when available
async function formatAddress(address: string): Promise<string> {
  const domain = await resolveAddressToXrplName(address);
  return domain || `${address.slice(0, 6)}...${address.slice(-4)}`;
}
```

### Batch resolution

```typescript
// Resolve multiple domains in parallel
async function resolveBatch(
  names: string[],
): Promise<Map<string, string | null>> {
  const results = await Promise.all(
    names.map(async (name) => ({
      name,
      address: await resolveXrplNameToAddress(name),
    })),
  );

  return new Map(results.map((r) => [r.name, r.address]));
}
```

---

## FAQ

**Q:** _Do I need to deploy my own registry contract?_
**A:** No, use the existing ZNS Registry at `0xf180136DdC9e4F8c9b5A9FE59e2b1f07265C5D4D`.

**Q:** _What if a domain has expired?_
**A:** The registry will still return the owner, but you should check the `expirationDate` field if freshness matters.

**Q:** _Can I use this with ethers.js instead of viem?_
**A:** Yes! The contract addresses and ABIs are the same; just adapt the client initialization and contract call syntax.

**Q:** _How do I register a .xrpl domain?_
**A:** Visit the official ZNS platform or use their registration contract (not covered in this guide).

**Q:** _Does the registry support subdomains (e.g., wallet.alice.xrpl)?_
**A:** Check the ZNS documentation for subdomain support; this guide covers primary domain resolution.

---

## Need help?

**Discord:** [https://discord.gg/xrplevm](https://discord.gg/xrplevm)

**ZNS Registry Contract:** `0xf180136DdC9e4F8c9b5A9FE59e2b1f07265C5D4D`

**ZNS Buy Domain** [https://zns.bio/](https://zns.bio/search?tab=smart&domain=pepe&chain=1440000)

**Transfer to .xrpl domain** [ens.xrplevm.org](https://ens.xrplevm.org)

Happy building!
