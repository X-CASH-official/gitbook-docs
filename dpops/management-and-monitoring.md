---
description: >-
  This section is designed to gather steps for managing the different services
  around the xcash-dpops program, getting logs and monitoring the activity.
---

# Management & Monitoring

## Managing Installation

You can manage your program installation with the installer script: 

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

![](../.gitbook/assets/image%20%2824%29.png)

The installation script enables you to install and manage your `xcash-dpops` program easily.  

### Restart Program

If you have an issue with your program running, or need to do some changes in the settings and need to restart the different program, it is recommended to restart the service using the installer script.

Run the installer script and choose **option 12**.

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

### Update program

When a new update of the program is pushed, you will need to a update your program. 

Run the installer script and choose **option 2**.

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

### Change delegate mode

To change your settings from a solo to a shared delegate \(and vice versa\), you can run the installer script and choose **option 9,** or change the settings \(fees and minimum payout\) with **option 10.**

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

You will be asked to input a delegate fee and a minimum payout amount to your voters. The script will automatically change the program settings to match your changes.

{% hint style="info" %}
Don't forget to update your delegate fee by [updating the public information](register-delegate.md#2-update-public-information) as well.
{% endhint %}

### Back up your local database

As a shared delegated, you will have the responsability to distribute the payout to your voters. The voters information and shares are stored locally into your delegate node database. Before attempting to update your node, or do maintenance on your server, it is highly recommended to backup this information.

To backup your shared delegate database, chose **option 19**:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

## `systemd` services

`systemd` is generally used to manage low-level programs in Linux-based systems. It's a reliable way to automatically run programs on startup, and manage and monitor the different services.

In `systemd`, a `unit` refers to any resource that the system knows how to operate on and manage. This is the primary object that the `systemd` tools know how to deal with. These resources are defined using configuration files called **unit files**. Whether you installed with the [autoinstaller script](node-installation.md#quick-installation) or [manually](node-installation.md#manual-installation), the programs needed to run the X-Cash consensus are managed in `systemd` with `unit` files.

The different services needed for the X-Cash consensus running on the server are listed below:

{% tabs %}
{% tab title="xcash-dpops.service" %}
```bash
[Unit]
Description=X-Cash DPOPS Daemon background process

[Service]
Type=simple
LimitNOFILE=infinity
User=root
WorkingDirectory=~/xcash-official/xcash-dpops/build
ExecStart=~/xcash-official/xcash-dpops/build/xcash-dpops --block-verifiers-secret-key BLOCK_VERIFIER_SECRET_KEY
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="xcash-daemon.service" %}
```text
[Unit]
Description=X-Cash Daemon background process

[Service]
Type=simple
User=root
ExecStart=~/xcash-official/xcash-core/build/release/bin/xcash-wallet-rpc --wallet-file ~/xcash-official/xcash-wallets/WALLET --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="xcash-rpc-wallet.service" %}
```text
[Unit]
Description=X-Cash RPC Wallet background process

[Service]
Type=simple
User=root
ExecStart=~/xcash-official/xcash-core/build/release/bin/xcash-wallet-rpc --wallet-file ~/xcash-official/xcash-wallets/WALLET --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="mongodb.service" %}
```text
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
```text
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
systemctl start xcash-dpops
```
{% endtab %}

{% tab title="Restart" %}
To **restart** a `systemd` service, run:

```text
systemctl restart SERVICE
```

_**Example:**_

```text
systemctl restart xcash-rpc-wallet
```
{% endtab %}

{% tab title="Stop" %}
To **stop** a `systemd` service, run:

```text
systemctl stop SERVICE
```

_**Example:**_

```text
systemctl stop xcash-daemon
```
{% endtab %}

{% tab title="Status" %}
To check the **status** of a `systemd` service, run:

```text
systemctl status SERVICE
```

_**Example:**_

```text
systemctl status mongodb
```

**Output:**

```text
● mongodb.service - MongoDB Database Server
   Loaded: loaded (/lib/systemd/system/mongodb.service; disabled; vendor preset: enabled)
   Active: active (exited) since Mon 2020-06-08 11:50:51 CEST; 38min ago
 Main PID: 14852 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/mongodb.service
           └─14854 /root/xcash-official/mongodb-linux-x86_64-ubuntu1804-4.2.3/bin/mongod --fork --syslog --dbpath /data/db/
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
{% tab title="xcash-dpops" %}
```text
journalctl --unit=xcash-dpops --follow -n 100 --output cat
```
{% endtab %}

{% tab title="xcash-daemon" %}
```text
journalctl --unit=xcash-daemon --follow -n 100 --output cat
```
{% endtab %}

{% tab title="xcash-rpc-wallet" %}
```text
journalctl --unit=xcash-rpc-wallet --follow -n 100 --output cat
```
{% endtab %}

{% tab title="mongodb" %}
```text
journalctl --unit=mongodb --follow -n 100 --output cat
```
{% endtab %}

{% tab title="firewall" %}
```text
journalctl --unit=firewall --follow -n 100 --output cat
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The **`xcash-daemon`** service export its logs in a different place. To display the latest log, use the following command:

```text
tail -n 100 ~/xcash-official/logs/xcash-daemon-log.txt
```
{% endhint %}

