---
description: >-
  Use X-Cash Blockchain Export (xcash-blockchain-export) to create a raw file of
  the blockchain
---

# Export blockchain file

## Overview

This tool creates an export of the local blockchain version to a raw format.

This could be useful if you want to process blockchain efficiently with your custom tools, as the raw format is probably easier to work with than X-Cash's custom lmdb database. This can also be used to share the blockchain to others allowing them to download it quicker.

The tool works on your local copy of the blockchain and does not require `xcashd` running.

## Syntax

`./xcash-blockchain-export [options]`

**Example:**

`./xcash-blockchain-export --help`

## Running

**Go to directory where you unpacked X-Cash.**

`./xcash-blockchain-export --stagenet --output-file=/tmp/blockchain.raw`

## Options

### **Help**

| **Option** | Description |
| :--- | :--- |
| `--help` | Enlist available options. |

#### Pick network <a id="pick-network"></a>

| Option | Description |
| :--- | :--- |
| \(missing\) | By default `xcash-blockchain-export` assumes mainnet. |
| `--stagenet` | Export stagenet blockchain. |
| `--testnet` | Export testnet blockchain. |

### Logging

**Specifying the log file path is not supported.**

| **Option** | Description |
| :--- | :--- |
| `--log-level` | `0-4` with `0` being minimal logging and `4` being full tracing. Defaults to `0`. These are general presets and do not directly map to severity levels. For example, even with minimal `0`, you may see some most important `INFO` entries. Example:  `./xcash-blockchain-export --log-level=1` |

### Input

| Option | Description |
| :--- | :--- |
| `--data-dir` | Full path to data directory. This is where the blockchain, log files, and p2p network memory are stored. For defaults and details see data directory. |
| `--database`, `--db-type` | The default and only valid value is `lmdb`. |

### Output

| Option | Description |
| :--- | :--- |
| `--output-file` | Specify output file path. The default is `$DATA_DIR/export/blockchain.raw`. Example:  `./xcash-blockchain-export --output-file=/tmp/blockchain.raw` |
| `--blocksdat` | Output in blocks.dat format. |
| `--block-stop` | Only export up to this block number. By default do the full export \(value `0`\). |

