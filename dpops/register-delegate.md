---
description: >-
  Once your server is set up and the necessary programs installed, you will be
  able to register yourself as a delegate.
---

# Register Delegate

## Introduction

Once you have correctly [set up your instance](server-setup.md), installed the different programs, you can now register as a delegate of the X-Cash Public Network.  
You will need to have all the services already running to generate the necessary parameters to register yourself as a delegate.

## 1. Register Yourself as a Delegate

You should have noted your block verifier keys during the [program installation](node-installation.md). You will need to prepare a **delegate name**, your **server IP address** or **domain name**, and the **block verifier public key.** Furthermore, you will have to register from the wallet you will be used to collect the reward.

{% hint style="warning" %}
Chose your delegate name wisely as you won't be able to change it down the line. If you want to change the name, you will have to generate a new block verifier key pair and register yourself again, but **you will however lose your previous delegate stats.**
{% endhint %}

First of all, the wallet service should be running in the background. Stop it by using the command:

```text
systemctl stop xcash-rpc-wallet
```

Open and let synchronize your wallet generated during the node installation, either when using the [auto-installer](node-installation.md#quick-installation) or [created manually](node-installation.md#generate-a-wallet).

```text
~/xcash-official/xcash-core/build/release/bin/xcash-wallet-cli --wallet-file ~/xcash-official/xcash-wallets/<WALLET_NAME>
```

Replace the **`<WALLET_NAME>`** with your own.

{% hint style="info" %}
If you installed with the auto-installer script, the wallet name will be **`delegate-wallet`**
{% endhint %}

Once your wallet is fully synchronized, you can use the **`delegate_register`** command with the following parameters:

```text
delegate_register <delegate_name> <IP_address|domain_name> <block_verifier_public_key>
```

And replace the information with:

* **`<delegate_name>`**: the name that will be displayed on the delegate explorer. _Cannot be updated._
* **`<IP_address|domain_name>`** : your server's IP address or its domain name \(if you have bought a domain name and correctly set up the DNS record\). It is recommended to register your delegate using your domain name, as you can redirect your server IP address to it easily in your DNS record, enabling you to change server in the future.  It is however possible to change this information at a later time. _Can be updated._
* **`<block_verifier_public_key>`** : the block verifier public key that you have generated [earlier](register-delegate.md#1-generate-a-block-verifier-key). _Cannot be updated._ 

**Example:**

```text
delegate_register my_delegate my_delegate.domain.com cf8718d638ce0a831f3538ea60d1e27c3a258c7004a1ad7c547cc5331de7d9d7
```

You will be prompted to wait for the next valid data interval. Once your request has been accepted, you will receive the message **`The delegate has been registered successfully`**.

You can check in the [delegates explorer](http://delegates.xcash.foundation/) if your node is correctly appearing.

You can **`exit`** the wallet, and restart the wallet service:

```text
systemctl restart xcash-rpc-wallet
```

## 2. Update Public Information

Each registered delegates will be displayed in the [delegates explorer](http://delegates.xcash.foundation/), along with their statistics and information. At registration, the minimum information that is displayed is your **delegate name** and **IP address**. You can add additional instructions to help others identify you or rally to your cause and vote for you.

First of all, the wallet service should be running in the background. Stop it by using the command:

```text
systemctl stop xcash-rpc-wallet
```

Open and let synchronize the wallet you used to register as a delegate, either when using the [auto-installer](node-installation.md#quick-installation) or [created manually](node-installation.md#generate-a-wallet).

```text
~/xcash-official/xcash-core/build/release/bin/xcash-wallet-cli --wallet-file ~/xcash-official/xcash-wallets/<WALLET_NAME>
```

Replace the **`<WALLET_NAME>`** with your own.

{% hint style="info" %}
If you installed with the autoinstaller script, the wallet name will be **`delegate-wallet`**
{% endhint %}

Once your wallet is fully synchronized, you can use the **`delegate_update`** command with the following parameters:

```text
delegate_update <item> <value>
```

Replace the `<item>` with one of the lists below, and change the corresponding `<value>`

<table>
  <thead>
    <tr>
      <th style="text-align:left">Item</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>about</code>
      </td>
      <td style="text-align:left"><b>String</b>  <em>(1024 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p>Short description about you and your motivations as a delegate.</p>
        <p><b>Example:</b>
        </p>
        <p><code>delegate_update about Even the smallest delegate can change the course of the future.</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>IP_address</code>
      </td>
      <td style="text-align:left"><b>String </b><em>(255 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p>Update the IP address or domain name of your delegate node. Must be in
          IPV4 format</p>
        <p><b>Example:</b>
        </p>
        <p><code>delegate_update IP_address mydomain.com </code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>website</code>
      </td>
      <td style="text-align:left"><b>String</b>  <em>(255 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p>Link to your landing page or website relating to your delegate information
          and statistics.</p>
        <p><b>Example:</b>
        </p>
        <p><code>delegate_update website my-delegate-website.com</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>team</code>
      </td>
      <td style="text-align:left"><b>String</b>  <em>(255 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p>In case of delegate node managed by several persons. Can be a team name,
          names or GitHub profiles.</p>
        <p><b>Example:</b>  <code>delegate_update team Manager: @tic | Treasury: @tac</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>shared_delegate_status</code>
      </td>
      <td style="text-align:left"><b>String </b><em>(255 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p><b>solo: </b>for a solo delegates.</p>
        <p><b>shared</b>: for shared delegates. Shared delegates set up a fee and
          redistribute a share of the reward to their voters.</p>
        <p><b>group</b>: for private groups. Private groups works as shared but the
          delegate decide how he/she going to distribute the votes. See how to set
          it up below.</p>
        <p><b>Example:</b>  <code>delegate_update shared_delegate_status group</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>delegate_fee</code>
      </td>
      <td style="text-align:left">
        <p><b>Number</b>
        </p>
        <p><em>[0 - 100] with up to 6 decimals</em>
        </p>
      </td>
      <td style="text-align:left">
        <p>Fees (in percentage) taken by the shared delegate on the block reward.
          Provide the fee you have used during the node installation.</p>
        <p><b>Example:</b>  <code>delegate_update delegate_fee 12.354321</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>server_specs</code>
      </td>
      <td style="text-align:left"><b>String</b>  <em>(255 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p>A description of the servers&apos; specifications and hardware.</p>
        <p><b>Example:</b>  <code>delegate_update server_specs Operating System = Ubuntu 20.04 - CPU = 6 threads (Intel E5-2630 v4 - 2.20GHz) - RAM = 16GB DDR4 - Hard drive = 400GB SSD - Bandwidth Transfer = Unlimited</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

You will be prompted to wait for the next valid data interval. Once your request has been accepted, you will receive the message **`The delegate info has been updated successfully`**.

You can **`exit`** the wallet and restart the wallet service:

```text
systemctl start xcash-rpc-wallet
```

## 3. Private group

A shared delegate can choose to run a private group, where the delegate decide which voters is getting a share of the block reward. This set up is favorable for people running a shared delegate group with a closed group of people, and only want to retribute this select group.

To change this setting, you will need to make some changes in the `xcash-dpops.service` and create a special configuration file that will list the wallet address you wish to send block reward payments to.

First, you will need to create a configuration file that will list the wallet address you will distribute to.

```bash
nano ~/xcash-official/xcash-dpops-configuration.txt
```

Then, you will need to add the addresses to distribute to in your list following the following format:

```bash
# Comment
wallet1|voting-address|payment-address
wallet2|voting-address|payment-address
```

Where `voting-address` is the wallet address used to vote for the delegate, and `payment-address` is the wallet you want to distribute the share of the block reward to.

{% hint style="info" %}
The `voting-address` and `payment-address` can be the same.
{% endhint %}

{% hint style="info" %}
The maximum amount of `payment-address` that can be in the list is **100**.
{% endhint %}

Once the config file is ready, save it and close. You will need to update the `xcash-dpops.service` so it can get the configuration file. First, run the stop program option in the installer script: 

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

And choose **option 13.** Once the programs are stopped, open the `xcash-dpops.service` located in `/lib/systemd/system/`and add the parameter `--private-group <path-to-configuration-file>`to the ExecStart line.

```bash
nano /lib/systemd/system/xcash-dpops.service
```

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
ExecStart=~/xcash-official/xcash-dpops/build/xcash-dpops --private-group ~/xcash-official/xcash-dpops-configuration.txt --block-verifiers-secret-key BLOCK_VERIFIER_SECRET_KEY
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endtab %}
{% endtabs %}

Once done, save your changes and restart the programs by running the **option 12** of the auto-installer script.

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

