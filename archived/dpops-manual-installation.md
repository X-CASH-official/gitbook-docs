# DPOPS Manual Installation

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

Make sure to have all the [dependencies](../dpops/node-installation.md#install-dependencies) installed, and build the binaries using `make`:

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
The instructions to generate the block verifier secret key is given in the [generate block verifier key](../dpops/node-installation.md#generate-a-block-verifier-key) guide. Make sure to update the file with your **`Block Verifier Secret Key`** and restart the service.
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

Then, add your block verifier secret key as a parameter in the service. See the [setup of the services](../dpops/node-installation.md#6-xcash-dpops-service) guide to see how to edit the unit file.

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

## Shared delegates Manual Installation

If you plan on being a delegate but need to accept people vote to help you place in the top delegates spot and earn the right to forge blocks, you will need to run a shared delegate.  
The shared delegates website will automatically pay your voters taking into account their share.

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
Make sure to[ update your public information](../dpops/register-delegate.md#3-update-public-information) when you change fees, and change **`shared_delegate_status`** to true if you are running a shared delegate node to let people know that they can vote for you.
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

