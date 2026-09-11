# Stream Onchain Data with Mirror Pipelines

**Goldsky Mirror** lets you stream **onchain data directly into your database** with sub-second latency. Mirror pipelines give you the flexibility to combine onchain and offchain data, power real-time dashboards, APIs, and analytics without maintaining complex infrastructure.

## How It Works

Goldsky Mirror uses `.yaml` pipeline configurations to define **data pipelines** that:

1. Are **reorg-aware** and always up-to-date
2. Support **automatic backfill** and live streaming
3. Apply **transforms** to filter or reshape data
4. Work across chains with **harmonized timestamps**

Mirror handles the infrastructure for you, so you can focus on using the data instead of moving it.

## Build a Pipeline in 2 Steps

1. Select the chain you want to index, either through [direct indexing](https://docs.goldsky.com/mirror/sources/direct-indexing) or [subgraphs](https://docs.goldsky.com/mirror/sources/subgraphs). Filter and reshape your data with [transforms](https://docs.goldsky.com/mirror/transforms/transforms-overview).
2. Send data to the best [sink](https://docs.goldsky.com/mirror/extensions/channels/overview) for your use case: fast APIs, robust analytics, or even real-time channels. Pick from Postgres, ClickHouse, Kafka, and more.

> **View supported chains** including XRPL EVM:
> [https://docs.goldsky.com/chains/supported-networks](https://docs.goldsky.com/chains/supported-networks)

## Get Started in 5 Minutes

Set up your first database pipeline by [creating a pipeline](https://docs.goldsky.com/mirror/create-a-pipeline). Mirror supports both a drag-and-drop **visual editor (Flow)** and a **CLI-based workflow** for power users.

## Getting Started

> Step-by-step walkthrough for building your first pipeline

[Watch here](https://docs.goldsky.com/mirror/create-a-pipeline)

You can create a pipeline using:

1. **[Goldsky Flow](https://docs.goldsky.com/mirror/create-a-pipeline#goldsky-flow)**: a visual editor for building pipelines in your browser
2. **[Goldsky CLI](https://docs.goldsky.com/mirror/create-a-pipeline#goldsky-cli)**: configure and deploy via command line
