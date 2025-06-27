Here’s the improved **Mirror introduction page** content, rewritten for clarity, structure, and improved engagement — including your provided content and the link to supported network IDs:

---

# Introduction

## Stream Onchain Data with Mirror Pipelines

**Goldsky Mirror** lets you stream **onchain data directly into your database** with sub-second latency. Mirror pipelines give you the flexibility to combine onchain and offchain data, power real-time dashboards, APIs, and analytics without maintaining complex infrastructure.

---

## How It Works

Goldsky Mirror uses `.yaml` pipeline configurations to define **data pipelines** that:

1. Are **reorg-aware** and always up-to-date
2. Support **automatic backfill** and live streaming
3. Apply **transforms** to filter or reshape data
4. Work across chains with **harmonized timestamps**

Mirror handles the infrastructure for you—so you can focus on using the data, not moving it.

---

## Build a Pipeline in 3 Steps

<Steps>
  <Step title="Choose a chain">
    <Card title="Mirror supports 130+ chains" icon="arrow-right-from-bracket" iconType="duotone" href="/mirror/sources/supported-sources">
      Select the chain you want to index—either through [direct indexing](/mirror/sources/direct-indexing) or [subgraphs](/mirror/sources/subgraphs). Filter and reshape your data with [transforms](/mirror/transforms/transforms-overview).
    </Card>
  </Step>

  <Step title="Choose a database">
    <Card title="Optimize data for your use case" icon="arrow-right-to-bracket" iconType="duotone" href="/mirror/sinks/supported-sinks">
      Send data to the best sink for your use case: fast APIs, robust analytics, or even real-time [channels](/mirror/extensions/channels/overview). Pick from Postgres, ClickHouse, Kafka, and more.
    </Card>
  </Step>
</Steps>

> 📚 **View supported chains** including XRPL EVM:
> [https://docs.goldsky.com/chains/supported-networks](https://docs.goldsky.com/chains/supported-networks)

---

## Get Started in 5 Minutes

Set up your first database pipeline by [creating a pipeline](https://docs.goldsky.com/mirror/create-a-pipeline). Mirror supports both a drag-and-drop **visual editor (Flow)** and a **CLI-based workflow** for power users.


---

## Getting Started

> 📺 Step-by-step walkthrough for building your first pipeline

[Watch here](https://docs.goldsky.com/mirror/create-a-pipeline)

You can create a pipeline using:

1. **[Goldsky Flow](https://docs.goldsky.com/mirror/create-a-pipeline#goldsky-flow)** – a visual editor for building pipelines in your browser
2. **[Goldsky CLI](https://docs.goldsky.com/mirror/create-a-pipeline#creating-mirror-pipelines-with-the-cli)** – configure and deploy via command line

---

Let me know if you'd like me to revise the rest of the long page (Goldsky Flow section and beyond), or if you want this intro split into a landing and getting-started doc!
