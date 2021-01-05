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
* **`<IP_address|domain_name>`** : your server's IP address or its domain name \(if you have bought a domain name and correctly set up the DNS record\). It is possible to change this information at a later time. _Can be updated._
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
      <td style="text-align:left"><b>String</b>
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
        <p><b>Example:</b>  <code>delegate_update server_specs Operating System : Ubuntu 18.04 | CPU : 8 threads (Intel E3-1246 v3 - 3.50GHz RAM = 32GB DDR3 | Hard drive = 2x HDD SATA 2 TB | Bandwidth Transfer = Unlimited</code>
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

## 3. Private group and shared delegates

Shared delegates



