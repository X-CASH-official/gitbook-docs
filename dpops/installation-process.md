---
description: >-
  A step-by-step guide to install and set up the DPoPS program on your XCASH
  node and become a delegate.
---

# Node Program Installation

## Introduction

{% hint style="info" %}
This guide is assuming that you have already followed the step of the [server setup guide](server-setup.md), or that you are confortable and knowledgeable about your Linux instance.
{% endhint %}

This guide will walk you through installing, registering and preparing a delegate node on your instance. Whether you are experienced in Linux or completely new to it, you will be able, at the end of the guide, to have your delegate node running and start securing the X-Cash public network \(provided you get elected as a delegate 😉\).

This guide is splitted in two sections:

* \*\*\*\*[**Quick installation**](installation-process.md#easy-installation): With a simple auto-installer script, you will be able to install the `xcash-dpops` program that will enable you to relay and forge blocks, and participate in the security of the network.  _Linux knowledge is not mandatory to follow the auto-installer script._ 
* \*\*\*\*[**Manual installation**](installation-process.md#manual-installation-process): This step-by-step guide will cover everything about building the `xcash-dpops` program, the dependencies from the source code and managing the services. _It is expected that you are confortable on Linux to follow this guide._

## Requirements

### System Requirements

{% hint style="info" %}
In the first beta version, X-Cash's DPoPS will only run on a Linux/Unix OS. **We recommend installing it on a Ubuntu VPS/dedicated server \(18.04\) for best compatibility.**
{% endhint %}

The delegate node will need to transit a lot of information, notably messages to the other delegates to verify the block informations, As time goes by, the features and information that the delegates handle will increase \(notably when we will develop **token creation**, **NFT**, **sidechains**, **smart contracts** and other exciting features ⭐\). 

{% hint style="info" %}
The recommended system requirement is designed to be "**future-development proof**", meaning that an hardware update should never be needed and still comfortably handle the `xcash-dpops` program.
{% endhint %}

|  | **Minimum**  | **Recommended**  |
| :--- | :--- | :--- |
| **OS** | Ubuntu 18.04 | Ubuntu 18.04 |
| **CPU** | 4 threads, 2.0 GHz or more per thread  | 4 threads, 2.0 GHz or more per thread |
| **RAM** | 6GB | 32GB |
| **Hard Drive** | 50GB  | 2TB |
| **Bandwidth Transfer** | 100GB per month | 500GB per month |
| **Bandwidth Speed** | 100 Mbps | 500 Mbps |

{% hint style="info" %}
 It is estimated that the blockchain size will increase by **18GB per year.**
{% endhint %}

{% hint style="warning" %}
The **minimum requirements** will suffice at the inception of the new consensus. However, as new features are brought to the program, hardware updates will be quickly needed.
{% endhint %}

### Dependencies

The following table summarizes the tools and libraries required to run X-Cash's DPoPS program.

| Dependencies | Min. version | Ubuntu package |
| :--- | :--- | :--- |
| **GCC** | 4.7.3 | `build-essential` |
| **CMake** | 3.0.0 | `cmake` |
| **pkg**-**config** | any | `pkg-config` |
| **OpenSSL** | any | `libssl-dev` |
| **Git** | any | `git` |
| **MongoDB** | 4.0.3 | Install from [binaries](https://www.mongodb.com/download-center/community) |
| **MongoDB C Driver** \(includes BSON libary\) | 1.13.1 | Build from [source](https://github.com/mongodb/mongo-c-driver/releases/) |
| **xcash-core** | Latest version | [download the latest release](https://github.com/X-CASH-official/X-CASH/releases) or [build from source](https://github.com/X-CASH-official/X-CASH#compiling-x-cash-from-source) |

### Time Synchronization

The `xcash-dpops` program uses the system time to calibrate itself and get signal to send and receive data. It is important that the system time is accurate, otherwise the program will not work as intendend.

{% hint style="info" %}
The timezone does not matter and will not affect synchronization.
{% endhint %}

To check your system time, use the following command, and verify that the setting `System clock synchronization: yes` 

```bash
timedatectl
```

If it says `no`, run the following command: 

```bash
timedatectl set-ntp true
systemctl restart systemd-timesyncd
```

And run `timedatectl`again. It should look as follow:

{% code title="$ timedatectl" %}
```text
                      Local time: Mon 2020-06-01 16:18:19 CEST
                  Universal time: Mon 2020-06-01 14:18:19 UTC
                        RTC time: Mon 2020-06-01 14:18:19
                       Time zone: Europe/Berlin (CEST, +0200)
       System clock synchronized: yes
systemd-timesyncd.service active: yes
                 RTC in local TZ: no
```
{% endcode %}

## Installer Script

The Installer Script has been designed to easily interact with the `xcash-dpops` program and provide easy steps for installation and update. You can also use this script to restart the programs if you are not confortable with the command line interface. 

To display the setting menu, run the following command that will fetch the `autoinstaller.sh` script from the official [xcash-dpops](https://github.com/X-CASH-official/xcash-dpops) repository:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

This will open the following settings screen to choose from:

![](../.gitbook/assets/image%20%2817%29.png)

You can select the task by inputting the corresponding number:

1. **Install:** This setting will prepare the necessary directories, download the dependencies, build and  install the `xcash-dpops` program. The steps to follow are stated down below. 
2. **Update:** Updates all the packages and releases of the dependencies to build the program.
3. **Uninstall:** Removes all the files, dependencies and related program of the `xcash-dpops`.
4. **Install / Update Blockchain:** Downloads or updates the X-Cash blockchain data.
5. **Change Solo Delegate or Shared Delegate:** Switches from a solo delegate setup to a shared delegate setup or the other way around.
6. **Edit Shared Delegate Settings:** If you are running a shared delegate setup, changes the fees and minimum payout to voters.
7. **Restart Programs:** Restarts all the programs and services relating to the `xcash-dpops.`
8. **Test Update:** Alpha test feature \(
9. **Test Update Reset Delegates:** Alpha test feature
10. **Firewall:** Install the firewall for solo delegates.
11. **Shared Delegates Firewall:** Install the firewall with paramaters for shared delegates.

## Quick Installation

Once you have prepared your Linux instance by following the [server setup guide](server-setup.md), you can run an installer script to easily install, build and configure the node, as well as download the blockchain. 

#### **Installing the `xcash-dpops` program**

**`xcash-dpops`** is the program needed to run a delegate node. It is responsible for sending messages to the other delegates, organize the consensus, relay and forge new blocks, etc...

To start the installation process, run the `autoinstaller.sh` script: 

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

Choose `Install` \(1\) and press `ENTER`. 

![](../.gitbook/assets/image%20%2819%29.png)

You will be prompted to choose an installation directory: 

![](../.gitbook/assets/image%20%2816%29.png)

Press `ENTER` for the default location \(`/root/xcash-programs/`\), or provide another path in the form `/directory/`

{% hint style="warning" %}
It is recommended to always choose the default location.
{% endhint %}

Next, you will need to choose the blockchain file directory. 

![](../.gitbook/assets/image%20%288%29.png)

Press `ENTER` for the default location \(`/root/.X-CASH/`\).

Next, you will need to choose the directory for the delegate database. 

![](../.gitbook/assets/image%20%2814%29.png)

Press `ENTER` for the default location \(`/data/db/`\).

You will be then asked if you want to install the `xcash-dpops` program as a **shared delegate** or a **solo delegate**.

{% hint style="info" %}
In the X-Cash Public Network consensus, the delegates are voted into the top position using XCASH. In some cases, a delegate who owns a large amount of XCASH could become a delegate by himself, without anyone voting for him/her. He would then be a **solo delegate**.    
Solo delegates do not need to set up fees and minimum payout threshold as there is no need to redistribute the reward.
{% endhint %}

![](../.gitbook/assets/image%20%284%29.png)

Press `ENTER` for the default setting \(`shared delegate`\), or type `No` for `solo delegate`. 

If you choose to install the program as a shared delegate, you will need to set up your delegate fees and the minimum payment amount.

![](../.gitbook/assets/image%20%287%29.png)

{% hint style="info" %}
The **shared delegate fee** is the percentage of fees that is taken from the reward and kept by the delegate. It can be used to pay for the server hosting, to finance project in the X-Cash ecosystem, etc.. 
{% endhint %}

Type in your desired delegate fee \(in %\) with up to 6 decimals. 

{% hint style="info" %}
The `xcash-dpops` program will handle automatically the payment of the voters.  
****The **shared delegate minimum payment amount** is the minimal amount of XCASH to be sent periodically to the delegate's voters. 
{% endhint %}

Type your desired minimum payment amount \(minimum: 10,000 XCASH, maximum: 10M XCASH\) in whole number.

The script will now ask you if you want to create a wallet to collect the block rewards. 

![](../.gitbook/assets/image%20%289%29.png)

Press `ENTER` for the default setting \(`YES`\), or type `No`.

If you say `YES`, the installer will then ask you if you want to automatically generate a password for the wallet or provide a custom one. Press `ENTER` for the default setting \(`YES`\), or type `No` to provide a custom password.

You will then be asked if you want to create a new block verifier secret key, or import an existing one:

![](../.gitbook/assets/image%20%2818%29.png)

Type `I` to import your secret key, or `C` to create a new one.

{% hint style="info" %}
The Block Verifier Key is used by the delegates to sign messages when interacting with the network.   
If it's your first installation of the xcash-dpops program, you shouldn't have generate one before. Choose `Create`  \(`C`\) to automatically generate a new one.  
{% endhint %}

This would be the last setting to provide. Once every steps have been followed, the settings will be summarized as below and the installation will start:

![](../.gitbook/assets/image%20%2815%29.png)

{% hint style="warning" %}
Your wallet password will be displayed again at the end of the installation process. It is however a good idea to save it now.
{% endhint %}

The script will then install automatically the different programs and dependencies needed for the `xcash-dpops` program.

{% hint style="info" %}
The installation script will follow automatically the next step of installation. Some of these steps **might take a while.**   
Indeed, the script will download the blockchain \(~16GB as of 01/06/2020\), create a new wallet and synchronize it.  
Depending on your server connection, this could take up to several hours. 
{% endhint %}

![Screencap of all the steps of the installer script](../.gitbook/assets/image%20%2810%29.png)

Once the script has installed everything, you will be prompted with your X-Cash delegate wallet data.

![](../.gitbook/assets/image%20%2812%29.png)

**Make sure to copy this information in a secure place,** as this is the wallet that will receive the block reward. 

Now that the program is installed, you can go register yourself as a delegate. Follow the [register delegate](set-up-your-delegates.md) guide to continue the node setup. 

## Manual Installation

This guide is designed for people knowledgeable in Linux. If you are not confortable with the Linux distribution, or ifyou are following these steps without understanding what you are doing, you might make mistake that will prevent the xcash-dpops program to run as intended.

### Installation Directories

{% hint style="info" %}
It is recommended to install the program the different programs needed for the `xcash-dpops` in the same folder, in the the `/root/` directory  or the `/home/$USER/` if you are installing from a user different than root.
{% endhint %}

In the `~` directory, create the `xcash-official` directory and the `xcash-wallets` and `logs` directory within: 

```bash
mkdir -p ~/xcash-official/{xcash-wallet,logs}
```

### Install Dependencies

First, update your system packages and install the necessary dependencies: 

```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install build-essential cmake pkg-config libssl-dev git -y
```

If you want to install [`xcash-core`](https://github.com/X-CASH-official/xcash-core) from source, you will need to install [additionnal packages](https://github.com/X-CASH-official/xcash-core#dependencies). 

#### Installing MongoDB

You will need to get the latest stable version \(current release\) on the mongoDB website: [https://www.mongodb.com/download-center/community](https://www.mongodb.com/download-center/community)

![](../.gitbook/assets/image%20%2821%29.png)

**Version:** Latest current release.  
**OS:** Ubuntu 18.04 Linux x64 \(if you are using this version\).  
**Package**: Server

Then copy the link address of the tgz file and use it to download the folder. We recommend installing the mongoDB folder in your installation directory. Replace the link and version \(`x.y.z`\) with the most recent one. 

```bash
cd ~/xcash-official/ && wget https://fastdl.mongodb.org/src/mongodb-src-rx.y.z.tar.gz
```

Then, extract it and remove the downloaded file: 

```bash
tar -xf mongodb-src-r*.tar.gz && rm mongodb-src-r*.tar.gz
```

Additionnaly, create the `/data/db` folder that will store the mongoDB delegate database and give it the right permissions. 

```bash
sudo mkdir -p /data/db && sudo chmod 770 /data/db && sudo chown $USER /data/db
```

Lastly, add the MongoDB folder to your path \(replace the the path with the one you have\) with the following command.

```bash
echo -e '\nexport PATH=/$USER/xcash-official/mongodb-src-r4.2.7:$PATH' >> ~/.profile && source ~/.profile
```

#### Building the MongoDB C Driver from source

First, download the latest stable version of the MongoDB C Driver.  
Go to the [official GitHub repository](https://github.com/mongodb/mongo-c-driver/) and download the latest stable[ release](https://github.com/mongodb/mongo-c-driver/tags). Get the tar gz file in your installation folder:

```bash
cd ~/xcash-official/ && wget https://github.com/mongodb/mongo-c-driver/releases/download/1.16.2/mongo-c-driver-1.16.2.tar.gz
```

Now, build the driver using the following commands \(based on these [instructions](http://mongoc.org/libmongoc/current/installing.html#building-from-a-release-tarball)\):

```bash
tar xzf mongo-c-driver-*.tar.gz && rm mongo-c-driver-*.tar.gz
cd mongo-c-driver-*
mkdir cmake-build && cd cmake-build
cmake -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF ..
sudo make -j `nproc`
sudo make install
```

Then, update the the links and cache to the new libraries:

```bash
sudo ldconfig
```

### Build Instructions

At this point, all the dependencies shoud be installed and built. First, clone the `xcash-dpops` repository:

```bash
cd ~/xcash-official/ && git clone https://github.com/X-CASH-official/xcash-dpops.git
```

Go into the downloaded folder, and build using `make`: 

```bash
cd ~/xcash-official/xcash-dpops
make clean ; make release -j `nproc`
```

Once the build is completed, you will get the `XCASH_DPOPS Has Been Built Successfully` message.

## Test build

{% hint style="info" %}
It is recommended to run the X-CASH DPOPS test before you run the main program. The test will ensure that your system is compatible, and that you have setup your system correctly.
{% endhint %}

{% hint style="warning" %}
To run the X-CASH DPOPS test, make sure to have already started the `XCASH Daemon`, `XCASH Wallet` and `MongoDB` systemd services, and to have stopped the `XCASH DPOPS` systemd service if it was already running.
{% endhint %}

Navigate to the folder that contains the binary, rebuild the binary in debug mode then run the test

```bash
make clean ; make debug -j `nproc`
./XCASH_DPOPS --test
```

The test will return the number of passed and failed test at the bottom of the console. The failed test need to be 0 before you run the node. If the output is not showing 0 for failed test, then you need to scroll through the testing output and find what test failed \(It will be red instead of green\).

If this is a system compatibility test, then you will need to fix the system. If this is a core test that has failed, then you need to possibly rebuild, or [contact us on the DPoPS testing section](https://xcashteam.atlassian.net/servicedesk) or go the the [Discord channel](https://discord.gg/wXFGERr) to get help from testers and the dev-team.

