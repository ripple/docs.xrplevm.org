# Managing keys

Effective key management is critical for the security and operation of validators in the XRPL EVM sidechain. This document provides an overview of the types of keys used by a validator and explains how to manage them using the `exrpd` binary.

## Introduction to validator Keys

Validators in the XRPL EVM sidechain rely on two distinct types of keys, each serving specific purposes. Keeping both keys secure and backed up is essential to ensure the reliability and safety of the validator. Compromise or loss of these keys can lead to unauthorized access, disruption of operations, or even the inability to participate in consensus. Proper storage, regular backups, and using secure mechanisms like hardware wallets or encrypted backups are recommended best practices.

### Node Key

- **Purpose**: Used for consensus and block signing.
- **Location**: Stored in `config/priv_val_key.json`.
- **Importance**: The node key is critical for validator operations. If compromised, it could allow malicious actors to manipulate block signing, impacting the chain's integrity.

### Operator Key

- **Purpose**: Used for managing the validator node, including governance actions and node configuration.
- **Flexibility**: Unlike the node key, operator keys can be stored externally or within the `exrpd` keyring.
- **Security Considerations**: Proper storage and handling of the operator key are crucial, as these keys grant administrative access to the node.

## Methods for managing Operator Keys

The operator key can be managed using two primary methods:

### External wallets

- **Example**: Tools like [Keplr](https://keplr.app/) provide secure storage for operator keys.
- **Advantages**:
  - Enhanced security by separating the key from the node.
  - User-friendly interfaces for managing keys and interacting with governance.
- **Recommendation**: This method is ideal for users prioritizing security and ease of use.

### Keyrings

The `exrpd` binary supports multiple keyring backends, each suited for different use cases:

- **OS**: Leverages the operating system's secure storage mechanisms, such as Keychain on macOS or Credential Manager on Windows. This method provides robust protection against unauthorized access.
- **File**: Stores keys in an encrypted file on disk, which can be easily backed up or transferred to another machine. Ensure the file's location is secured and inaccessible to unauthorized users.
- **Test**: Designed for development and testing purposes, this backend stores keys in plaintext.

{% admonition type="danger" name="Test keyring" %}
Test keyring is provided for testing purposes only It is not recommended for use in production environments.
{% /admonition %}

## Managing Operator Key with Keyrings

The `exrpd` CLI makes it straightforward to create and manage keys. Below are step-by-step instructions:

### Creating a new key

Run the following command to create a new key:

```bash
exrpd keys add <key_name> --keyring-backend <os|file|test>
```

When adding a new key, the `exrpd` CLI generates a mnemonic phrase and displays it only once during this process. It is essential to back up this mnemonic securely, as it is required for restoring the key in the future. Use a secure storage solution, such as a hardware wallet or encrypted backup service, to ensure it remains protected.

### Listing keys

To view the keys stored in the keyring, use the following command:

```bash
exrpd keys list --keyring-backend <os|file|test>
```

This will display the names of the keys along with their addresses. Regularly checking your keyring can help ensure that no unauthorized keys have been added.

### Back up a key

Keys should be backed up securely to prevent loss. Use the following command to display the private key:

```bash
exrpd keys export <key_name> --keyring-backend <os|file|test>
```

- **Important**: Store the mnemonic phrase in a secure location, such as a hardware wallet or encrypted storage.

### Restoring a key

To restore a key using the mnemonic, run:

```bash
exrpd keys import <name> <keyfile> --keyring-backend <os|file|test>
```

## Further Security Recommendations

For validators aiming to maximize security, consider [adding Horcrux](../advanced/adding-horocrux.md), a tool for managing distributed key signing. Horcrux splits keys into shares, requiring multiple parts to reconstruct the full key, significantly reducing the risk of compromise.
