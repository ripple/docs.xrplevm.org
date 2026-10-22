# Join the Proof of Authority with your node

The XRPL EVM runs in a Proof of Authority consensus mechanism. In order to start signing for new blocks and participate in the network consensus, the current validators need to accept your node as a new trusted validator through a governance proposal. On Mainnet and Testnet, a proposal passes when validators holding at least 66.7% of the voting power take part (quorum) and at least 66.7% of the votes are `Yes` (80% for an expedited proposal).

To begin the process, join the [XRPL EVM Sidechain Discord](https://discord.gg/xrplevm) and select your validator role in the `#roles` channel. After that, you will need to introduce yourself in the `#become-a-validator` channel. Explain who you are and why you want to run a validator. Generally, you will be accepted if you have a real interest in the project, either because you want to use the network for a company, are a recognized member of the community who wants to contribute to its long-term governance, or just have an academic interest.

While doing your introduction, you will need to provide the details that identify your validator.

- **Moniker**: The public name of your validator
- **Validator account address**: The account address of the key that will operate your validator. It starts with _ethm1_ (not _ethmvaloper_). Can be obtained by running:

```bash
exrpd keys show <key_name> --keyring-backend <keyring_backend> -a
```

This account must not hold any `poa` tokens or delegations: the network mints the validator's voting power when the proposal executes.

- **Public key**: The consensus public key of your node. Can be obtained by running:

```bash
exrpd comet show-validator
```

After that, a governance proposal with a `MsgAddValidator` message for your address and public key is submitted and voted on over a period of 14 days on Mainnet and Testnet (1 day for an expedited proposal; Devnet uses 300 seconds, or 60 seconds when expedited). During this time, some members may write to you publicly or privately to ask more questions. You can view the process on the [XRPL EVM Governance portal](https://governance.xrplevm.org/proposals). Once the proposal passes, your node starts signing blocks with the same voting power as every other validator.

## Leaving the validator set

A validator can remove itself without a governance proposal by signing a `MsgSelfRemoveValidator` with its validator account:

```bash
exrpd tx poa self-remove-validator --from <key_name> --chain-id <chain-id> --gas auto --gas-adjustment 1.5 --gas-prices 571000000axrp
```

Validators can also be removed by governance with a `MsgRemoveValidator` proposal.
