# Interacting with the Node CLI

This guide provides an overview of the `exrpd` CLI (Command-Line Interface) for the XRPL EVM Sidechain. It is intended for newcomers who want to explore and interact with their own node at a deeper level. You will find the main commands and subcommands below, along with short descriptions of what they do and tips on how to use them.

## Overview

The `exrpd` CLI is organized into primary commands. Each command may have one or more **subcommands**. To list subcommands, you can run:

```
exrpd <command> --help
```

or, to see the main help, simply run:

```
exrpd --help
```

This displays usage, available commands, and descriptions for each command. The following sections cover each major command category in more detail.

## Configuration Commands

#### `exrpd config`

Use the `config` command to manage node configuration. This is helpful for setting up or inspecting config files.

| Subcommand | Description                                                                                              |
| ---------- | -------------------------------------------------------------------------------------------------------- |
| `diff`     | Outputs all config values that differ from the default `app.toml` configuration.                         |
| `get`      | Displays the current value of a specific configuration parameter.                                        |
| `home`     | Prints the folder used as the binary home directory. _(When using `confix` standalone, no home is set.)_ |
| `migrate`  | Migrates a Cosmos SDK app configuration file to a specified version.                                     |
| `set`      | Sets a new value for a given configuration parameter.                                                    |
| `view`     | Displays the entire configuration file.                                                                  |

**Examples**

- **Check all deviations from default config:**
  ```
  exrpd config diff
  ```
- **Get a specific config value:**
  ```
  exrpd config get <key_name>
  ```
- **Set a config value:**
  ```
  exrpd config set <key_name> <new_value>
  ```
- **Display current config file contents:**
  ```
  exrpd config view
  ```

## Key Management

#### `exrpd keys`

Use the `keys` command to manage local keys (private/public key pairs) for your application. This includes creating new keys, importing/exporting existing keys, and more.

{% admonition type="info" name="Advanced configuration" %}
Managing your node’s keys is essential. If you plan to operate a production node or validator, make sure to review the advanced key management configurations on the [Managing keys](../validators/managing-keys.md) page.
{% /admonition %}

| Subcommand              | Description                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------ |
| `add`                   | Generate or recover a key, encrypt it locally, and save it under a specified name.   |
| `delete`                | Delete an existing key from local storage.                                           |
| `export`                | Export a private key (either encrypted or unencrypted, depending on flags used).     |
| `import`                | Import a previously exported private key into the local keybase.                     |
| `list`                  | List all locally stored keys.                                                        |
| `migrate`               | Migrate keys from Amino to Proto serialization format.                               |
| `mnemonic`              | Generate or compute the BIP39 mnemonic from supplied entropy.                        |
| `parse`                 | Convert an address between hex and Bech32 encoding.                                  |
| `rename`                | Rename an existing local key.                                                        |
| `show`                  | Show information about a key, including its public address.                          |
| `unsafe-export-eth-key` | **UNSAFE**: Export an Ethereum private key. Use with caution.                        |
| `unsafe-import-eth-key` | **UNSAFE**: Import an Ethereum private key into the local keybase. Use with caution. |

**Examples**

- **Create a new key:**
  ```
  exrpd keys add myNewKey
  ```
- **List existing keys:**
  ```
  exrpd keys list
  ```
- **Export a key:**
  ```
  exrpd keys export myNewKey
  ```
- **Delete a key:**
  ```
  exrpd keys delete myNewKey
  ```

## Query Commands

#### `exrpd query`

The `query` command allows you to query data from various modules, transactions, blocks, or on-chain states. Use `exrpd query <subcommand> --help` for more on how to craft your queries.

Below is a list of subcommands within `query`:

| Subcommand            | Description                                                                        |
| --------------------- | ---------------------------------------------------------------------------------- |
| `auth`                | Query for authentication module data.                                              |
| `authz`               | Query for authorization module data.                                               |
| `bank`                | Query bank balances and other bank module data.                                    |
| `block`               | Query a committed block by height, hash, or matching events.                       |
| `block-results`       | Query the results of a committed block by height.                                  |
| `blocks`              | Query paginated blocks that match a set of events.                                 |
| `comet-validator-set` | Query the full CometBFT validator set at a specific block height.                  |
| `consensus`           | Query for consensus parameters.                                                    |
| `distribution`        | Query for distribution module data (rewards, commissions, etc.).                   |
| `erc20`               | Query ERC-20 contract-related information.                                         |
| `evidence`            | Query evidence of misbehavior.                                                     |
| `evm`                 | Query EVM module data.                                                             |
| `feegrant`            | Query for fee grant allowances.                                                    |
| `feemarket`           | Query data from the fee market module.                                             |
| `gov`                 | Query governance proposals, votes, deposits, and more.                             |
| `ibc`                 | Query Inter-Blockchain Communication (IBC) data.                                   |
| `ibc-transfer`        | Query for IBC token transfer details.                                              |
| `interchain-accounts` | Query IBC interchain accounts.                                                     |
| `params`              | Query for on-chain parameters.                                                     |
| `poa`                 | Query for Proof-of-Authority module data.                                          |
| `ratelimit`           | Query the rate-limit module data.                                                  |
| `slashing`            | Query data related to slashing, signing info, and parameters.                      |
| `staking`             | Query staking information (validators, delegations, unbonding, etc.).              |
| `tx`                  | Query a single transaction by its hash, `"address/seq"` combination, or signature. |
| `txs`                 | Query multiple transactions using events.                                          |
| `upgrade`             | Query network upgrade plans.                                                       |
| `wait-tx`             | Wait for a transaction to be included in a block.                                  |

**Examples**

- **Check an account balance:**
  ```
  exrpd query bank balances <account_address>
  ```
- **Query the latest block:**
  ```
  exrpd query block
  ```
- **Lookup a transaction by hash:**
  ```
  exrpd query tx <tx_hash>
  ```

## Transaction Commands

#### `exrpd tx`

The `tx` command handles transactions that change the state of the chain. Here, you can create, sign, broadcast, and simulate transactions.

Below is a list of subcommands within `tx`:

| Subcommand            | Description                                                                |
| --------------------- | -------------------------------------------------------------------------- |
| `auth`                | Send transactions related to the auth module.                              |
| `authz`               | Authorization transaction subcommands (grant, revoke, etc.).               |
| `bank`                | Send tokens and manage bank-related transactions.                          |
| `broadcast`           | Broadcast offline-generated transactions to the network.                   |
| `consensus`           | Manage consensus-related transactions.                                     |
| `crisis`              | Commands for crisis management (halt the chain, etc.).                     |
| `decode`              | Decode a binary-encoded transaction string.                                |
| `distribution`        | Manage distribution transactions (withdraw rewards, set commission, etc.). |
| `encode`              | Encode offline-generated transactions.                                     |
| `erc20`               | Manage ERC-20-related transactions.                                        |
| `evidence`            | Submit evidence of misbehavior.                                            |
| `evm`                 | Manage EVM-related transactions (deployment, contract calls, etc.).        |
| `feegrant`            | Feegrant transaction subcommands.                                          |
| `feemarket`           | Manage the fee market module transactions.                                 |
| `gov`                 | Governance transactions (submit proposals, vote, deposit, etc.).           |
| `ibc`                 | IBC transaction subcommands.                                               |
| `ibc-transfer`        | IBC fungible token transfer transactions.                                  |
| `interchain-accounts` | IBC interchain accounts transaction subcommands.                           |
| `multi-sign`          | Generate multisig signatures for offline-generated transactions.           |
| `multisign-batch`     | Assemble multisig transactions in batch from batch signatures.             |
| `poa`                 | Proof-of-Authority module transaction commands.                            |
| `ratelimit`           | Ratelimit module transaction commands.                                     |
| `sign`                | Sign an offline-generated transaction with a local key.                    |
| `sign-batch`          | Sign multiple transactions in batch.                                       |
| `simulate`            | Simulate a transaction to estimate gas usage.                              |
| `slashing`            | Submit slashing-related transactions.                                      |
| `staking`             | Manage staking transactions (delegate, unbond, redelegate, etc.).          |
| `upgrade`             | Submit upgrade plan transactions.                                          |
| `validate-signatures` | Validate the signatures of a transaction.                                  |

**Examples**

- **Send tokens from one account to another:**
  ```
  exrpd tx bank send <from_address> <to_address> <amounttoken> --gas auto --gas-adjustment 1.2
  ```
- **Propose a governance change:**
  ```
  exrpd tx gov submit-proposal <proposal_type> --title "<Title>" --description "<Description>" ...
  ```
- **Simulate gas usage for a transaction:**
  ```
  exrpd tx simulate --file <unsigned_tx_file>
  ```

## Version Information

#### `exrpd version`

Prints the `exrpd` application’s binary version information. This can be helpful to ensure you are on the correct version of the XRPL EVM sidechain software.

**Example**

```
exrpd version
```

## Quick Reference: Common Node CLI Tasks

This section provides a practical cheat sheet for everyday tasks when working with the `exrpd` CLI. Whether you're managing wallets, staking tokens, operating a validator, or participating in governance, these ready-to-use commands help you move fast and confidently.

---

### 🔐 Key Management

**Add New Wallet**

```bash
exrpd keys add $WALLET
```

**Restore Wallet from Mnemonic**

```bash
exrpd keys add $WALLET --recover
```

**List All Wallets**

```bash
exrpd keys list
```

**Delete a Wallet**

```bash
exrpd keys delete $WALLET
```

**Check Wallet Balance**

```bash
exrpd query bank balances $WALLET_ADDRESS
```

**Export Wallet Key (Save to File)**

```bash
exrpd keys export $WALLET > wallet.backup
```

**View EVM Private Key**

```bash
exrpd keys unsafe-export-eth-key $WALLET
```

**Import Wallet Key (Restore from Backup)**

```bash
exrpd keys import $WALLET wallet.backup
```

---

### 💰 Tokens and Staking

**Withdraw All Rewards**

```bash
exrpd tx distribution withdraw-all-rewards --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5
```

**Withdraw Rewards + Commission**

```bash
exrpd tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5 -y
```

**Delegate to Yourself**

```bash
exrpd tx staking delegate $(exrpd keys show $WALLET --bech val -a) 1000000uxrp --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5 -y
```

**Delegate to Another Validator**

```bash
exrpd tx staking delegate <TO_VALOPER_ADDRESS> 1000000uxrp --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5 -y
```

**Redelegate Stake**

```bash
exrpd tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000uxrp --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5 -y
```

**Unbond Tokens**

```bash
exrpd tx staking unbond $(exrpd keys show $WALLET --bech val -a) 1000000uxrp --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5 -y
```

**Transfer Funds**

```bash
exrpd tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000uxrp --gas auto --gas-adjustment 1.5 -y
```

---

### 🏛️ Validator Operations

**Create a New Validator**

```bash
exrpd tx staking create-validator \
--amount 1000000uxrp \
--from $WALLET \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(exrpd tendermint show-validator) \
--moniker "$MONIKER" \
--identity "" \
--details "I love blockchain ❤️" \
--chain-id exrp_1440002-1 \
--gas auto --gas-adjustment 1.5 \
-y
```

**Edit Existing Validator Info**

```bash
exrpd tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "$MONIKER" \
--identity "" \
--details "I love blockchain ❤️" \
--from $WALLET \
--chain-id exrp_1440002-1 \
--gas auto --gas-adjustment 1.5 \
-y
```

**Check Validator Status**

```bash
exrpd status 2>&1 | jq
```

**Get Validator Details**

```bash
exrpd query staking validator $(exrpd keys show $WALLET --bech val -a)
```

**Check Jailing Info**

```bash
exrpd query slashing signing-info $(exrpd tendermint show-validator)
```

**Check Slashing Parameters**

```bash
exrpd query slashing params
```

**Unjail Validator**

```bash
exrpd tx slashing unjail --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5 -y
```

**List Active Validators**

```bash
exrpd query staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**Check Validator Key Consistency**

```bash
[[ $(exrpd query staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(exrpd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

---

### 🗳️ Governance

**Submit a Text Proposal**

```bash
exrpd tx gov submit-proposal \
--title "" \
--description "" \
--deposit 1000000uxrp \
--type Text \
--from $WALLET \
--gas auto --gas-adjustment 1.5 \
-y
```

**List Proposals**

```bash
exrpd query gov proposals
```

**View a Proposal**

```bash
exrpd query gov proposal 1
```

**Vote on a Proposal**

```bash
exrpd tx gov vote 1 yes --from $WALLET --chain-id exrp_1440002-1 --gas auto --gas-adjustment 1.5
```


## Learn more

If you want to learn more about `exrpd`, you can explore the following resources:

- [Upgrading your node](./upgrading-your-node.md) — learn how to upgrade your node to the latest version.
- [Node configuration options](../advanced/node-configuration-options.md) — discover configuration options for fine-tuning your node’s.
