# xcash-wallet-cli functions

## Register a delegate

Make sure to stop the XCASH Wallet service if it is running

```bash
systemctl stop xcash-rpc-wallet
```

Open the wallet file in the `xcash-wallet-cli` and once the wallet is fully synchronize, type the command:

```text
delegate_register delegate_name delegate_IP_address
```

* Replace `delegate_name` with the delegate name you have chosen.
* Replace `delegate_IP_address` with your VPS/dedicated servers IP Address or a domain name 

## Vote for a delegate

Make sure to stop the XCASH Wallet service if it is running

```bash
systemctl stop xcash-rpc-wallet
```

Open the wallet file in the `xcash-wallet-cli` and once the wallet is fully synchronize, type the command:

```text
vote <delegates_public_address|delegates_name>
```

* Replace `delegates_public_address|delegates_name`with the `delegates public address` or `delegates name`

## Update a delegate's information

Make sure to stop the XCASH Wallet service if it is running

```bash
systemctl stop xcash-rpc-wallet
```

Open the wallet file in the `xcash-wallet-cli` and once the wallet is fully synchronize, type the command:

```text
delegate_update <item> <value>
```

* Replace `<item>` with the item you want to update. The list of valid items is:

```text
IP_address
about
website
team
shared_delegate_status
delegate_fee
server_specs
```

* Replace `<value>` with the updated information

## Remove a delegate

Make sure to stop the XCASH Wallet service if it is running

```bash
systemctl stop xcash-rpc-wallet
```

Open the wallet file in the `xcash-wallet-cli` and once the wallet is fully synchronize, type the command:

```text
delegate_remove
```

