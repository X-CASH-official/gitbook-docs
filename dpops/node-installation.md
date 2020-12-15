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

## Installer Script

The **Installer Script** has been designed to easily interact with the **`xcash-dpops`** program and provide easy steps for installation and updates. You can also use this script to restart the programs if you are not comfortable with the command-line interface.

To run the latest version of the installer script, run the following command that will fetch the **`autoinstaller.sh`** script from the official [xcash-dpops](https://github.com/X-CASH-official/xcash-dpops) repository:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

 You will be prompted with the installer menu of the `xcash-dpops` program:

![](../.gitbook/assets/image%20%2824%29.png)

The installation script enables you to install and manage your `xcash-dpops` program easily.  

## Node Program Installation

Once you have prepared your Linux instance by following the [server setup guide](server-setup.md), you can run an installer script to easily install, build and configure the node, as well as download the blockchain.

#### **Installing the** `xcash-dpops` **program**

**`xcash-dpops`** is the program needed to run a delegate node. It is responsible for sending messages to the other delegates, organize the consensus, relay and forge new blocks, etc...

To start the installation process, run the **`autoinstaller.sh`** script:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

Choose **`Install`** \(1\) and press **`ENTER`**.

![](../.gitbook/assets/image%20%2819%29.png)

You will be prompted to choose an installation directory:

![](../.gitbook/assets/image%20%2816%29.png)

Press **`ENTER`** for the default location \(**`/root/xcash-official/`**\), or provide another path in the form **`/directory/`**

{% hint style="warning" %}
**It is recommended to always choose the default location.**
{% endhint %}

Next, you will need to choose the blockchain file directory.

![](../.gitbook/assets/image%20%288%29.png)

Press **`ENTER`** for the default location \(**`/root/.X-CASH/`**\).

Next, you will need to choose the directory for the delegate database.

![](../.gitbook/assets/image%20%2814%29.png)

Press **`ENTER`** for the default location \(**`/data/db/`**\).

You will be then asked if you want to install the **`xcash-dpops`** program as a **shared delegate** or a **solo delegate**.

{% hint style="info" %}
In the X-Cash Public Network consensus, the delegates are voted into the top position using the XCASH cryptocurrency.  
In some cases, a delegate who owns a large amount of XCASH could become a delegate by himself, without anyone voting for him/her. We are calling them **solo delegate**.  
Solo delegates do not need to set up fees and minimum payout threshold as there is no need to redistribute the reward.
{% endhint %}

![](../.gitbook/assets/image%20%284%29.png)

Press `ENTER` for the default setting \(`shared delegate`\), or type `No` for `solo delegate`.

If you choose to install the program as a shared delegate, you will need to set up your delegate fees and the minimum payment amount.

![](../.gitbook/assets/image%20%287%29.png)

{% hint style="info" %}
The **shared delegate fee** is the percentage of fees that are taken from the reward and kept by the delegate. It can be used to pay for the server hosting, to finance projects in the X-Cash ecosystem, etc..
{% endhint %}

Type in your desired delegate fee **in % with up to 6 decimals**.

{% hint style="info" %}
The **`xcash-dpops`** program will handle automatically the payment of the voters with a dedicated **delegate pool website**.  
The shared delegate **minimum payment amount** is the minimal amount of XCASH to be sent periodically to the delegate's voters.
{% endhint %}

Type your desired minimum payment amount \(minimum: 10000 XCASH, maximum: 10M XCASH\) as a whole number.

The script will now ask you if you want to create a wallet to collect the block rewards.

![](../.gitbook/assets/image%20%289%29.png)

Press **`ENTER`** for the default setting \(**`YES`**\), or type **`No`**.

If you say **`YES`**, the installer will then ask you if you want to automatically generate a password for the wallet or provide a custom one. Press **`ENTER`** for the default setting \(**`YES`**\), or type **`No`** to provide a custom password.

You will then be asked if you want to create a new block verifier secret key, or import an existing one:

![](../.gitbook/assets/image%20%2818%29.png)

Type **`I`** to import your secret key, or **`C`** to create a new one.

{% hint style="info" %}
The **Block Verifier Key** is used by the delegates to sign messages when interacting with the network.  
If it's your first install and you don't have one, choose **`Create`** \(**`C`**\) to automatically generate a new one.
{% endhint %}

This would be the last setting to provide. Once every steps have been followed, the settings will be summarized as below and the installation will start:

![](../.gitbook/assets/image%20%2815%29.png)

{% hint style="warning" %}
Your wallet password will be displayed again at the end of the installation process. It is however a good idea to save it now.
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
By default, the wallet is named **`delegate-wallet`** and is stored in **`/root/xcash-official/xcash-wallets/`**
{% endhint %}

Now that the program is installed, your delegate wallet initialized, and the different services running, you can go register yourself as a delegate. Follow the [register delegate](register-delegate.md) guide to continue the node setup.

## Manual Installation

{% hint style="danger" %}
This guide hasn't been updated since May 2020. It is highly recommended to install the xcash-dpops program with the installer script provided above. 
{% endhint %}

{% hint style="warning" %}
This guide is designed for people knowledgeable in Linux and who want to install everything from scratch. If you are not comfortable with the Linux distribution, or if you are following these steps without understanding what you are doing, you might make a mistake that will prevent the **`xcash-dpops`** program to run as intended.
{% endhint %}

#### 1. Installation Directories

{% hint style="info" %}
It is recommended to install the program the different programs needed for the **`xcash-dpops`** in the same folder, in the the **`/root/`** directory or the **`/home/$USER/`** if you are installing from a user different than root.
{% endhint %}

In the **`~`** directory, create the **`xcash-official`** directory and the **`xcash-wallets`** , **`systemdpid`** , and `logs` directory within. Additionally, create the **`.X-CASH`** directory that will store the X-Cash blockchain file.

```bash
mkdir -p ~/xcash-official/{xcash-wallets,logs,systemdpid}
mkdir -p ~/.X-CASH
```

#### 2. Install Dependencies

First, update your system packages and install the necessary dependencies:

```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install build-essential cmake pkg-config libssl-dev git -y
```

If you want to install [`xcash-core`](https://github.com/X-CASH-official/xcash-core) from source, you will need to install these [additional packages](https://github.com/X-CASH-official/xcash-core#dependencies).

```bash
sudo apt install libboost-all-dev libzmq3-dev libunbound-dev libsodium-dev libminiupnpc-dev libunwind8-dev liblzma-dev libreadline6-dev libldns-dev libexpat1-dev libgtest-dev doxygen graphviz libpcsclite-dev screen p7zip-full -y
```

#### 2.a\) Installing MongoDB

You will need to get the latest stable version \(current release\) on the MongoDB website: [https://www.mongodb.com/download-center/community](https://www.mongodb.com/download-center/community)

![](../.gitbook/assets/image%20%2822%29.png)

**Version:** Latest current release.  
**OS:** Ubuntu 18.04 Linux x64 \(if you are using this version\).  
**Package**: Server

Then click on **All version binaries**.

![](../.gitbook/assets/image%20%2821%29.png)

Copy the link address of the tgz file of the latest version available \(disregarding debug symbols version\) and use it to download the installation folder using this command \(replacing the link with the most recent version\).

```bash
cd ~/xcash-official && wget http://downloads.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1804-v4.4-latest.tgz
```

Then, extract it and remove the downloaded file:

```bash
tar -xf mongodb-linux-x86_64*.tgz && rm mongodb-linux-x86_64*.tgz
```

Lastly, add the MongoDB binaries folder to your path with the following command.

```bash
echo -e '\nexport PATH=$USER/xcash-official/mongodb-linux-x86_64-ubuntu1804-4.4.0-rc7-36-gcf4ac31/bin:$PATH' >> ~/.profile && source ~/.profile
```

Replace `$USER/xcash-official/mongodb-linux-x86_64-ubuntu1804-4.4.0-rc7-36-gcf4ac31/bin` with the MongoDB binaries folder in your system.

Additionally, create the `/data/db` folder that will keep the delegates databases:

```bash
sudo mkdir -p /data/db && sudo chmod 770 /data/db && sudo chown $USER /data/db
```

#### 2.b\) Building the MongoDB C Driver from source

First, download the latest stable version of the MongoDB C Driver.  
Go to the [official GitHub repository](https://github.com/mongodb/mongo-c-driver/) and download the latest stable[ release](https://github.com/mongodb/mongo-c-driver/tags). Get the tarball file in your installation folder:

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

Then, update the links and cache of the newly installed library:

```bash
sudo ldconfig
```

#### 2.c\) Building `xcash-core` from source

Clone the `xcash-core` repository to your installation folder and go to the downloaded folder:

```bash
cd ~/xcash-official/ && git clone https://github.com/X-CASH-official/xcash-core.git
cd xcash-core
git checkout --quiet xcash_proof_of_stake
```

Make sure to have all the [dependencies](node-installation.md#install-dependencies) installed, and build the binaries using `make`:

```bash
make clean
make release -j `nproc`
```

Once the build finishes, the binaries will be located in `~/xcash-official/xcash-core/build/release/bin`

#### 3. Build Instructions

At this point, all the dependencies should be installed and built. First, clone the **`xcash-dpops`** repository:

```bash
cd ~/xcash-official/ && git clone https://github.com/X-CASH-official/xcash-dpops.git
```

Go into the downloaded folder, and build using `make`:

```bash
cd ~/xcash-official/xcash-dpops
make clean ; make release -j `nproc`
```

Once the build is completed, you will get the `xcash-dpops Has Been Built Successfully` message. Now that the program is built, you will need to generate a wallet to be used for the delegate and set up the different `units` for `systemd` to organize how your server manages the different services.

#### 4. Generate a Wallet

You will need to create a wallet to register as a delegate, to receive the block reward if you are elected as a top delegate and if you are a shared delegate, the payments will be sent from this wallet as well.

To generate a new wallet, use the following command:

```bash
~/xcash-official/xcash-core/build/release/bin/xcash-wallet-cli --generate-new-wallet ~/xcash-official/xcash-wallet/<WALLET_NAME> --password <PASSWORD> --mnemonic-language English --restore-height 0 --daemon-address <REMOTE_NODE>
```

Replace **`<WALLET_NAME>`**, **`<PASSWORD>`** and the **`<REMOTE_NODE>`** with the parameters of your choice.

{% hint style="info" %}
The wallet synchronization can take time the first time. It will depend on which remote node you chose, and your servers internet connection.
{% endhint %}

{% hint style="danger" %}
**Make sure that you write down your mnemonic key and store it in a secure place** as it will be the only way to restore your wallet in case of a problem. Failing to do so _will_ result in loss of funds.
{% endhint %}

The wallet files will be located in **`~/xcash-official/xcash-wallets/`**

#### 5. Setup The Services

In `systemd`, a `unit` refers to any resource that the system knows how to operate on and manage. This is the primary object that the `systemd` tools know how to deal with. These resources are defined using configuration files called **unit files**.

On this guide, we will set up the different unit files to manage the programs needed to run your delegate node. The `unit` files template are present in the `xcash-dpops/scripts/systemd` folder, but will need to be adjusted with your delegate information.

#### 5.a\) Initialization

Create two empty `PID` files in the `systemdpid` folder previously created, that will manage the corresponding services:

```bash
touch ~/xcash-official/systemdpid/mongod.pid ~/xcash-official/systemdpid/xcash-daemon.pid
```

#### 5.b\) mongodb Service

Edit the systemd unit file `mongodb.service` from in the `xcash-dpops/scripts/systemd`folder :

```bash
nano ~/xcash-official/xcash-dpops/scripts/systemd/mongodb.service
```

You will get the following **`unit`**file:

{% code title="mongodb.service" %}
```bash
[Unit]
Description=MongoDB X-Cash Database Server
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
{% endcode %}

In the file, replace the following if needed:

* **`User`**: User of the system \(most likely `root`\)
* **`PIDFile`**: The path to `mongod.pid` file that  you created at the initialization step. 
* **`ExecStart`**: 
  * Replace the path to the `mongod` file.
  * Replace the path to the database directory \(`/data/db` as per the instructions\) 

#### 5.c\) xcash-daemon Service

Edit the systemd unit file `xcash-daemon.service` from in the `xcash-dpops/scripts/systemd`folder :

```bash
nano ~/xcash-official/xcash-dpops/scripts/systemd/xcash-daemon.service
```

You will get the following **`unit`**file:

{% code title="xcash-daemon.service" %}
```bash
[Unit]
Description=X-CASH Daemon background process

[Service]
Type=forking
User=root
PIDFile=~/xcash-official/systemdpid/xcash-daemon.pid
ExecStart=~/xcash-official/xcash-core/build/release/bin/xcashd --rpc-bind-ip 0.0.0.0 --p2p-bind-ip 0.0.0.0 --rpc-bind-port 18281 --restricted-rpc --confirm-external-bind --log-file ~/xcash-official/logs/xcash-daemon-log.txt --max-log-file-size 0 --detach --pidfile ~/xcash-official/systemdpid/xcash-daemon.pid
RuntimeMaxSec=15d
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endcode %}

In the file, replace the following if needed:

* **`User`**: User of the system \(most likely `root`\)
* **`PIDFile`**: The path to `xcash-daemon.pid` file that you created at the initialization step. 
* **`ExecStart`**: 
  * Replace the path to the `xcashd` file. 
  * Replace the path to the `xcash-daemon_Log.txt` file.
  * Replace the path to the `xcash-daemon.pid` file.

#### 5.d\) xcash-rpc-wallet Service

Edit the systemd unit file `xcash-rpc-wallet.service` from in the `xcash-dpops/scripts/systemd`folder :

```bash
nano ~/xcash-official/xcash-dpops/scripts/systemd/xcash-rpc-wallet.service
```

You will get the following **`unit`** file:

{% code title="xcash-rpc-wallet.service" %}
```bash
[Unit]
Description=X-Cash RPC Wallet background service

[Service]
Type=simple
User=root
ExecStart=~/xcash-official/xcash-core/build/release/bin/xcash-wallet-rpc --wallet-file ~/xcash-official/xcash-wallets/WALLET --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon
Restart=always

[Install]
WantedBy=multi-user.target
```
{% endcode %}

In the file, replace the following if needed:

* **`User`**: User of the system \(most likely `root`\)
* **`ExecStart`**: 
  * Replace the path to the **`xcash-wallet-rpc`** file.
  * Replace the path to the **`xcash-wallets`** folder, and replace **`WALLET`** by your delegate wallet name.
  * Replace **`PASSWORD`** with your delegate wallet password.

#### 5.e\) Firewall Service

Edit the systemd unit file **`firewall.service`** from in the **`xcash-dpops/scripts/systemd`**folder :

```bash
nano ~/xcash-official/xcash-dpops/scripts/systemd/firewall.service
```

You will get the following **`unit`** file:

{% code title="firewall.service" %}
```bash
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
{% endcode %}

In the file, replace the following if needed:

* **`User`**: User of the system \(most likely `root`\)
* **`ExecStart`**: Replace the path to the `firewall/firewall_script.sh` file.

#### 5.f\) xcash-dpops Service

Edit the systemd unit file **`xcash-dpops.service`** from in the **`xcash-dpops/scripts/systemd`**folder :

```bash
nano ~/xcash-official/xcash-dpops/scripts/systemd/xcash-dpops.service
```

You will get the following **`unit`** file:

{% code title="xcash-dpops.service" %}
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
{% endcode %}

In the file, replace the following if needed:

* **`User`**: User of the system \(most likely `root`\)
* **`WorkingDirectory`**: Replace the path of the `xcash-dpops/build` folder.
* **`ExecStart`**: 
  * Replace the path to the **`xcash-dpops`** file.
  * Replace `BLOCK_VERIFIER_SECRET_KEY`  with your generated verifier secret key. **This should be the first parameter.**

{% hint style="info" %}
The instructions to generate the block verifier secret key is given in the [generate block verifier key](node-installation.md#generate-a-block-verifier-key) guide. Make sure to update the file with your **`Block Verifier Secret Key`** and restart the service.
{% endhint %}

#### 5.g\) Install and reload

Now that you have prepared all the `unit` systemd files, you will need to copy all of them to the `system` folder:

```bash
cp -a ~/xcash-official/xcash-dpops/scripts/systemd/* /lib/systemd/system/
```

Then, reload `systemd` to take the changes into account and run the services.

```bash
systemctl daemon-reload
```

#### 6. Generate a Block Verifier Key

The **block verifier key** is a unique identifier generated by a delegate and is used in the consensus process to sign messages and verify the information. As a delegate, you will need to generate a **block verifier key** before being able to register yourself in the system.

First of all, stop the currently running process of the **`xcash-dpops`** program.

```text
systemctl stop xcash-dpops
```

Then, run the **`xcash-dpops`** program with the `--generate-key` option. The program is located in the `xcash-dpops/build` folder. Run the command:

```text
~/xcash-official/xcash-dpops/build/xcash-dpops --generate-key
```

You will be given a **block verifier** **public key** and **block verifier** **private key**.

{% code title="Block Verifier Key" %}
```text
XCASH DPOPS - Version 1.0.0
---------------------------
Public Key:
cf8718d638ce0a831f3538ea60d1e27c3a258c7004a1ad7c547cc5331de7d9d7
Secret Key:
ca1319431124f55fa0d9e0fcc84edd780f80da7d91f08d334fac17c803934ecccf8718d638ce0a831f3538ea60d1e27c3a258c7004a1ad7c547cc5331de7d9d7
```
{% endcode %}

{% hint style="danger" %}
**Securely save the key you will be using!** Once you have registered as a delegate with this key pair, you will be identified in the system with this.  
If you lose it, you will lose your delegate stats and will have to start over.
{% endhint %}

First of all, stop the currently running process of the **`xcash-dpops`** program.

```text
systemctl stop xcash-dpops
```

Then, add your block verifier secret key as a parameter in the service. See the [setup of the services](node-installation.md#6-xcash-dpops-service) guide to see how to edit the unit file.

Once done, make sure to save the new changes to **`systemctl`** by using the following command:

```text
systemctl daemon-reload
```

#### 7. Test build

{% hint style="info" %}
It is recommended to run the **`xcash-dpops`** `test` before you run the main program. The test will ensure that your system is compatible and that you have set up your system correctly.
{% endhint %}

{% hint style="warning" %}
To run the X-CASH DPOPS test, make sure to have already started the `xcash-daemon`, `xcash-rpc-wallet` and `mongodb` systemd services, and to have stopped the `xcash-dpops` systemd service if it was already running.
{% endhint %}

Navigate to the folder that contains the binary, rebuild the binary in debug mode then run the test

```bash
make clean ; make debug -j `nproc`
cd build && ./xcash-dpops --test
```

The test will return the number of passed and failed tests at the bottom of the console. The failed test need to be 0 before you run the node. If the output is not showing 0 for a failed test, then you need to scroll through the testing output and find what test failed \(It will be red instead of green\).

If this is a system compatibility test, then you will need to fix the system. If this is a core test that has failed, then you need to possibly rebuild, or [contact us on the DPoPS testing section](https://xcashteam.atlassian.net/servicedesk) or go the [Discord channel](https://discord.gg/wXFGERr) to get help from testers and the dev-team.

