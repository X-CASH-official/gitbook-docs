---
description: >-
  Use X-Cash Blockchain Import (xcash-blockchain-import) to import a raw file of
  the blockchain
---

# xcash-blockchain-import

### Overview <a id="overview"></a>

The tool imports a bootstrap file `blockchain.raw` to your full node to be used as the`lmdb` blockchain.

This could be useful if you want to decouple download from verification for any reason \(like testing performance in isolation\), or if you have a limited speed connection. Updated version of the `blockchain.raw` file can be found on X-Cash communities.

For best practices, you should use your own trusted `blockchain.raw` file that you exported earlier.

Also note that importing `blockchain.raw` will in most cases not speed up the process over syncing up from p2p network. This is because usual bottlenecks are disk IO and verification, not the download itself.

The tool works on your local files and does not require `xcash` running.

## Syntax

`./xcash-blockchain-import [options]`

**Example:**

`./xcash-blockchain-import --help`

## Running

Go to directory where you unpacked X-Cash.

`./xcash-blockchain-import --stagenet --input-file=/tmp/blockchain.raw`

## Options

### Help

| Option | Description |
| :--- | :--- |
| `--help` | Enlist available options. |

### Pick network

| Option | Description |
| :--- | :--- |
| \(missing\) | By default `xcash-blockchain-import` assumes the mainnet blockchain. |
| `--stagenet` | Import stagenet blockchain. |
| `--testnet` | Import testnet blockchain. |

### Logging

#### Specifying the log file path is not supported

| Option | Description |
| :--- | :--- |
| `--log-level` | `0-4` with `0` being minimal logging and `4` being full tracing. Defaults to `0`. These are general presets and do not directly map to severity levels. For example, even with minimal `0`, you may see some most important `INFO` entries. Example:  `./xcash-blockchain-import --log-level=1` |

### Input

| Option | Description |
| :--- | :--- |
| `--data-dir` | Full path to data directory. This is where the blockchain, log files, and p2p network memory are stored. For defaults and details see data directory. |
| `--count-blocks` | Count blocks in the bootstrap file and exit. |
| `--drop-hard-fork` | Whether to drop hard fork data. Off by default \(`0`\). |
| `--database` | The only valid value seems to be `lmdb` \(the default\). |

### Output

| Option | Description |
| :--- | :--- |
| `--data-dir` | Full path to data directory. This is where the blockchain, log files, and p2p network memory are stored. For defaults and details see data directory. |
| `--count-blocks` | Count blocks in the bootstrap file and exit. |
| `--drop-hard-fork` | Whether to drop hard fork data. Off by default \(`0`\). |
| `--database` |  |

### Performance

| Option | Description |
| :--- | :--- |
| `--dangerous-unverified-import` | The safe default is to run verification \(value `0`\). You can enable `--dangerous-unverified-import` if you are importing from your own and trusted blockchain.raw \(which we assume was already verified\). The "dangerous" mode will greatly speed up the process. |
| `--batch` | Whether to save to disk on an ongoing basis \(the default, value `1`\) or maybe do everything in RAM and save everything in the end \(value `0`\). No batching is only effective in combination with no verification \(`--dangerous-unverified-import`\). See also `--batch-size`. |
| `--batch-size` | How often to save to disk expressed in number of blocks. By default save every `5000` blocks \(when verifying\) or every `20000` blocks \(when not verifying\). Big batches are faster but require more RAM. |
| `--resume` | Resume from current height if output database already exists \(the default, value `1`\). Changing to `--resume=0` doesn't change much — existing blocks are skipped pretty quickly and the process is resumed anyway. |

