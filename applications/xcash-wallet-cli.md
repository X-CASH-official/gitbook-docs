---
description: >-
  Use X-Cash CLI Wallet (xcash-wallet-cli) to interact with a wallet through the
  terminal
---

# X-Cash Wallet \(CLI\)

## Overview

### Command line wallet

The "fficial command line wallet for X-Cash. Available for Linux, macOS and Windows.

Wallet uses your private keys to compute your total balance, retrieve your transactions history, and sign transactions.

However, wallet does not store the blockchain and does not directly participate in the p2p network.

{% hint style="info" %}
The CLI wallet is the most reliable, secured and most feature complete wallet for X-Cash.
{% endhint %}

![Ilustration of the 1.5.0 CLI Wallet displaying an unconfirmed balance](../.gitbook/assets/image%20%281%29%20%281%29.png)

### Depends on the full node

Wallet connects to a full node to scan the blockchain for your transaction outputs and to send your transactions out to the network. The full node can be either local \(same computer\) or remote.

The best practice is to run the full node on the same computer as the wallet \(or within your home network\) and the connection happens over HTTP.

Any transaction leaving the wallet is already blinded by all X-Cash privacy features. This means plain text HTTP communication isn't an issue on its own even if you connect to a remote node.

However, connecting to a remote node has other nuanced trade-offs, such as easiness of use and faster syncing. This happens at the cost of a potential privacy reduction and limited reliability. It is recommended to fully familiarize with the limits of using a remote node vs. a local node before attempting in doing so.

## Syntax

`./xcash-wallet-cli [options] [command]`

#### Example:

`./xcash-wallet-cli --restore-deterministic-wallet`

## Running

Go to directory where you unpacked X-Cash.

Run the daemon and wait until it syncs up with the network \(may take up to a few days depending on your last block height, network speed and CPU available load\):

`./xcashd`

In a separate terminal window, run the wallet:

`./xcash-wallet-cli --generate-new-wallet XCashExampleWallet`

## Options

### **Help and version**

| **Option** | Description |
| :--- | :--- |
| `--help` | Enlist available options. |
| `--version` | Show `xcash-wallet-cli` version to stdout. Example:  `X-CASH '' (v1.5.0-unknown)` |

### Pick network

| Option | Description |
| :--- | :--- |
| `(default)` | By default wallet assumes mainnet. |
| `--stagenet` | Run on stagenet. Remember to run your daemon with `--stagenet` as well. |
| `--testnet` | Run on testnet. Remember to run your daemon with `--testnet` as well. |

### **Logging**

| Option | Description |
| :--- | :--- |
| `--log-file` | Full path to the log file. |
| `--log-level <arg>` | `0-4` with `0` being minimal logging and `4` being full tracing. Defaults to `0`. These are general presets and do not directly map to severity levels. For example, even with minimal `0`, you may see some most important `INFO`entries. |
| `--max-log-file-size` | Soft limit in bytes for the log file \(=104850000 by default, which is just under 100MB\). Once log file grows past that limit, x-cash creates the next log file with a UTC timestamp postfix `-YYYY-MM-DD-HH-MM-SS`.  In production deployments, you would probably prefer to use established solutions like logrotate instead. In that case, set `--max-log-file-size 0` to prevent x-cash from managing the log files. |
| `--max-log-files` | Limit on the number of log files \(=50 by default\). The oldest log files are removed. In production deployments, you would probably prefer to use established solutions like logrotate instead. |

### **Full node connection**

Wallet depends on a full node for all non-local operations. The following options define how to connect to `xcashd`:

| **Option** | Description |
| :--- | :--- |
| `--daemon-address` | Use `xcashd` instance at `<host>:<port>`. Example:  `./xcash-wallet-cli --daemon-address xcash-stagenet.exan.tech:38081 --stagenet` |
| `--daemon-host` | Use `xcashd` instance at host `<arg>` instead of localhost. |
| `--daemon-port` | Use `xcashd` instance at port `<arg>` instead of 18281. |
| `--daemon-login` | Specify `username[:password]` for `xcashd` RPC API. It is based on HTTP Basic Auth. Mind that connections are by default un-encrypted. Authentication only makes sense if you establish a secure connection \(maybe via Tor, or SSH tunneling, or reverse proxy w/ TLS\). |
| `--trusted-daemon` | Enable commands and behaviors which rely on `xcashd` instance being trusted. Default for localhost connection. The trust in this context concerns preserving your privacy. Only use this flag if you do control `xcashd`. Trusted daemon allows for commands like `rescan_spent`, `start_mining`, `import_key_images` and behaviors like **not** warning about potential attack on transient problems with transaction sending. |
| `--untrusted-daemon` | Disable commands and behaviors which rely on `xcashd` instance being trusted. Default for a non-localhost connections. See `--trusted-daemon` for more details. |
| `--do-not-relay` | The newly created transaction will not be relayed to the X-Cash network. Instead it will be dumped to a file in a raw hexadecimal format. Useful if you want to push the transaction through a gateway. This may be easier to use over Tor than X-Cash wallet. |
| `--allow-mismatched-daemon-version` | Allow communicating with `xcashd` that uses a different RPC version. |

### **Create new wallet**

| Option | Description |
| :--- | :--- |
| `--generate-new-wallet` | Create a new X-Cash wallet and save it to `<arg>` file. You will be asked for a password. The password is used to encrypt the wallet file but it is unrelated to your master spend key or mnemonic seed. Generate a very strong password with your password manager \(~256 bits of entropy\). Example:  `./xcashd-wallet-cli --stagenet --generate-new-wallet $HOME/.bitxcash/stagenet/wallets/XCashExampleStagenetWallet` |
| `--kdf-rounds` | Concerns encrypting the wallet file. The wallet file is encrypted with ChaCha stream cipher. The encryption key is derived from the user supplied password by hashing the password with CryptoNight. This option defines how many times the CryptoNight hashing will be applied. The default is `1` round of hashing.   Note this is **unrelated** to spend key generation.   The more rounds the longer you will wait to open the wallet or send transaction. But also the attacker will have it harder to brute force your wallet password.   **Note:** You will have to remember and provide the same `kdf-rounds` on every wallet access!  **Recommendation:** Do not change the default value. Instead generate a very strong wallet password with your password manager \(256 bits of entropy\). |

### **Open existing wallet**

| **Option** | Description |
| :--- | :--- |
| `--wallet-file` | Open existing wallet. Example:   `./xcash-wallet-cli --stagenet --wallet-file $HOME/.bitxc/stagenet/wallets/XCashExampleStagenetWallet`   This is only for wallet files generated with `xcash-wallet-cli`, `xcash-wallet-gui`, or `xcash-wallet-rpc` tools. If you have other type of wallet then see importing options. |
| `--password` | Provide wallet password as a parameter instead of interactively. Remember to escape/quote as needed.   **Not recommended** because the password will remain in your command history and will also be visible in the process table. For automation prefer `--password-file`.   The option also works in combination with `--generate-new-wallet`. |
| `--password-file` | Provide password as a file in stead of interactively. Trailing `\n` are discarded when reading the password file.   Prefer this over `--password` if you automate wallet access. Make sure the password file is meaningfully separated from the wallet file. Otherwise it provides no security benefit.   The option also works in combination with `--generate-new-wallet`. |

### **Restore wallet**

| **Option** | Description |
| :--- | :--- |
| `--generate-from-device` | Restore/generate a special wallet to work with a **hardware device** like Ledger or Trezor and save it to `<arg>` file. Example:   `./xcash-wallet-cli --stagenet --generate-from-device XCashExampleDeviceWallet --subaddress-lookahead 5:20`   This is a one-time action. Next time you simply open the wallet.  By default the command expects Ledger hardware connected. For Trezor hardware add `--hw-device Trezor` \(expected ~May 2019\).  It will take **up to 25 minutes** with default settings. This is because hardware devices are slow to pre-generate subaddresses. To mitigate use low `--subaddress-lookahead 5:20`.   The local wallet will not have private spend key and will not be able to spend on its own. It serves as a user interface and a bridge for low-power hardware devices. Transaction signing with a private spend key always happens on the hardware device.   See the complete guide to hardware wallet setup. |
| `--generate-from-view-key` | Restore a view-only version of the wallet to track incoming transactions and save it to `<arg>` file. The wallet is created based on a **secret view key** and **standard address**. The secret view key is meant to be pasted as hexadecimal. |
| `--generate-from-spend-key` | Restore a wallet from **secret spend key** and save it to `<arg>` file. The secret spend key is meant to be pasted as hexadecimal. |
| `--restore-deterministic-wallet` | Restore a wallet from **secret mnemonic seed**. Use this to restore from your 25 words backup.   You will be asked for a password to encrypt the wallet file \(once restored\). Note this is **not** a passphrase to mnemonic seed. Mnemonic seeds generated by X-Cash official wallets are naked. |

### **Multisig wallet**

| **Option** | Description |
| :--- | :--- |
| `--generate-from-multisig-keys` | Create a standard wallet from multisig keys. This is useful to combine all multisig secret keys back into the standard wallet \(when you no longer need the multisig\). The wallet will then have control of the funds. It only supports providing all secret keys even if the multisig scheme allowed for less \(only `N/N` not `N/M`\). |
| `--restore-multisig-wallet` | Restore a multisig wallet from **secret seed** that was earlier exported with the `seed` interactive command. This only restores your part of the wallet. Other multisig participants will still be necessary to sign the transaction. |

### **Config file**

| **Option** | Description |
| :--- | :--- |
| `--config-file` | Full path to the configuration file. Note this should be a separate config than `xcashd`uses because these tools accept different set of options. |

### **Performance**

| **Option** | Description |
| :--- | :--- |
| `--subaddress-lookahead` | Accepts `m:n`, by default `50:200`. The first value is the number of accounts and the second value is the number of subaddresses per account.   The wallet will not check for payments to subaddresses further than `n`away from the last received payment. This can happen if you generated unique subaddresses for `n` clients in a row but none of them paid.   On the other hand the more subaddresses you set to look ahead, the longer it takes to create your wallet, because they must be pre-computed. This is normally not a concern, except for hardware wallets. On the Ledger the default value of `50:200` can take over 20 minutes \(one time on wallet creation\)! |
| `--max-concurrency` | Max number of threads to use for parallel jobs. The default value `0` uses the number of CPU threads |

### **Internationalization**

| **Option** | Description |
| :--- | :--- |
| `--mnemonic-language` | Language for mnemonic seed words. One of `english`, `english_old`, `esperanto`, `french`, `german`, `italian`, `japanese`, `lojban`, `portuguese`, `russian`, `spanish`.   It might be a good idea to stick to default English which is by far the most popular and well tested. It also avoids potential non-ASCII characters pitfalls or bugs. |
| `--use-english-language-names` | If your display freezes, exit blind with ^C, then run again with `--use-english-language-names`. This can happen when X-Cash prompts for a language displaying language names in their natives alphabets. |

### **Legacy**

{% hint style="info" %}
These options are either legacy or rarely useful.
{% endhint %}

| **Option** | Description |
| :--- | :--- |
| `--non-deterministic` | Generate legacy non-deterministic wallet. The view key will **not** be derived from the spend key. You would also have to backup the _.keys. To restore non-deterministic wallet \(standard address\) use `--generate-from-keys`. To restore fully you will need the_ .keys file. |
| `--generate-from-keys` | Restore legacy non-deterministic wallet by providing both spend and view keys and the standard address. |
| `--shared-ringdb-dir` | Set shared ring database path. No longer worthwhile. |
| `--electrum-seed` | Provide mnemonic seed as a command line option for `--restore-deterministic-wallet` instead of interactively. This is not recommended b/c the seed will be saved in your command history and also visible in the process list. |
| `--generate-from-json` | You would run `xcash-wallet-rpc` to use this option. |
| `--tx-notify` | You would run `xcash-wallet-rpc` to use this option. |

## Default setup

Wallet files are created and seek in current directory. This is rarely what you want. Use `--wallet-file` and similar options to control this.

Log files are created in the same directory as `xcash-wallet-cli` binary. Use `--log-file` to specify the location.

## Commands

Commands are used interactively in the `xcash-wallet-cli` prompt.

You can also run a one-off command by providing it as a commandline parameter. This is rarely useful though. For automation prefer `xcash-wallet-rpc`.

The CLI wallet has **built-in help for individual commands** - we will not attempt to reproduce that. Instead we focus on grouping commands so you can quickly find what you are looking for. Use `help command_name` to learn more.

### Help and version

`help` - list all commands

`help <command>` - show help for individual command

`version` - show version of the xcash-wallet-cli binary

### Balance

`account` - total balance; list accounts with respective balances

`balance detail` - within the current account, list addresses with respective balances

### Transfer funds

To perform on-chain transaction you should used the function transfer:`transfer [<tx_privacy_settings>] [index=<N1>[,<N2>,...]] [<priority>] [<ring_size>] <address> <amount> [<payment_id>]`

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Option</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>tx_privacy_settings</code>
      </td>
      <td style="text-align:left">Blockchain privacy setting feature, can be <code>public</code> or <code>private</code>.
        Defaults to private.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>index</code>
      </td>
      <td style="text-align:left">Index of the transaction.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>priority</code>
      </td>
      <td style="text-align:left">
        <p>Priority setting i.e. fee (multiplier) settings, can be <code>[0, 1, 2, 3, 4]</code>
        </p>
        <p>Defaults to 1.
          <br />Leads to a multiplication of the base fee per kb of respectively <code>1x, 4x, 20x, and 166x</code>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>ring_size</code>
      </td>
      <td style="text-align:left">Number of possible signers in the ring signature. Defaults and fixed to <code>21</code>since
        1.4.0</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>address</code>
      </td>
      <td style="text-align:left">Recipient address. Should be an <code>XCASH</code>, <code>integrated</code> or <code>sub</code>address.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>amount</code>
      </td>
      <td style="text-align:left">Amount to be sent in <code>XCASH</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>payment_id</code>
      </td>
      <td style="text-align:left">Payment id to be added to the transaction. Defaults to <code>none</code>.</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
#### About the fixed ring size

In X-Cash 1.4.0, we introduced a fixed ring size of **21** along with the release of public transactions.  
This choice has been made to improve significantly the privacy at the cost of an increase of the transaction size which is largely overbalanced by the introduction of bulletproof.
{% endhint %}

### Vote for a delegate

Open the wallet file in the `xcash-wallet-cli` and once the wallet is fully synchronize, type the command:

```text
vote <delegates_public_ip|delegates_name>
```

* Replace `delegates_public_address|delegates_name`with the `delegates public address` or `delegates name`

### Manage accounts

`account`

`account new`

`account switch`

`account label`

### Manage addresses

`address all`

`address new`

`address label`

### Network status

`status` - show if synced up to the blockchain height

`fee` - show current fee-per-byte and full node's mempool \(the backlog of transactions depending on the priority\)

`wallet_info` - show wallet file path, standard address, type and network

### Secret mnemonic seed

`seed` - show raw mnemonic seed

`encrypted_seed` - create mnemonic seed encrypted with the passphrase; you will need to remember or store the passphrase separately; restoring will not be possible without the passphrase

### Secret keys

`spendkey` - show secret spend key and public spend key

`viewkey` - show secret view key and public view key

### Proofs

`get_reserve_proof` -&gt; `check_reserve_proof` - prove the balance

`get_spend_proof` -&gt; `check_spend_proof` - prove you made the payment

`sign <file>` -&gt; `verify <filename> <address> <signature>` - prove ownership of the address; allows to verify the file was signed by the owner of specific X-Cash address

`get_tx_proof` -&gt; `check_tx_proof`

### Multisig

### **Setup**

`prepare_multisig`

`make_multisig`

`finalize_multisig`

### **Update**

`export_multisig_info`

`import_multisig_info`

### **Other**

`submit_multisig`

`exchange_multisig_keys`

`export_raw_multisig_tx`

### Tx private key

These allow to learn and verify transaction's private key `r`. This was useful to create a proof of payment but got superseded by `get_spend_proof`.

`get_tx_key <txid>`

`check_tx_key <txid> <txkey> <address>`

`set_tx_key <txid> <tx_key>`

### Advanced

`unspent_outputs` - show a list of, and a histogram of unspent outputs \(indivisible pieces of your total balance\)

`export_key_images <file>` -&gt; `import_key_images <file>` - used to inform the view-only wallet about outgoing transactions so it can calculate the real balance; normally view-only wallets only learn about incoming transactions, not outgoing

`export_outputs <file>` - export a set of outputs owned by this wallet to a `<file>`

