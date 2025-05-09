---
title: Indexer
sidebar_position: 3
---

# Indexing XRPL EVM with Goldsky

Goldsky provides high-performance subgraphs for querying onchain data from XRPL EVM. Subgraphs allow developers to efficiently access indexed blockchain data via GraphQL endpoints, optimized for speed, scale, and ease of use.

## Supported Networks

| Network  | Status           | Subgraph Slug       |
|----------|------------------|---------------------|
| Testnet  |     ✅           | `xrplevm-testnet`   |
| Mainnet  |     🚧           | *coming soon*       |

## What is a Subgraph?

A **subgraph** defines how data is extracted from the blockchain, how it’s stored, and how it can be queried. With Goldsky, subgraphs are deployed instantly and maintained with real-time updates — no manual syncing or infrastructure setup needed.

Each subgraph exposes a **GraphQL API** endpoint, allowing you to query data such as:

- Contract events (e.g. `Transfer`, `Swap`)
- Account balances
- Token metadata
- Custom application logic

## Benefits of Indexing

Indexing makes it easier to work with onchain data by turning raw blockchain activity into clean, queryable data.

- **Fast queries** — Access structured data instantly using GraphQL  
- **No infra required** — Goldsky hosts everything for you  
- **Customizable** — Index only what your app needs  
- **Real-time updates** — New data is synced as blocks are produced  
- **Webhooks** — Trigger actions when new data matches your query  
- **Easy to manage** — Deploy and update subgraphs in minutes

## Getting Started

Follow these steps to get started with Goldsky:

1. [Create an account](https://app.goldsky.com) 

2. Generate an API key

3. Install the Goldsky CLI

Once you've completed these steps, follow the guide [here](https://docs.goldsky.com/subgraphs/deploying-subgraphs) to deploy your first subgraph 🚀

Feel free to reach out to [Goldsky](https://docs.goldsky.com/getting-support) anytime if you have questions or need support!
