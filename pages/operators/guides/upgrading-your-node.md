# Upgrading your node

Blockchain upgrades are a complex process that requires all network participants to agree on a specific time to execute a specific change in the protocol. Cosmos based networks, like the XRPL EVM Sidechain, solve this complexity with a set of fixed operations that need to happen when upgrading the chain:

1. A governance proposal containing a chain upgrade transaction is created. This transaction contains specific details of the upgrade such as which block will the upgrade be executed, the version number and some metadata.
2. Once the proposal is created it is voted as another regular governance proposal.
3. If the proposal collects enough votes to be approved, the chain upgrade is planned at the block specified.
4. When the block of the upgrade is minted, all the nodes, that are running the old version will halt, showing an error in the logs notifying the operator to upgrade it to the new binary.
5. After more than 2/3 of the total validators upgrade their nodes, the chain starts creating new blocks. And the upgrade has finished successfully.

{% admonition type="info" name="List of upgrades" %}
In the [Networks](../resources/networks.md) page, you can find all the hard fork upgrades for each network
{% /admonition %}

## Automated upgrades

Manual actions required by node operators during the chain upgrades can be automated using Cosmovisor. This client automatically detects new binaries for newer versions available and downloads them. Once a new chain upgrade is triggered, the cosmovisor restarts the node with the new binary, reducing the downtime of your node.

{% admonition type="info" name="Important" %}
The following instructions are adapted from the official Cosmovisor documentation. If you want more in-depth information or advanced usage, consult that resource.
{% /admonition %}

### Install Cosmovisor

You can download Cosmovisor from the [GitHub releases](https://github.com/cosmos/cosmos-sdk/releases/tag/cosmovisor%2Fv1.5.0).

To install the latest version of cosmovisor, run the following command:

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

To install a specific version, you can specify the version:

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```

Run cosmovisor version to check the cosmovisor version.

```bash
cosmovisor version
```

### Directory Structure

Cosmovisor requires a specific directory layout to manage your XRPL EVM sidechain binaries. The recommended structure is:

```
~/.exrpd/
└── cosmovisor
    ├── current -> genesis (symlink)
    ├── genesis
    │   └── bin
    │       └── exrpd (the current binary)
    └── upgrades
        └── <upgrade-name>
            └── bin
                └── exrpd (new binary for the upgrade)
```

- `genesis/bin/exrpd` is your current XRPL EVM sidechain binary (the one you’re running).
- `upgrades/<upgrade-name>/bin/exrpd` is where you place the upgraded binary once an on-chain upgrade is approved.

### Configuration

Cosmovisor uses environment variables to locate directories and manage behavior. The most important variables are:

- `DAEMON_NAME`  
  The name of the binary that Cosmovisor will run (e.g., `exrpd`).

- `DAEMON_HOME`  
  The path to the home directory for your chain (e.g., `~/.exrpd`).

- `DAEMON_DATA_BACKUP_DIR`  
  (Optional) A directory where Cosmovisor will store backups of data.

- `UNSAFE_SKIP_BACKUP`  
  Set to `true` or `false` depending on whether you want Cosmovisor to skip making data backups.

- `DAEMON_RESTART_AFTER_UPGRADE`  
  If `true`, Cosmovisor will automatically restart the node after the upgrade.

- `DAEMON_ALLOW_DOWNLOAD_BINARIES`  
  If `true`, Cosmovisor will attempt to download and install the binary itself based on the instructions in the info attribute in the data/upgrade-info.json file.

A typical environment configuration might look like this:

```bash
export DAEMON_NAME=exrpd
export DAEMON_HOME=$HOME/.exrpd
export DAEMON_RESTART_AFTER_UPGRADE=true
export UNSAFE_SKIP_BACKUP=false
export DAEMON_ALLOW_DOWNLOAD_BINARIES=true
```

### Running the Node with Cosmovisor

You can start the node with Cosmovisor directly from the command line:

```bash
cosmovisor start
```

This command uses `$DAEMON_NAME` (in this case, `exrpd`) and `$DAEMON_HOME` to locate your sidechain binary and configuration.

### Performing Upgrades

When an upgrade is triggered on-chain (via governance or other mechanisms), you need to:

1. Download or build the **new** XRPL EVM sidechain binary.
2. Create a subdirectory in `~/.exrpd/cosmovisor/upgrades/` corresponding to the upgrade name.
3. Place the new binary inside a `bin/` folder within that subdirectory.
4. Cosmovisor will detect the upgrade at the appointed block height and automatically switch to the new binary.

#### Example Upgrade Directory

If the upgrade name is `v5`, the new binary should be placed at:

```
~/.exprd/cosmovisor/upgrades/v5/bin/exrpd
```

Make sure to give execute permissions to the new binary:

```bash
chmod +x ~/.exrpd/cosmovisor/upgrades/v5/bin/exrpd
```

### Automated downloads

Generally, cosmovisor requires that the system administrator place all relevant binaries on disk before the upgrade happens. However, for people who don't need such control and want an automated setup (maybe they are syncing a non-validating fullnode and want to do little maintenance), there is another option.

{% admonition type="warning" name="Security note" %}
We don't recommend using auto-download because it doesn't verify in advance if a binary is available. If there will be any issue with downloading a binary, the cosmovisor will stop and won't restart an App (which could lead to a chain halt).
{% /admonition %}

If `DAEMON_ALLOW_DOWNLOAD_BINARIES` is set to `true`, and no local binary can be found when an upgrade is triggered, cosmovisor will attempt to download and install the binary itself based on the instructions in the info attribute in the data/upgrade-info.json file. The files is constructed by the x/upgrade module and contains data from the upgrade Plan object.

## Additional Resources

- [Cosmovisor Docs (Cosmos SDK)](https://docs.cosmos.network/main/build/tooling/cosmovisor)
- [XRPL EVM Sidechain Repository](https://github.com/xrplevm/node)

---

**That’s it!** With Cosmovisor set up, your XRPL EVM sidechain node will automatically update to new binaries when an on-chain upgrade proposal is executed, reducing manual intervention and downtime.
