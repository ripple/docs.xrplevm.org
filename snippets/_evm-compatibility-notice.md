{% admonition type="info" name="EVM Compatibility Notice" %}
Following the upgrade from **legacy evmOS** to **Cosmos EVM**, the **XRPL EVM** is compatible with:

- **EVM Version:** Prague
- **Solidity Compiler:** `solc` up to **0.8.30** works out of the box (its default EVM target is `prague`)

Contracts compiled for earlier EVM versions (Paris, Shanghai, Cancun) remain fully compatible. If you use a newer compiler release, set `evmVersion` to `prague` (or lower) in your compiler settings.
{% /admonition %}
