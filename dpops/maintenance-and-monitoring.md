---
description: >-
  This section is designed to gather steps for managing the different services
  around the xcash-dpops program, getting logs and monitoring the activity.
---

# Management & Statistics

## Services Management

`systemd` is generally used to manage low-level programs in Linux-based systems. It's a reliable way to automatically run programs on startup, and manage and monitor the different services.

In `systemd`, a `unit` refers to any resource that the system knows how to operate on and manage. This is the primary object that the `systemd` tools know how to deal with. These resources are defined using configuration files called **unit files**. Whether you installed with the [autoinstaller script](installation-process.md#quick-installation) or [manually](installation-process.md#manual-installation), the programs needed to run the X-Cash consensus are managed in `systemd` with `unit` files.

The different services needed for the X-Cash consensus running in the server are the following ones: 

{% tabs %}
{% tab title="XCASH\_DPOPS.service" %}
```bash
[Unit]
Description=XCASH DPOPS
 
[Service]
Type=simple
LimitNOFILE=infinity
User=root
WorkingDirectory=~/xcash-official/xcash-dpops/build
ExecStart=~/xcash-official/xcash-dpops/build/XCASH_DPOPS --block_verifiers_secret_key BLOCK_VERIFIER_SECRET_KEY
Restart=always
 
[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="XCASH\_Daemon.service" %}
```
[Unit]
Description=XCASH Wallet RPC
 
[Service]
Type=simple
User=root
ExecStart=~/xcash-official/xcash-core/build/release/bin/xcash-wallet-rpc --wallet-file ~/xcash-official/xcash_wallets/WALLET --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon
Restart=always
 
[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="XCASH\_Wallet.service" %}
```
[Unit]
Description=XCASH Wallet RPC
 
[Service]
Type=simple
User=root
ExecStart=~/xcash-official/xcash-core/build/release/bin/xcash-wallet-rpc --wallet-file ~/xcash-official/xcash_wallets/WALLET --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon
Restart=always
 
[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="MongoDB.service" %}
```
[Unit]
Description=MongoDB Database Server
After=network.target

[Service]
Type=forking
User=root
Type=oneshot
RemainAfterExit=yes
PIDFile=~/xcash-official/systemdpid/mongod.pid
ExecStart=~/xcash-official/mongodb-linux-x86_64-ubuntu1804-4.4.0-rc7-36-gcf4ac31/bin/mongod --fork --syslog --dbpath /data/db

LimitFSIZE=infinity
LimitCPU=infinity
LimitAS=infinity
LimitNOFILE=64000
LimitNPROC=64000
LimitMEMLOCK=infinity
TasksMax=infinity
TasksAccounting=false

[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="firewall.service" %}
```
[Unit]
Description=firewall
 
[Service]
Type=oneshot
RemainAfterExit=yes
User=root
ExecStart=~/xcash-official/xcash-dpops/scripts/firewall/firewall_script.sh
 
[Install]
WantedBy=multi-user.target
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
All the `unit` files are located in **`/lib/systemd/system/`**
{% endhint %}

 

**Actions on systemd services**

**Journalctl**

**Website**

**Delegate Website**



