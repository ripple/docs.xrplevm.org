# Snapshots

Snapshots provide a quick way to bootstrap your node’s state by downloading a pre-synchronized version of the blockchain data. This reduces the time and resources required to sync from the genesis block. Providers currently publish Mainnet, Testnet and Devnet snapshots for various use cases. Some snapshots are pruned (minimal historical data), others retain default state, and some include full historical archives for comprehensive analysis.

## Snapshots

{% tabs %}

{% tab label="Mainnet" %}

| Provider     | URL                                                                                                  |
| ------------ | ---------------------------------------------------------------------------------------------------- |
| **Peersyst** | [Download Mainnet](https://xrpl-evm-snapshots.s3.us-east-1.amazonaws.com/mainnet.tar.lz4) |
| **Polkachu** | [Download Mainnet](https://polkachu.com/tendermint_snapshots/xrp)                                    |
| **Cumulo**   | [Download Mainnet](https://cumulo.pro/services/xrplevm_mainnet/snapshot)                             |
| **ITRocket** | [Download Mainnet](https://itrocket.net/services/mainnet/xrplevm/)                                   |

{% /tab %}

{% tab label="Testnet" %}

| Provider     | URL                                                                                                  |
| ------------ | ---------------------------------------------------------------------------------------------------- |
| **Peersyst** | [Download Testnet](https://xrpl-evm-snapshots.s3.us-east-1.amazonaws.com/testnet.tar.lz4) |
| **Polkachu** | [Download Testnet](https://polkachu.com/testnets/xrp/snapshots)                                      |
| **Cumulo**   | [Download Testnet](https://cumulo.pro/services/xrplevm/)                                             |
| **ITRocket** | [Download Testnet](https://itrocket.net/services/testnet/xrplevm/)                                   |

{% /tab %}

{% tab label="Devnet" %}

| Provider     | URL                                                            |
| ------------ | -------------------------------------------------------------- |
| **Peersyst** | [Download Devnet](https://xrpl-evm-snapshots.s3.us-east-1.amazonaws.com/devnet.tar.lz4) |

Peersyst is currently the only provider publishing Devnet snapshots. Archives that predate the January 2026 Devnet reset are not compatible with the current chain, so do not reuse older Devnet downloads.

{% /tab %}

{% /tabs %}

**Note:** Always verify the authenticity and integrity of snapshots before using them. Keep in mind that third-party providers may have different update schedules, data policies, and retention periods. Refer to the provider’s documentation for instructions on how to use these snapshots and any associated terms of service.

## Restore examples

Use the command set that matches the provider archive format.

### Peersyst (`.tar.lz4`)

```bash
cd ~/.exrpd
wget -O snapshot.tar.lz4 <snapshot_url>
tar -xI lz4 -f snapshot.tar.lz4
```

### Cumulo (`.tar.zst`)

```bash
sudo apt update && sudo apt install -y aria2 zstd curl

SERVICE=exrpd.service
STATE=~/.exrpd/data/priv_validator_state.json
BACKUP=/tmp/priv_validator_state.json

sudo systemctl stop $SERVICE

if [ -f "$STATE" ]; then
	cp "$STATE" "$BACKUP"
fi

rm -rf ~/.exrpd/data && mkdir -p ~/.exrpd/data
cd ~/.exrpd
aria2c -x 16 -s 16 -k 1M --file-allocation=none -o snapshot.tar.zst "https://xrpl.cumulo.com.es/snapshots/latest_xrpl_snapshot.tar.zst"
zstd -t snapshot.tar.zst
tar --use-compress-program=zstd -xf snapshot.tar.zst -C ~/.exrpd/data
rm -f snapshot.tar.zst

if [ -f "$BACKUP" ]; then
	mv "$BACKUP" "$STATE"
fi

sudo systemctl start $SERVICE
```

{% admonition type="warning" name="Validator signer safety" %}
Restore `priv_validator_state.json` after extraction when replacing data, and keep only one active validator signer for the same `priv_validator_key.json`.
{% /admonition %}

If integrity check fails (`zstd -t` or tar extraction errors such as `Unexpected EOF`), delete the archive and download it again before extracting.
