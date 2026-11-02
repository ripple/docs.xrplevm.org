# Adding Horcrux

## What is Horcrux?

Horcrux is a secure multi-party computation (MPC) signing service designed for CometBFT nodes (formerly known as Tendermint). It enhances the security and availability of validator infrastructures by introducing fault-tolerant block signing mechanisms and advanced private key protection.

### Key Features

- **High Availability (HA)**: Horcrux replaces traditional remote signers with a cluster of signer nodes, enabling fault-tolerant block signing and minimizing downtime risks.
- **Enhanced Security**: It protects your validator’s private key by splitting it across multiple signer nodes using threshold Ed25519 signatures, ensuring no single point of failure.
- **Optimized Performance**: Provides robust security and availability without compromising block signing performance.

### Why is Horcrux Important?

A validator must stay online and must never sign twice. Downtime gets it jailed, and double signing gets it tombstoned.

Horcrux addresses these challenges by:

1. **Preventing Downtime**: Traditional low-availability setups require manual failovers, which can lead to downtime if intervention is delayed. Horcrux’s fault-tolerant design mitigates this risk.
2. **Avoiding Double Signing**: High-availability systems that rely on hot spares risk double signing in the event of failover detection bugs. Horcrux’s consensus and failover detection mechanisms reduce this risk.
3. **Ensuring Consensus**: Through multi-party computation and Raft-based leader election, Horcrux ensures reliable and secure operations, balancing high availability with robust security.

## Learn More and Install Horcrux

For detailed installation instructions and migration guidance, refer to the Horcrux migration guide:

[Horcrux Installation Guide](https://github.com/amygdala-labs/horcrux/blob/main/docs/migrating.md)

{% admonition type="warning" name="Archived repository" %}
The Horcrux repository (formerly `strangelove-ventures/horcrux`, now `amygdala-labs/horcrux`) was archived in March 2025 and no longer receives updates. Take this into account before using it on a production validator.
{% /admonition %}
