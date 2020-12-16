---
description: >-
  A step-by-step guide to install and set up the DPoPS program on your XCASH
  node and become a delegate.
---

# Node Program Installation

## Introduction

{% hint style="info" %}
This guide is assuming that you have already followed the step of the [server setup guide](server-setup.md), or that you are comfortable and knowledgeable about your Linux instance.
{% endhint %}

This guide will walk you through installing, registering, and preparing a delegate node on your instance. Whether you are experienced in Linux or completely new to it, you will be able, at the end of the guide, to have your delegate node running and start securing the X-Cash public network \(provided you get elected as a delegate 😉\).

The `xcash-dpops` program is used to manage the block validation, communicate with the other delegates and forge blocks. It has also been designed to automatically retribute the voters of their shares.  

## Requirements

### System Requirements

{% hint style="info" %}
In the first beta version, X-Cash's DPoPS will only run on a Linux/Unix OS. **We recommend installing it on a Ubuntu VPS/dedicated server \(18.04\) for the best compatibility.**
{% endhint %}

The delegate node will need to transit a lot of information, notably messages to the other delegates to verify the block information, As time goes by, the features and information that the delegates handle will increase \(notably when the X-Cash Foundation will add **token creation**, **NFT**, **sidechains**, **smart contracts,** and other exciting features ⭐\).

{% hint style="info" %}
The recommended system requirement is designed to be **future-development proof**, meaning that a hardware update should never be needed and still comfortably handle the **`xcash-dpops`** program.
{% endhint %}

|  | **Minimum** | **Recommended** |
| :--- | :--- | :--- |
| **OS** | Ubuntu 18.04 | Ubuntu 18.04 or 20.04 |
| **CPU** | 4 threads, 2.0 GHz or more per thread | 4 threads, 2.0 GHz or more per thread |
| **RAM** | 6GB | 32GB |
| **Hard Drive** | 50GB | 2TB |
| **Bandwidth Transfer** | 100GB per month | 500GB per month |
| **Bandwidth Speed** | 100 Mbps | 500 Mbps |

{% hint style="info" %}
It is estimated that the blockchain and decentralized database size will increase by **9GB per year.** 
{% endhint %}

{% hint style="warning" %}
The **minimum requirements** will suffice at the inception of the new consensus. However, as new features are brought to the program and the blockchain, hardware updates will be needed.
{% endhint %}

### Dependencies

{% hint style="info" %}
The dependencies will be automatically installed and updated with the installation script.
{% endhint %}

The following table summarizes the tools and libraries required to run **`xcash-dpops`** program.

| Dependencies | Min. version | Ubuntu package |
| :--- | :--- | :--- |
| **GCC** | 4.7.3 | `build-essential` |
| **CMake** | 3.0.0 | `cmake` |
| **pkg**-**config** | any | `pkg-config` |
| **OpenSSL** | any | `libssl-dev` |
| **Git** | any | `git` |
| **MongoDB** | 4.0.3 | Install from [binaries](https://www.mongodb.com/download-center/community) |
| **MongoDB C Driver** \(includes BSON library\) | 1.13.1 | Build from [source](https://github.com/mongodb/mongo-c-driver/releases/) |
| **xcash-core** | Latest version | [download the latest release](https://github.com/X-CASH-official/X-CASH/releases) or [build from source](https://github.com/X-CASH-official/X-CASH#compiling-x-cash-from-source) |

### Time Synchronization

The **`xcash-dpops`** program uses the system time to calibrate itself and get the signal to send and receive data. It is important that the system time is accurate, otherwise, the program will not work as intended.

To check your system time, use the following command, and verify that the setting `System clock synchronization: yes`

```bash
timedatectl
```

If it says `no`, run the following command:

```bash
timedatectl set-ntp true
systemctl restart systemd-timesyncd
```

## Installation

### Installer Script

The **Installer Script** has been designed to easily interact with the **`xcash-dpops`** program and provide easy steps for installation and updates. You can also use this script to restart the programs if you are not comfortable with the command-line interface.

To run the latest version of the installer script, run the following command that will fetch the **`autoinstaller.sh`** script from the official [xcash-dpops](https://github.com/X-CASH-official/xcash-dpops) repository:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

 You will be prompted with the installer menu of the `xcash-dpops` program:

![](../.gitbook/assets/image%20%2824%29.png)

The installation script enables you to install and manage your `xcash-dpops` program easily.  

### Installing the X-Cash DPoPS program

{% hint style="info" %}
Before starting, it is recommended to start the installer script in a multiplexer window such as [byobu ](https://www.byobu.org/)to avoid corrupted installation in case of SSH disconnection.
{% endhint %}

Once you have prepared your Linux instance by following the [server setup guide](server-setup.md), you can run the installer script to easily install, build and configure the node, as well as download the blockchain.

**`xcash-dpops`** is the program needed to run and manage a delegate node. It is responsible for sending messages to the other delegates, organize the consensus, relay and forge new blocks, etc...

To start the installation process, run the latest version of the **`autoinstaller.sh`** script:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

Choose **`Install`** \(1\) in the `X-Cash DPoPS Installation Settings` section and press **`ENTER`**

#### 1. Installation Directories

You will be prompted to choose different installation directories for:

* The `xcash-dpops` program
* The blockchain file
* The delegate database

{% hint style="warning" %}
We **very highly** recommend using the default locations, as several update and installation programs are referencing to these paths. 
{% endhint %}

#### 2. Delegate mode

You will be then asked if you want to configure the **`xcash-dpops`** program as a **shared delegate** or a **solo delegate**.

**Shared Delegates** - Rely on the vote of others to be elected into the top 50 and get a forging position. Voters are compensated at the rate of their vote and receive a share \(minus the delegate fee\) of the block reward.

**Solo Delegates** - In some cases, delegates who owns a consequent amount of XCASH can be "elected" in the top 50 by voting for themselves. They don't need to setup fees and will keep the block reward for themselves. 

{% hint style="info" %}
You will be able to change this setting whenever you want in the installer script menu \(option 9\). 
{% endhint %}

Press `ENTER` for the default setting \(`shared delegate`\), or type `No` for `solo delegate`.

If you choose to install the program as a shared delegate, you will need to set up your delegate fee and the minimum payment amount.

![](../.gitbook/assets/image%20%287%29.png)

{% hint style="info" %}
The **shared delegate fee** is the percentage of fees that are taken from the reward and kept by the delegate. It can be used to pay for the server hosting, to finance projects in the X-Cash ecosystem, etc... 
{% endhint %}

Type in your desired delegate fee expressed **in % with up to 6 decimals**.

{% hint style="info" %}
The **`xcash-dpops`** program will set up a wallet service that will automatically handle the payment of the voters, just like a mining pool would.  
The shared delegate **minimum payment amount** is the minimal threshold of XCASH to be sent periodically to the delegate's voters.
{% endhint %}

Type your desired minimum payment amount \(minimum: 10,000 XCASH, maximum: 10M XCASH\) as a whole number.

#### 3. Block reward wallet

You will be asked to provide a wallet to receive the block reward when you are forging a new block. 

You can either restore an existing wallet or choose to create a new wallet.

![](../.gitbook/assets/image%20%289%29.png)

Leave empty or type **`YES`** to **create a new wallet**, or type No to choose to **restore an existing wallet** using the mnemonic seed. 

**Create a new wallet -** If you choose to create a new wallet \(**`YES`**\), the installer will then ask you if you want to automatically generate a password for the wallet or provide a custom one. Press **`ENTER`** for the default setting \(**`YES`**\), or type **`No`** to provide a custom password.

**Restore a wallet -** If you choose to not to create a new wallet and restore an existing one instead \(**`No`**\), the installer will ask you to provide your wallet mnemonic seed and a new password.

#### **4. Block Verifier Keys**

Your block verifier key is used to identify yourself as a delegate. It is mandatory to store it securely. You will be asked if you want to create a new block verifier secret key, or import an existing one:

![](../.gitbook/assets/image%20%2818%29.png)

Type **`I`** to import your secret key, or **`C`** to create a new one.

**Generate the block verifier keys -** If it is your first time setting up a delegate node, you will need to generate a new block verifier key pair \(`C`\). By choosing to generate a new key pair, it will automatically be added to your `xcash-dpops` settings 

**Import the block verifier keys -** If you already have a block verifier key, you can choose to import it \(`I`\). The program will then ask you to give your **block verifier private key**.   
You will most likely already have a block verifier key pair if you have already registered as a delegate in the past and are just trying to reinstall the node program.

#### 5. Finishing 

Once these settings have been provided, you will have an installation summary displayed.

![](../.gitbook/assets/image%20%2815%29.png)

{% hint style="warning" %}
The wallet password you have chosen or generated automatically will be displayed again at the end of the installation process. It is however a good idea to save it now.
{% endhint %}

The script will then install automatically the different programs and dependencies needed for the **`xcash-dpops`** program.

{% hint style="info" %}
The installation script will follow automatically the next step of installation. Some of these steps **might take a while.**  
Indeed, the script will download the blockchain \(`~16GB as of 01/06/2020`\), create a new wallet and synchronize it.  
Depending on your server connection and specifications this could take up to several hours.
{% endhint %}

![Screencap of all the steps of the installer script](../.gitbook/assets/image%20%2810%29.png)

Once the script has installed everything, you will be prompted with your **X-Cash Delegate wallet data**.

![](../.gitbook/assets/image%20%2812%29.png)

{% hint style="danger" %}
**Make sure to copy this information in a secure place,** as this is the wallet that will receive the block reward. You will need the block verifiers key to **register yourself as a delegate**.  
Losing this information _will_ result in a loss of funds.
{% endhint %}

{% hint style="info" %}
By default, the wallet is named **`delegate-wallet`** and is stored in **`~/xcash-official/xcash-wallets/`**
{% endhint %}

Now that the program is installed, your delegate wallet initialized, and the different services running, you can go register yourself as a delegate. Follow the [register delegate](register-delegate.md) guide to continue the node setup.

## 

