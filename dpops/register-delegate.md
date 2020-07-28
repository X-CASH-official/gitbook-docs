---
description: >-
  Once your server is set up and the necessary programs installed, you will be
  able to register yourself as a delegate.
---

# Register Delegate

## Introduction

Once you have correctly [set up your instance](server-setup.md), installed the different programs, either from the [installer script](node-installation.md#quick-installation) or [manually](node-installation.md#manual-installation), you can now register as a delegate of the X-Cash Public Network.  
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

You can **`exit`** the wallet, and restart the wallet service:

```text
systemctl restart xcash-rpc-wallet
```

## 2. Update Public Information

Each registered delegates will be displayed in the [delegates explorer](http://delegates.xcash.foundation/), along with their statistics and information. At registration, the minimum information that is displayed is your **delegate name** and **IP address**. You can add additionnal instructions to help other identify you or rally to your cause and vote for you.

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

Replace the `<item>` with one of the list below, and change the correspondig `<value>`

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
      <td style="text-align:left"><code>IP_address</code>
      </td>
      <td style="text-align:left"><b>String</b>  <em>(255 char. max.)</em>
      </td>
      <td style="text-align:left">
        <p>Server <b>IP address</b>
        </p>
        <p><b>Example:</b>
        </p>
        <p><code>delegate_update IP_address 104.23.22.12</code>
        </p>
      </td>
    </tr>
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
      <td style="text-align:left"><b>Boolean</b>
      </td>
      <td style="text-align:left">
        <p><b>True</b> if shared delegate, <b>false</b> if solo delegate.</p>
        <p><b>Example:</b>  <code>delegate_update shared_delegate_status true</code>
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
        <p>Fees (in percentage) taken by the shared delegate on the block reward.</p>
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

## 3. Shared Delegates

If you plan on being a delegate but need to accept people vote to help you place in the top delegates spot and earn the right to forge blocks, you will need to run a shared delegate.  
The shared delegates website will automatically pay your voters taking into account their share.

### Quick Setup \(easy\)

You can run the **`Change Solo Delegate`** or **`Shared Delegate`** or **`Edit Shared Delegate Settings`** option in the auto-installer script to easily update your delegate settings. To open the installer script, run:

```text
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

You will be asked to input a delegate fee and a minimum payout amount to your voters. The script will automatically change the program settings to match your changes.

{% hint style="info" %}
Don't forget to update your delegate fee by [updating the public information](register-delegate.md#2-update-public-information) as well.
{% endhint %}

### Manual Setup

{% hint style="info" %}
We recommend changing from a solo delegate to a shared delegate with the auto-installer script as described above. The manual process described below can be prone to error in the setup.
{% endhint %}

To run your node as a shared delegate, you just need to run the **`xcash-dpops`** program with the following added set of parameters:

```text
--shared-delegates-website --fee <fee> --minimum-amount <amount>
```

With :

* **`<fee>` :** Your delegate fee is the amount of the block reward you are keeping to yourself. The **`<fee>`** is expressed in percentage \(%\) and can take up to 6 decimals. 
* **`<amount>` :** The minimum amount of XCASH that can be sent to voters as part of their payment. Has to be an integer number.

_**Example**_

```text
--shared-delegates-website --fee 12.092032 --minimum-amount 10000
```

Will register as a shared delegate, taking a fee of **`12.092032%`** from the block reward, and process payments to voters when they have accumulated to **`10,000`** XCASH.

#### Update the **`xcash-dpops`** program

To make sure that your node is running with the parameters, you should update the **`unit`** files and restart the service.

First, make sure to stop the running **`xcash-dpops`** service:

```text
systemctl stop xcash-dpops
```

Then, edit the **`unit`** file:

```text
nano /lib/systemd/system/xcash-dpops.service
```

And add the parameters to the **`ExecStart`** variable:

{% code title="XCASH\_DPOPS.service" %}
```text
[Unit]
Description=XCASH DPOPS

[Service]
Type=simple
LimitNOFILE=infinity
User=root
WorkingDirectory=~/xcash-official/xcash-dpops/build
ExecStart=~/xcash-official/xcash-dpops/build/xcash-dpops --block-verifiers-secret-key BLOCK_VERIFIER_SECRET_KEY --shared-delegates-website --fee <fee> --minimum-amount <amount>
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endcode %}

{% hint style="danger" %}
Make sure that the following parameter is in first place:

```text
--block-verifiers-secret-key BLOCK_VERIFIER_SECRET_KEY
```
{% endhint %}

{% hint style="info" %}
Make sure to[ update your public information](register-delegate.md#3-update-public-information) when you change fees, and change **`shared_delegate_status`** to true if you are running a shared delegate node to let people know that they can vote for you.
{% endhint %}

#### **Build the shared delegate website**

{% hint style="info" %}
The **`shared delegate website`** has been automatically installed if you installed it with the auto-installer script.
{% endhint %}

As a shared delegate, you will need to pay your voters a share of the block reward. We have designed a [**`delegate pool website`**](https://github.com/X-CASH-official/delegates-pool-website), which works similarly to a regular PoW pool website, where your voters can see their pending payments, your forging statistics and so on...

First, install the website dependencies:

**NodeJS**

Install nodeJS latest available version:

```text
sudo apt install nodejs
```

And update to the latest version using [this tutorial](https://phoenixnap.com/kb/update-node-js-version).

#### **npm**

{% hint style="info" %}
First, if you are installing from root user, you need to give the current permissions:

```text
npm config set user 0
npm config set unsafe-perm true
```
{% endhint %}

Install **`npm`** globally:

```text
npm install -g npm
```

**Angular & Uglify JS**

Install **`Angular`** and **`UglifyJS`** globally:

```text
npm install -g @angular/cli@latest uglify-js
```

**Build**

Once all dependencies are installed, clone the [**`delegate pool website`**](https://github.com/X-CASH-official/delegates-pool-website) repository :

```text
cd ~/xcash-official && git clone https://github.com/X-CASH-official/delegates-pool-website.git
```

Go into the folder, install the dependencies, and build the website:

```text
cd ~/xcash-official/delegates-pool-website
npm update
npm run build
```

It will build in the **`dist`**folder.

Compress the **`.js`** files with **`Uglify-JS`** and move all of the contents of this folder to your **`xcash-dpops/`** folder:

```text
cd ~/xcash-official/delegates-pool-website/dist
for f in *.js; do echo "Processing $f file.."; uglifyjs $f --compress --mangle --output "{$f}min"; rm $f; mv "{$f}min" $f; done
rm -r ~/xcash-official/xcash-dpops/delegates-pool-website
mkdir ~/xcash-official/xcash-dpops/delegates-pool-website
cd ..
cp -a dist/* ~/xcash-official/xcash-dpops/delegates-pool-website
```

**And you are done** 🎉  
Verify that you are listed in the [delegate explorer](http://delegates.xcash.foundation/), and start advertising your node to get people joining your cause and vote for you!

