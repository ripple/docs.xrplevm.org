# Maintaining your validator

The following page explains how to maintain your validator. This includes useful commands for editing validator metadata, confirming validator health, and performing maintenance safely.

{% admonition type="warning" name="Double-sign prevention" %}
Keep a **single active validator signer** only. Never run two active instances with the same `~/.exrpd/config/priv_validator_key.json`, and never roll back `~/.exrpd/data/priv_validator_state.json`.
{% /admonition %}

## Edit Validator Description

You can edit your validator's public description. This info identifies your validator in explorers and governance tools. Voting power on the XRPL EVM is assigned by Proof of Authority, not by delegations, so every validator has the same voting power. Flags you leave out keep their current value. <!-- SPELLING_IGNORE: delegators --><!-- STYLE_OVERRIDE: defaults to -->

The <key_name> specifies which validator you are editing. If you choose to not include certain flags, remember that the --from flag must be included to identify the validator to update.

The `--identity` can be used as to verify identity with systems like Keybase or UPort. When using with Keybase `--identity` must be populated with a 16-digit string that is generated with a [keybase.io](https://keybase.io/) account. It is a cryptographically secure method of verifying your identity across multiple online networks. The Keybase API allows us to retrieve your Keybase avatar. This is how you can add a logo to your validator profile. <!-- SPELLING_IGNORE: uport -->

```bash
exrpd tx staking edit-validator \
  --new-moniker="<your_custom_moniker>" \
  --website="https://xrpl.org" \
  --identity=6A0D65E29A4CBC8E \
  --details="<your_validator_description>" \
  --chain-id=<chain_id> \
  --gas="auto" \
  --gas-prices="571000000axrp" \
  --from=<key_name> \
  --commission-rate="0.10"
```

Note that the `commission-rate` value must adhere to the following invariants:

- Must be between 0 and the validator's `commission-max-rate`
- Must not exceed the validator's `commission-max-change-rate` which is the maximum % point change rate **per day**. In other words, a validator can only change its commission once per day and within `commission-max-change-rate` bounds.

## View Validator Description

View the validator's information with this command:

```bash
exrpd query staking validator <account>
```

## Track Validator Signing Information

To track a validator's signatures from past transactions use the `signing-info` command.

```bash
exrpd query slashing signing-info <validator-pubkey> --chain-id=<chain_id>
```

## Unjail Validator

<!-- SPELLING_IGNORE: unjail -->

A validator is jailed for 30 minutes (`downtime_jail_duration`) when it signs fewer than 50% of the blocks in a window of 88,888 blocks (`signed_blocks_window`). No tokens are slashed (`slash_fraction_downtime` is `0`), but the validator stays out of the active set until you submit an `Unjail` transaction from the operator account. Check the current values with `exrpd query slashing params`.

```bash
exrpd tx slashing unjail --from=<key_name> --chain-id=<chain_id> --gas=auto --gas-adjustment=1.5 --gas-prices=571000000axrp
```

## Confirm Your Validator is Running

Your validator is active if the following command returns anything:

```bash
exrpd query comet-validator-set | grep "$(exrpd comet show-address)"
```

You should now see your validator in one of the Exrp explorers. You are looking for the `bech32` encoded `address` in the `~/.exrpd/config/priv_validator_key.json` file. <!-- SPELLING_IGNORE: exrp -->

**Note** Validators join the set through governance (see [Join the Proof of Authority](./join-the-proof-of-authority.md)). The set is capped at 64 validators (`max_validators`).

## Halting Your Validator

When attempting to perform routine maintenance or planning for an upcoming coordinated upgrade, it can be useful to have your validator systematically and gracefully halt. Set the `halt-height` to the height at which you want your node to shut down, or pass the `--halt-height` flag to `exrpd`. The node shuts down with a 0 exit code at that given height after committing the block.

## Secure maintenance checklist (validators)

Use this checklist before restarting a validator:

1. Stop the node process (`systemctl`, `cosmovisor`, or `docker`) and confirm no active validator signer `exrpd` process remains.
2. Confirm no second host/container is running as an active validator signer with the same validator key.
3. Back up signer files:
   - `~/.exrpd/config/priv_validator_key.json`
   - `~/.exrpd/data/priv_validator_state.json`
4. Perform maintenance or upgrade.
5. Start exactly one signer instance and monitor logs.

For full upgrade runbook steps, see [Upgrading your node](../guides/upgrading-your-node.md).
