---
html: axelar-overview.html
blurb: Overview of the Axelar bridge.
labels:
  - Interoperability
status: not_enabled
---

<embed src="/snippets/_axelar-overview-disclaimer.md" />

# Overview 

Bridges are specialized protocols designed to facilitate interoperability between distinct blockchain networks, allowing them to communicate and exchange assets or data despite differences in consensus algorithms, data structures, and other foundational technologies. They typically consist of smart contracts and off-chain components that validate and relay information between the connected chains, ensuring that transactions are securely executed across heterogeneous environments.

Currently, the transfer of XRP from the XRP Ledger (XRPL) to the XRPL EVM Sidechain is enabled through the Axelar Network. Axelar is a decentralized cross-chain communication protocol designed to provide secure and scalable connectivity across Web3 ecosystems. It leverages a proof-of-stake (PoS) consensus model, which has been proven robust in multiple blockchain environments such as Ethereum, Polygon, and Cosmos. This consensus mechanism is maintained by a network of validators that ensure message integrity and prevent double-spending or malicious activities during interchain transactions. By abstracting the complexities of cross-chain interactions, Axelar enables the development of "Interchain dApps" that natively support multi-chain functionalities, thus broadening the scope and capabilities of decentralized applications beyond single-chain constraints.

Leveraging Axelar's decentralised network of validators and robust message-passing protocol enables assets to move between the XRPL's native ledger and its EVM-compatible sidechain with full transactional consistency. This integration combines the XRPL's high-throughput and low-cost transactions with the programmable smart contract capabilities of the EVM ecosystem, allowing for expanded use cases, such as decentralised finance (DeFi) applications and cross-chain liquidity solutions.


For further details on the implementation of the Axelar bridge on the XRPL EVM sidechain, see: 

- [Axelar Interchain Transfer](axelar-interchain-transfer.md): Learn more about Axelar interchain transfer.
- [Axelar General Message Passing (GMP)](axelar-general-message-passing.md): Learn more about Axelar general message passing.
- [Deployed Contracts](axelar-deployed-contracts.md): List of deployed contracts on the XRPL EVM Sidechain networks.