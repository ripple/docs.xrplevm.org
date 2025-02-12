# Join the Proof of Authority with your node

The XRPL EVM runs in a Proof of Authority consensus mechanism. In order to start signing for new blocks and participate in the network consensus, the current validators need to accept your node as a new trusted validator. This democratic process requires the approval of the majority of the current validators.

To begin the process, join the [XRPL EVM Sidechain Discord](https://discord.gg/xrplevm) and select your validator role in the `#roles` channel. After that, you will need to introduce yourself in the `#become-a-validator` channel. Explain who you are and why you want to run a validator. Generally, you will be accepted if you have a real interest in the project, either because you want to use the network for a company, are a recognized member of the community who wants to contribute to its long-term governance, or just have an academic interest.

While doing your introduction, you will need to provide the details that identify your validator.

- **Moniker**: The public name of your validator
- **Validator operator address**: The address of the operator of the node that starts with _ethmvaloper_ . Can be obtained by running:

```bash
exrpd keys show <key_name> --keyring <keying_backend> --bech val
```

- **Public key**: The public key of your node. Can be obtained by running:

```bash
exrpd tendermint show-validator
```

After that, a proposal to accept your validator will be voted on over a period of 7 days. During this time, some members may write to you publicly or privately to ask more questions. You can view the process on the [XRPL EVM Sidechain Explorer](https://governance.xrplevm.org/xrp/proposals).
