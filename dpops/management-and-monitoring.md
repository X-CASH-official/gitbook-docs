---
description: >-
  This section is designed to gather steps for managing the different services
  around the xcash-dpops program, getting logs and monitoring the activity.
---

# Management & Monitoring

## `systemd` services

`systemd` is generally used to manage low-level programs in Linux-based systems. It's a reliable way to automatically run programs on startup, and manage and monitor the different services.

In `systemd`, a `unit` refers to any resource that the system knows how to operate on and manage. This is the primary object that the `systemd` tools know how to deal with. These resources are defined using configuration files called **unit files**. Whether you installed with the [autoinstaller script](node-installation.md#quick-installation) or [manually](node-installation.md#manual-installation), the programs needed to run the X-Cash consensus are managed in `systemd` with `unit` files.

The different services needed for the X-Cash consensus running on the server are the listed below:  

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
All the **`unit`** files are located in **`/lib/systemd/system/`**
{% endhint %}

### **Services Management**

At some point, you might have to need to restart or check the status of the services, either to reflect a change you have made, to update, or to run the program with different parameters.

{% tabs %}
{% tab title="Start" %}
To **start** a `systemd` service, run:

```text
systemctl start SERVICE
```

_**Example:**_ 

```text
systemctl start XCASH_DPOPS
```
{% endtab %}

{% tab title="Restart" %}
To **restart** a `systemd` service, run:

```text
systemctl restart SERVICE
```

_**Example:**_ 

```text
systemctl restart XCASH_Wallet
```
{% endtab %}

{% tab title="Stop" %}
To **stop** a `systemd`  service, run:

```text
systemctl stop SERVICE
```

_**Example:**_ 

```text
systemctl stop XCASH_Daemon
```
{% endtab %}

{% tab title="Status" %}
To check the **status** of a `systemd` service, run:

```text
systemctl status SERVICE
```

_**Example:**_ 

```text
systemctl status MongoDB
```

**Output:** 

```text
● MongoDB.service - MongoDB Database Server
   Loaded: loaded (/lib/systemd/system/MongoDB.service; disabled; vendor preset: enabled)
   Active: active (exited) since Mon 2020-06-08 11:50:51 CEST; 38min ago
 Main PID: 14852 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/MongoDB.service
           └─14854 /root/x-network/mongodb-linux-x86_64-ubuntu1804-4.2.3/bin/mongod --fork --syslog --dbpath /data/db/
```
{% endtab %}
{% endtabs %}

### **Monitoring & Logging**

While the services are running in the background, you might want to check the outputs of the different programs. 

To monitor the services, we are using **`journalctl`** which fetch the journal of the **`systemd`** services. Using **`journalctl`** without paramaters will show the full contents of the journal, starting with the oldest entry collected. 

For a live logging with better readability, we will limit the output by using the following parameters: 

```text
journalctl --unit=SERVICE --follow -n 100 --output cat
```

To check the **`xcash-dpops`** services, you can copy the following commands: 

{% tabs %}
{% tab title="XCASH\_DPOPS" %}
```text
journalctl --unit=XCASH_DPOPS --follow -n 100 --output cat
```
{% endtab %}

{% tab title="XCASH\_Daemon" %}
```
journalctl --unit=XCASH_Daemon --follow -n 100 --output cat
```
{% endtab %}

{% tab title="XCASH\_Wallet" %}
```
journalctl --unit=XCASH_Wallet --follow -n 100 --output cat
```
{% endtab %}

{% tab title="MongoDB" %}
```
journalctl --unit=XCASH_MongoDB --follow -n 100 --output cat
```
{% endtab %}

{% tab title="firewall" %}
```
journalctl --unit=firewall --follow -n 100 --output cat
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The **`XCASH_Daemon`** service export its logs in a different place. To display the latest log, use the following command:

```text
tail -n 100 ~/xcash-official/logs/XCASH_Daemon_log.txt
```
{% endhint %}



