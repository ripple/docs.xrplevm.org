# Querying Events from Your Goldsky Indexer

After your Goldsky indexer is live, you can begin querying contract events using standard GraphQL syntax. Below is an example that fetches the last five `Transfer` events:

```graphql
{
  transfers(first: 5, orderBy: timestamp, orderDirection: desc) {
    id
    from
    to
    value
    timestamp
  }
}
```

## Use Cases

* Render user transaction history on profile pages
* Display leaderboards based on contract interactions
* Monitor token minting, burning, and reward distributions

## Tools

You can test queries using:

* [Goldsky Studio Playground](https://app.goldsky.com/)
* Your app frontend (React, Next.js, etc.)
* GraphQL clients like Apollo, URQL, or Axios wrappers

## Custom Entities

Define and query custom business logic entities such as:

* `Predictions`
* `MatchResults`
* `Trophies`
* `Groups`

Goldsky gives you full flexibility to tailor the schema to your use case.

---
