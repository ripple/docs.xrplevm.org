# Best Practices for Goldsky Indexing on XRPL EVM

To get the most out of your Goldsky setup and ensure smooth performance and scalability, follow these recommended practices:

## 1. Upload Accurate and Minimal ABI

Only include ABI entries relevant to the events you plan to index. This reduces noise and improves decoding accuracy.

## 2. Start from a Logical Block Number

Avoid syncing from block `0` unless absolutely necessary. Set a reasonable `startBlock` to speed up syncing and reduce resource usage.

## 3. Keep Schema Clean and Domain-Specific

Structure your entities based on your app’s business model. Use clear and semantic naming like `UserPrediction`, `Matchday`, or `RewardClaim`.

## 4. Avoid Over-Indexing

Index only the events and fields you truly need. This keeps response times fast and subgraph costs low.

## 5. Version Your Indexer

If your contract is upgraded or schema changes, create a new subgraph version rather than modifying the live one. This ensures continuity and reliability.

## 6. Monitor and Log Errors

Watch for decoding failures, unexpected null fields, or schema mismatches in the Goldsky dashboard. Fix issues early to avoid gaps in data.

## 7. Integrate with CI/CD (Advanced)

Use Goldsky’s CLI or APIs to automate schema deployment as part of your app’s continuous deployment pipeline.

---