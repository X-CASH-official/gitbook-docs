---
description: >-
  Use the X-Cash Daemon (xcashd) to create a node, sync the blockchain and relay
  transactions.
---

# Interacting with the network

## Overview

#### Syncing the blockchain <a id="connects-you-to-monero-network"></a>

The X-Cash daemon `xcashd` keeps your computer synced up with the X-Cash network.

It downloads and validates the blockchain from the other nodes.

![Overview of the daemon syncing and checking the blockchain by 100 blocks chunks](../.gitbook/assets/image%20%282%29.png)

#### About privacy and best practices <a id="not-aware-of-your-private-keys"></a>

`xcashd` is entirely decoupled from your wallet.

`xcashd` does not access your private keys and therefore doesn't know your transactions and balance.

This means that you can run `xcashd`on a separate computer or in the cloud and access it remotely to retrieve/provide the data needed for wallet operations.

In practice, you can connect to a remote`xcashd`instance provided by a semi-trusted 3rd party. Such 3rd party will not be able to steal your funds as all transactions are signed locally.

However, there are privacy and reliability implications when using a remote, untrusted node. For any real business we recommend running your own full node.

To provide a better reliability and trust, the team has setup a several public remote nodes across different locations:

| Name | Location |
| :--- | :--- |
| **usseed1.x-cash.org:18281** | US - Oregon |
| **usseed2.x-cash.org:18281** | US - Virginia |
| **euseed1.x-cash.org:18281** | Europe - Germany |
| **euseed3.x-cash.org:18281** | Europe - France |
| **asiaseed2.x-cash.org:18281** | Asia - Singapore |

## Syntax

`./xcashd [options] [command]`

Options define how the daemon should be working and are optional. Their names follow the `--option-name` convention.

Commands give access to specific services provided by the daemon. Commands are executed against the running daemon. Their names follow the `command_name` convention.

## Running

Go to directory where you unpacked X-Cash.

For learning and experimentation, you should stick to the testnet to avoid any wrong manipulation.

```bash
./xcashd --stagenet --detach                # run as a daemon in background
./xcashd --stagenet exit                    # ask daemon to exit gracefully
```

By default, the daemon connects to the mainnet. If you want to use real XCASH there is no need to add a specific argument.

```bash
./xcashd --detach                           # run as a daemon in background
./xcashd exit                               # ask daemon to exit gracefully
```

## Options

Options define how the daemon should be working. Their names follow the `--option-name`convention.

The following groups are only to make reference easier to follow. The daemon itself does not group options in any way.

### **Help and version**

| **Option** | Description |
| :--- | :--- |
| `--help` | Enlist available options. |
| `--version` | Show `xcashd` version to stdout. Example:  `X-CASH '' (v1.5.0-unknown)` |
| `--os-version` | Show build timestamp and target operating system. Example output: `OS: Microsoft (build 17763), 64-bit` |

### **Pick network**

| **Option** | Description |
| :--- | :--- |
| \(missing\) | By default xcashd assumes mainnet. |
| `--stagenet` | Run on stagenet. Remember to run your wallet with `--stagenet` as well. |
| `--testnet` | Run on testnet. Remember to run your wallet with `--testnet` as well. |

### **Logging**

| **Option** | Description |
| :--- | :--- |
| `--log-file` | Full path to the log file. Example \(make sure your file permissions are correctly setup\):  `./xcashd --log-file=/var/log/xcash/mainnet/xcash.log` |
| `--log-level` | `0-4` with `0` being minimal logging and `4` being full tracing. Defaults to `0`. These are general presets and do not directly map to severity levels. For example, even with minimal `0`, you may see some most important `INFO` entries. Temporarily changing to `1` allows for much better understanding of how the full node operates. Example:  `./xcashd --log-level=1` |
| `--max-log-file-size` | Soft limit in bytes for the log file \(=104850000 by default, which is just under 100MB\). Once log file grows past that limit, `xcashd` creates the next log file with a UTC timestamp postfix `-YYYY-MM-DD-HH-MM-SS`.  In production deployments, you would probably prefer to use established solutions like logrotate instead. In that case, set `--max-log-file-size=0` to prevent xcashd from managing the log files. |
| `--max-log-files` | Limit on the number of log files \(=50 by default\). The oldest log files are removed. In production deployments, you would probably prefer to use established solutions like logrotate instead. |

### **Server**

`xcashd` defaults are adjusted for running it occasionally on the same computer as your X-Cash wallet.

The following options will be helpful if you intend to have an always running node — most likely on a remote server or your own separate PC.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>--config-file</code>
      </td>
      <td style="text-align:left">Full path to the configuration file. By default xcashd looks for bitxcash.conf
        in X-Cash data directory.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--data-dir</code>
      </td>
      <td style="text-align:left">Full path to data directory. This is where the blockchain, log files,
        and p2p network memory are stored. For defaults and details see data directory.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--pidfile</code>
      </td>
      <td style="text-align:left">
        <p>Full path to the PID file. Works only with <code>--detach</code>.</p>
        <p><b>Example: </b><code>./xcashd --detach --pidfile=/run/xcash/xcashd.pid</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--detach</code>
      </td>
      <td style="text-align:left">Go to background (decouple from the terminal). This is useful for long-running
        / server scenarios. Typically, you will also want to manage <code>xcashd</code> daemon
        with systemd or similar. By default <code>xcashd</code> runs in a foreground.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--non-interactive</code>
      </td>
      <td style="text-align:left">Do not require tty in a foreground mode. Helpful when running in a container.
        By default <code>xcashd</code> runs in a foreground and opens stdin for reading.
        This breaks containerization because no tty gets assigned and <code>xcashd</code> process
        crashes. You can make it run in a background with <code>--detach</code> but
        this is inconvenient in a containerized environment because the canonical
        usage is that the container waits on the main process to exist (forking
        makes things more complicated).</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--no-igd</code>
      </td>
      <td style="text-align:left">Disable UPnP port mapping on the router (&quot;Internet Gateway Device&quot;).
        Add this option to improve security if you are not behind a NAT (you can
        bind directly to public IP or you run through Tor).</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--max-txpool-weight</code>
      </td>
      <td style="text-align:left">Set maximum transactions pool size in bytes. By default 648000000 (~618MB).
        These are transactions pending for confirmations (not included in any block).</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--enforce-dns-checkpointing</code>
      </td>
      <td style="text-align:left">
        <p>The emergency checkpoints set by X-CashPulse operators will be enforced.
          It is probably a good idea to set enforcing for unattended nodes.</p>
        <p>If encountered block hash does not match corresponding checkpoint, the
          local blockchain will be rolled back a few blocks, effectively blocking
          following what X-CashPulse operators consider invalid fork.</p>
        <p>The log entry will be produced: <code>ERROR Local blockchain failed to pass a checkpoint, rolling back!</code> Eventually,
          the alternative (&quot;fixed&quot;) fork will get heavier and the node
          will follow it, leaving the &quot;invalid&quot; fork behind.</p>
        <p>By default checkpointing only notifies about discrepancy by producing
          the following log entry: <code>ERRORWARNING: local blockchain failed to pass a xcashPulse checkpoint, and you could be on a fork. You should either sync up from scratch, OR download a fresh blockchain bootstrap, OR enable checkpoint enforcing with the --enforce-dns-checkpointing command-line option</code>  <b>As of 04/09/2019, X-Cash has not implemented X-CashPulse operators.</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--disable-dns-checkpoints</code>
      </td>
      <td style="text-align:left">
        <p>The X-CashPulse checkpoints set by core developers will be discarded.</p>
        <p><b>As of 04/09/2019, X-Cash has not implemented the X-CashPulse operators feature.</b>
        </p>
      </td>
    </tr>
  </tbody>
</table>

The following options define how your node participates in X-Cash peer-to-peer network. This is for node-to-node communication. The following options do **not** affect wallet-to-node interface.

The node and peer words are used interchangeably.

| **Option** | Description |
| :--- | :--- |
| `--p2p-bind-ip` | Network interface to bind to for p2p network protocol. Default value `0.0.0.0` binds to all network interfaces and should be used in general working conditions.  You must change this if you want to constrain binding, for example to configure connection through Tor via torsocks:  `DNS_PUBLIC=tcp://1.1.1.1 TORSOCKS_ALLOW_INBOUND=1 torsocks ./xcashd --p2p-bind-ip 127.0.0.1 --no-igd --hide-my-port` |
| `--p2p-bind-port` | TCP port to listen for p2p network connections. Defaults to `18281` for mainnet, `28281` for testnet, and `38281` for stagenet. You normally wouldn't change that. This is helpful to run several nodes on your machine to simulate private a X-Cash p2p network \(likely using private testnet\). Example: `./xcashd --p2p-bind-port=48080` |
| `--p2p-external-port` | TCP port to listen for p2p network connections on your router. Relevant if you are behind a NAT and still want to accept incoming connections. You must then set this to relevant port on your router. This is to let `xcashd` know what to advertise on the network. By default this value is `0`. |
| `--hide-my-port` | `xcashd` will still open and listen on the p2p port. However, it will not announce itself as a peer list candidate. Technically, it will return port `0` in a response to p2p handshake \(`node_data.my_port = 0` in `get_local_node_data` function\). In effect nodes you connect to won't spread your IP to other nodes. To sum up, it is not really hiding, it is more like "do not advertise". |
| `--seed-node` | Connect to a node to retrieve other nodes' addresses, and disconnect. If not specified, `xcashd` will use hardcoded seed nodes on the first run, and peers cached on disk on subsequent runs. |
| `--add-peer` | Manually add node to local peer list. |
| `--add-priority-node` | Specify list of nodes to connect to and then attempt to keep the connection open.   To add multiple nodes use the option several times. Example:  `./xcashd --add-priority-node=178.128.192.138:18081 --add-priority-node=144.76.202.167:18081` |
| `--add-exclusive-node` | Specify list of nodes to connect to only. If this option is given the options `--add-priority-node` and `--seed-node` are ignored.   To add multiple nodes use the option several times. Example:  `./xcashd --add-exclusive-node=178.128.192.138:18081 --add-exclusive-node=144.76.202.167:18081` |
| `--out-peers` | Set max number of outgoing connections to other nodes. By default 8. Value `-1`represents the code default. |
| `--in-peers` | Set max number of incoming connections \(nodes actively connecting to you\). By default unlimited. Value `-1` represents the code default. |
| `--limit-rate-up` | Set outgoing data transfer limit \[kB/s\]. By default 2048 kB/s. Value `-1` represents the code default. |
| `--limit-rate-down` | Set incoming data transfer limit \[kB/s\]. By default 8192 kB/s. Value `-1` represents the code default. |
| `--limit-rate` | Set the same limit value for incoming and outgoing data transfer. By default \(`-1`\) the individual up/down default limits will be used. It is better to use `--limit-rate-up`and `--limit-rate-down` instead to avoid confusion. |
| `--offline` | Do not listen for peers, nor connect to any. Useful for working with a local, archival blockchain. |
| `--allow-local-ip` | Allow adding local IP to peer list. Useful mostly for debug purposes when you may want to have multiple nodes on a single machine. |

### **Node RPC API**

`xcashd` node offers a powerful API. It serves 3 purposes:

* provides network data \(stats, blocks, transactions, ...\)
* provides local node information \(peer list, hash rate if mining, ...\)
* provides interface for wallets \(send transactions, ...\)

This API is typically referred to as "RPC" because it is mostly based on JSON/RPC standard.

The following options define how the API behaves.

| **Option** | Description |
| :--- | :--- |
| `--rpc-bind-ip` | IP to listen on. By default `127.0.0.1` because API gives full administrative capabilities over the node. Set it to `0.0.0.0` to listen on all interfaces - but only in connection with one of `*-restricted-*` options **and** `--confirm-external-bind`. |
| `--rpc-bind-port` | TCP port to listen on. By default `18281` \(mainnet\), `28281` \(testnet\), `38281`\(stagenet\). |
| `--rpc-restricted-bind-port` | TCP port to listen on with the limited version of API. The limited API can be made public to create an Open Node. At the same time, you may firewall the full API port to still enjoy local querying and administration. |
| `--confirm-external-bind` | Confirm you consciously set `--rpc-bind-ip` to non-localhost IP and you understand the consequences. |
| `--restricted-rpc` | Restrict API to view only commands and do not return privacy sensitive data. Note this does not make sense with `--rpc-restricted-bind-port`because you would end up with two restricted APIs. |
| `--rpc-login` | Specify `username[:password]` required to connect to API. Practical usage is limited because API communication is in plain text over HTTP. |
| `--rpc-access-control-origins` | Specify a comma separated list of origins to allow cross origin resource sharing. This is useful if you want to use `xcashd` API directly from a web browser via JavaScript \(say in a pure-fronted web appp scenario\). With this option `xcashd` will put proper HTTP CORS headers to its responses. You will also need to set `--rpc-login` if you use this option. Normally though, the API is used by backend app and this option isn't necessary. |

### **Accepting X-Cash**

These are network notifications offered by `xcashd`. There are also wallet notifications like `--tx-notify` offered by `xcashd-wallet-rpc` here.

| **Option** | Description |
| :--- | :--- |
| `--block-notify` | Run a program for each new block. The `<arg>` must be a **full path**. If the `<arg>`contains `%s` it will be replaced by the block hash. Example:  `./xcashd --block-notify="/usr/bin/echo %s"`  Block notifications are good for immediate reaction. However, you should always assume you will miss some block notifications and you should independently poll the API to cover this up.   Mind blockchain reorganizations. Block notifications can revert to same and past heights. Small reorganizations are natural and happen every day. |
| `--block-rate-notify` | Run a program when the number of blocks received in the recent past deviates significantly from the expectation. The `<arg>` must be a **full path**. The `<arg`&gt; can contain any of `%t`, `%b`, `%e` symbols to interpolate:   `%t`: the number of minutes in the observation window  `%b`: the number of blocks observed in that window  `%e`: the ideal number of blocks expected in that window  The option will let you know if the network hash rate drops by a lot. This may be indicative of a large section of the network miners moving off to mine a private chain, to be later released to the network. Note that if this event triggers, it is not incontrovertible proof that this is happening. It might just be chance. The longer the window \(the %t parameter\), and the larger the distance between actual and expected number of blocks, the more indicative it is of a possible chain reorg double-spend attack being prepared.  **Recommendation:** unless you run economically significant X-Cash exchange or operation, do **not** act on this data. It is hard to calibrate and easy to misinterpret. If this is a real attack, it will target high-liquidity entities and not small merchants. |
| `--reorg-notify` | Run a program when reorganization happens \(ie, at least one block is removed from the top of the blockchain\). The `<arg>` must be a **full path**. The `<arg`&gt; can contain any of `%s`, `%h`, `%n` symbols to interpolate:   `%s`: the height at which the split occurs   `%h`: the height of the new blockchain  `%d`: the number of blocks discarded from the old chain   `%n`: the number of blocks being added   The option will let you know when a block is removed from the chain to be replaced by other blocks. This happens when a 51% attack occurs, but small reorgs also happen in the normal course of things. The `%d` parameter will be set to the number of blocks discarded from the old chain \(so if this is higher than the number of confirmations you wait to act upon an incoming payment, that payment might have been cancelled\). The `%n` parameter wil be set to the number of blocks in the new chain \(so if this is higher than the number of confirmations you wait to act upon an incoming payment, any incoming payment in the first block will be automatically acted upon by your platform\).   **Recommendation**: unless you run economically significant X-Cash exchange or operation, you do **not** need to bother with this option. Simply account for reorganizations by requiring at least 10 confirmations to consider payments no reversible. |

### **Performance**

These are advanced options that allow you to optimize performance of your `xcashd` node, sometimes at the expense of reliability.

| Option | Description |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `--db-sync-mode` | Specify sync option, using format: \`\[safe fast |  | fast | fastest\]:\[sync | async\]:\[\[blocks\] | \[bytes\]\]`The default is`fast:async:250000000bytes`.  The`fast:async:\*`can corrupt blockchain database in case of a system crash. It should not corrupt if just`xcashd`crashes. If you are concerned with system crashes use`safe:sync\`. |
| `--max-concurrency` | Max number of threads to use for parallel jobs. The default value `0` uses the number of CPU threads. |  |  |  |  |  |
| `--prep-blocks-threads` | Sync up most of the way by using embedded, "known" block hashes. Pass `1`to turn on and `0` to turn off. This is on \(`1`\) by default. Normally, for every block the full node must calculate the block hash to verify miner's proof of work. Because the CryptoNight PoW used in X-Cash is very expensive \(even for verification\), `xcashd` offers skipping these calculations for old blocks. In other words, it's a mechanism to trust `xcashd` binary regarding old blocks' PoW validity, to sync up faster. |  |  |  |  |  |
| `--block-sync-size` | How many blocks are processed in a single batch during chain synchronization. By default this is 20 blocks for newer history and 100 blocks for older history \("pre v4"\). Default behavior is represented by value `0`. Intuitively, the more resources you have, the bigger batch size you may want to try out. Example: `./xcashd --block-sync-size=500` |  |  |  |  |  |
| `--bootstrap-daemon-address` | The host:port of a "bootstrap" remote open node that the connected wallets can use while this node is still not fully synced. Example: `./xcashd --bootstrap-daemon-address=opennode.xmr-tw.org:18089`. The node will forward selected RPC calls to the bootstrap node. The wallet will handle this automatically and transparently. Obviously, such bootstraping phase has privacy implications similar to directly using a remote node. |  |  |  |  |  |
| `--bootstrap-daemon-login` | Specify username:password for the bootstrap daemon login \(if required\). This considers the RPC interface used by the wallet. Normally, open nodes do not require any credentials. |  |  |  |  |  |

### **Mining**

{% hint style="danger" %}
Mining functions will not work anymore under X-Cash 2.0
{% endhint %}

The following options configure **solo mining** using **CPU** with the standard software stack `xcashd`. This is mostly useful for:

* generating your stagenet or testnet coins
* experimentation and learning
* if you have super cheap access to vast CPU resources

Be advised though that real mining happens **in pools** and with high-end **GPU-s** instead of CPU-s.  
Note: On block `281000`, X-Cash switched to Cryptonight HeavyX algorithm which is ASIC and NiceHash resistant.

| **Option** | Description |
| :--- | :--- |
| `--start-mining` | Specify wallet address to mining for. **This must be a standard address!** It can be neither a subaddres nor integrated address. |
| `--mining-threads` | Specify mining threads count. By default ony one thread will be used. For best results, set it to number of your physical cores. |
| `--extra-messages-file` | Specify file for extra messages to include into coinbase transactions. |
| `--bg-mining-enable` | Enable unobtrusive mining. In this mode mining will use a small percentage of your system resources to never noticeably slow down your computer. This is intended to encourage people to mine to improve decentralization. That being said chances of finding a block are diminishingly small with solo CPU mining, and even lesser with its unobtrusive version. You can tweak the unobtrusivness / power trade-offs with the further `--bg-*`options below. |
| `--bg-mining-ignore-battery` | If true, assumes plugged in when unable to query system power status. |
| `--bg-mining-min-idle-interval` | Specify min lookback interval in seconds for determining idle state. |
| `--bg-mining-idle-threshold` | Specify minimum avg idle percentage over lookback interval. |
| `--bg-mining-miner-target` | Specify maximum percentage cpu use by miner\(s\). |

### **Testing X-Cash**

These options are useful for X-Cash project developers and testers. Normal users shouldn't be concerned with these.

| **Option** | Description |
| :--- | :--- |
| `--test-drop-download` | For net tests: in download, discard ALL blocks instead checking/saving them \(very fast\). |
| `--test-drop-download-height` | Like test-drop-download but discards only after around certain height. By default `0`. |
| `--regtest` | Run in a regression testing mode. |
| `--fixed-difficulty` | Fixed difficulty used for testing. By default `0`. |
| `--test-dbg-lock-sleep` | Sleep time in ms, defaults to 0 \(off\), used to debug before/after locking mutex. Values 100 to 1000 are good for tests. |
| `--save-graph` | Save data for dr X-Cash. |

### **Legacy**

These options should no longer be necessary but are still present in `xcashd` for backwards compatibility.

| **Option** | Description |
| :--- | :--- |
| `--fluffy-blocks` | Relay compact blocks. Default. Compact block is just a header and a list of transaction IDs. |
| `--no-fluffy-blocks` | Relay classic full blocks. Classic block contains all transactions |
| `--show-time-stats` | Official docs say "Show time-stats when processing blocks/txs and disk synchronization" but it does not seem to produce any output during usual blockchain synchronization. |
| `--zmq-rpc-bind-ip` | IP for ZMQ RPC server to listen on. By default `127.0.0.1`. This is not yet widely used as ZMQ interface currently does not provide meaningful advantage over classic JSON-RPC interface. Unfortunately, currently there is no way to disable the ZMQ server. |
| `--zmq-rpc-bind-port` | Port for ZMQ RPC server to listen on. By default `18082` for mainnet, `38082` for stagenet, and `28082` for testnet. |
| `--db-type` | Specify database type. The default and only available: `lmdb`. |

## Commands

Commands give access to specific services provided by the daemon. Commands are executed against the running daemon. Their names follow the `command_name` convention.

The following groups are only to make reference easier to follow. The daemon itself does not group commands in any way.

See running for example usage. You can also type commands directly in the console of the running `xcashd` \(if not detached\).

### **Help, version, status**

| **Option** | Description |
| :--- | :--- |
| `help [<command>]` | Show help for `<command>`. |
| `version` | Show version information. Example output: `X-CASH '' (v1.5.0-unknown)` |
| `status` | Show status. Example output: `Height: 424951/424951 (100.0%) on mainnet, not mining, net hash 2.38 MH/s, v12, update needed, 8(out)+0(in) connections, uptime 0d 1h 18m 25s` |

### **P2P  network**

| **Option** | Description |
| :--- | :--- |
| `print_pl` | Show the full peer list. |
| `print_pl_stats` | Show the full peer list statistics \(white vs gray peers\). White peers are online and reachable. Grey peers are offline but your `xcashd` remembers them from past sessions. |
| `print_cn` | Show connected peers with connection initiative \(incoming/outgoing\) and other stats. |
| `ban <IP> [<seconds>]` | Ban a given `<IP>` for a given amount of `<seconds>`. By default the ban is for 24h. Example: `./xcashd ban 187.63.135.161` |
| `unban <IP>` | Unban a given `<IP>`. |
| `bans` | Show the currently banned IPs. Example output: `187.63.135.161 banned for 86397 seconds`. |
| `in_peers <max_number>` | Set the of incoming connections from other peers. |
| `out_peers <max_uber>` | Set the of outgoing connections to other peers. |
| `limit [<kB/s>]` | Get or set the download and upload limit. |
| `limit_down [<kB/s>]` | Get or set the download limit. |
| `limit_up [<kB/s>]` | Get or set the upload limit. |

### **Transaction pool**

| **Option** | Description |
| :--- | :--- |
| `flush_txpool [<txid>]` | Flush specified transaction from transactions pool, or flush the whole transactions pool if was not provided. |
| `print_pool` | Print the transaction pool using a verbose format. |
| `print_pool_sh` | Print the transaction pool using a short format. |
| `print_pool_stats` | Print the transaction pool's statistics \(number of transactions, memory size, fees, double spend attempts etc\). |

### **Transactions**

| **Option** | Description |
| :--- | :--- |
| `print_coinbase_tx_sum  <start_height> [<block_count>]` | Show a sum of all emitted coins and paid fees within specified range. Example: `./xcashd print_coinbase_tx_sum 0 1000000000000` |
| `print_tx <transaction_hash>  [+hex] [+json]` | Show specified transaction as JSON and/or HEX. |
| `relay_tx <txid>` | Force relaying the transaction. Useful if you want to rebroadcast the transaction for any reason or if transaction was previously created with "do\_not\_relay":true. |

### **Blockchain**

| **Option** | Description |
| :--- | :--- |
| `print_height` | Show local blockchain height. |
| `sync_info` | Show blockchain sync progress and connected peers along with download / upload stats. |
| `print_bc  <begin_height> [<end_height>]` | Show detailed data of specified block. |
| `hard_fork_info` | Show current consensus version and future hard fork block height, if any. |
| `is_key_image_spent  <key_image>` | Check if specified key image is spent. Key image is a hash. |

### **Manage daemon**

| Option | Description |
| :--- | :--- |
| `exit`, `stop_daemon` | Ask daemon to exit gracefully. The `exit` and `stop_daemon` are identical \(one is alias of the other\). |
| `set log <{+,-,}categories>` | Set the current log level/categories where `<level>` is a number 0-4 |
| `print_status` | Show if daemon is running. |
| `update (check download)` | Check if update is available and optionally download it. The hash is SHA-256. On linux use `sha256sum` to verify. Example output: `Update available: v0.13.0.4: https://downloads.getxcash.org/cli/xcash-linux-x64-v0.13.0.4.tar.bz2, hash 693e1a0210201f65138ace679d1ab1928aca06bb6e679c20d8b4d2d8717e50d6` `Update downloaded to: /opt/xcash-v0.13.0.2/xcash-linux-x64-v0.13.0.4.tar.bz2` |

### **Mining**

{% hint style="danger" %}
Mining functions will not work anymore under X-Cash 2.0
{% endhint %}

| **Option** | Description |
| :--- | :--- |
| `show_hr` | Ask `xcashd` daemon to stop printing current hash rate. Relevant only if `xcashd` is mining. |
| `hide_hr` | Ask `xcashd` daemon to print current hash rate. Relevant only if `xcashd`is mining. |
| `start_mining  <adr> [<threads>] [do_background_mining] [ignore_battery]` | Ask `xcashd`daemon to start mining. Block reward will go to `<addr>`. |
| `stop_mining` | Ask `xcashd` daemon to stop mining. |

### **Testing**

| **Option** | Description |
| :--- | :--- |
| `start_save_graph` | Start saving data for dr X-Cash. |
| `stop_save_graph` | Stop saving data for dr X-Cash. |

### **Legacy**

| **Option** | Description |
| :--- | :--- |
| `save` | Flush blockchain data to disk. This is normally no longer necessary as `xcashd` saves the blockchain automatically on exit. |
| `output_histogram [@<amount>] <min_count> [<max_count>]` | Show number of outputs for each amount denomination. This was only relevant in the pre-RingCT era. The old wallet used this to determine which outputs can be used for the requested mixin. With RingCT denominations are irrelevant as amounts are hidden. More info in these SA answers. |

