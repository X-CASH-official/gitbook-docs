---
description: >-
  These page is needed only by the people who were selected to join the
  alpha-test phase.
---

# Test phase \(alpha\)

## Introduction

The test phase will permit to evaluate certain features of the Delegated-Proof-of-Private-Stake. Each test phase will focus on validating a set of features with a minimum amount of participants before going to the next phase.

### 1. Alpha Close Testnet

**Period:** _07/10/2019 - 21/10/2019_  
**Number of participants:** _20_  
**Sign-up form:** [**Google Form** ](https://forms.gle/RJy1Mke86VeCsJTc8)\*\*\*\*

The alpha-test will focus into testing the core functions of the protocol, with a 20 delegates system \(One fifth of the final system\). What will be validated during this phase includes but is not limited to:

* Decentralized database syncing
* Block creation
* Delegates website
* X-Cash DPoPS core code

During the test phase, the seed nodes will start the blockchain at a pre-determined time. Once the 20 delegate slots have been filled and finished the [set up process](installation-process.md), all delegates will sync the test version of the blockchain and the forging of the blocks will start.

The aim of the test is to assess the stability for the first few days. Once stability is verified and admitted, the system can be forced or pushed to find exploit, bugs etc... through buffer overflow, large string injections or any other methods.

{% hint style="info" %}
A interesting test to carry out for the first phase is closing the program during a round where the user is the elected delegate to forge the block.
{% endhint %}

**What to report:** Unexpected behaviours, any `Function error` print out, and segmentation fault crashes during the process should be reported in the [Discord channel](https://discord.gg/TVw4gJT). Discovered bug and vulnerabilities in the code in the scope of the [Bug Bounty](../contribute/bug-bounty-program.md) need to be reported as per the instructions and guidelines of the program.

### 2. Beta Opened Testnet

**Period:** _21/10/2019 - 01/11/2019 \(indicative\)_  
**Number of participants:** _100_  
**Sign-up form:** _TBD_

{% hint style="info" %}
Dates and general information are subject to changes, and will depend entirely from the results of the alpha phase.
{% endhint %}

## I. Setup the Test **Environment**

### Create test folder

Create a `XCASH_DPOPS_Test` folder in the `Installed-Programs` folder.

{% hint style="warning" %}
Make sure you have installed the packages to [build XCASH from source](https://github.com/X-CASH-official/X-CASH#compiling-x-cash-from-source).
{% endhint %}

Copy the XCASH and XCASH\_DPOPS folders from the `Installed-Programs` folder to the `XCASH_DPOPS_Test` folder

```bash
cp -a ~/Installed-Programs/X-CASH ~/Installed-Programs/XCASH_DPOPS_Test/X-CASH 
cp -a ~/Installed-Programs/XCASH_DPOPS ~/Installed-Programs/XCASH_DPOPS_Test/XCASH_DPOPS
```

Navigate to each folder and change the branch to `xcash_proof_of_stake` branch, and build the binaries.

```bash
cd ~/Installed-Programs/XCASH_DPOPS_Test/X-CASH
git checkout xcash_proof_of_stake
make clean ; make release -j `nproc`
```

Create a wallet file for the wallet you are going to register.

{% hint style="danger" %}
**Make sure this is an empty wallet.** This should be a different wallet than the wallet you plan to register for the official DPoPS, to keep your wallets privacy until the official DPoPS.
{% endhint %}

```bash
cd ~/Installed-Programs/XCASH_DPOPS_Test/X-CASH/build/release/bin
./xcash-wallet-cli
```

Register your wallet with the X-Cash team to get **`XCASH_DPOPS_TEST` XCASH** sent to the wallet

Create a `XCASH_DPOPS_Blockchain_Test` folder in the `Installed-Programs`

```bash
cd ~/Installed-Programs
mkdir XCASH_DPOPS_Blockchain_Test
```

And then stop all of the systemd services

```text
systemctl stop MongoDB
systemctl stop XCASH_Daemon
systemctl stop XCASH_Wallet
systemctl stop XCASH_DPOPS
```

You will now have to import the blockchain. You can either [download the test blockchain](set-up-the-test-alpha-phase.md#download-the-test-blockchain) from our servers, or[ create your own](set-up-the-test-alpha-phase.md#create-the-test-blockchain-from-mainnet-blockchain) from a fully-synchronized one.

### Download the test blockchain

Download the `XCASH test blockchain` from our blockchain download server.

```bash
cd ~/Installed-Programs/XCASH_DPOPS_Test/
wget http://147.135.68.247:8000/XCASH_DPOPS_Blockchain_Test
```

Import the `XCASH_DPOPS_Blockchain` and save the blockchain in the `XCASH_DPOPS_Blockchain_Test` folder

```bash
/root/Installed-Programs/XCASH_DPOPS_Test/X-CASH/build/release/bin/xcash-blockchain-import -
```

### Create the test blockchain from mainnet blockchain

Copy the `.X-CASH` folder at `~/X.CASH` to the `XCASH_DPOPS_Test` folder and remove all the blocks up to `440875`

```bash
cp -a ~/.X-CASH /root/Installed-Programs/XCASH_DPOPS_Test/XCASH_DPOPS_Blockchain_Test
/root/Installed-Programs/XCASH_DPOPS_Test/X-CASH/build/release/bin/xcash-blockchain-import --pop-blocks NUMBER_OF_BLOCKS_TO_REMOVE
```

{% hint style="info" %}
Make sure to change`NUMBER_OF_BLOCKS_TO_REMOVE` by the number of blocks needed to subtract to arrive at the `440875` block.
{% endhint %}

### Last set of configuration

After the blockchain has been imported configure the `XCASH_Daemon`, `XCASH_Daemon_Block_Verifier`, `XCASH_Wallet` and `XCASH_DPOPS` systemd service files for the `XCASH_DPOPS test`.

{% hint style="info" %}
If you followed thoroughly the steps until now, you should just have to add `/XCASH_DPOPS_Test` to every full path in the [systemd service files](set-up-your-node.md#set-up-and-install-the-systemd-files).
{% endhint %}

Once it's done you can reload the systemd service files:

* Reload `systemd` after you have made any changes to the `systemd` service files

```text
systemctl daemon-reload
```

* Start all of the `systemd` services

```bash
systemctl start MongoDB
systemctl start XCASH_Daemon
systemctl start XCASH_Wallet
systemctl start XCASH_DPOPS
```

You can now verify if every information has been correctly imported and if you are ready for the tests.

* Check the block height of the XCASH\_DPOPS\_Blockchain\_Test \(it should be `440875`\)

```bash
curl -X POST http://127.0.0.1:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_count"}' -H 'Content-Type: application/json'
```

* Check if your wallet has XCASH\_DPOS\_TEST XCASH in it

```bash
curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_balance"}' -H 'Content-Type: application/json'
```

Register the wallet into the DPoPS system:

* Replace `DELEGATE_NAME` with the delegate name of your choice.
* Replace `DELEGATE_IP_ADDRESS` with the servers public IP address.

```bash
curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"delegate_register","params":{"delegate_name":"DELEGATE_NAME","delegate_IP_address":"DELEGATE_IP_ADDRESS"}}' -H 'Content-Type: application/json'
```

### Debug Code on a Server

Having the ability to debug the code while it is running on the server will be essential. In this part, we will explain how to use [Visual Studio Code](https://code.visualstudio.com/) and [GDB](https://www.gnu.org/software/gdb/) to visually debug the program on your desktop.

{% hint style="info" %}
This works best on a Linux host machine, although there are ways to get it to work with MacOS and Windows.
{% endhint %}

{% hint style="warning" %}
**It is strongly recommended to run this on a Linux host inside of a virtual machine. This is due to the fact that** _**X11 forwarding**_ **can allow someone on the server to run X11 programs on your machine, or to collect data on your currently running programs.**
{% endhint %}

#### Install GDB and VSCode

First, install [GDB](https://www.gnu.org/software/gdb/) on your server

```bash
sudo apt install gdb
```

Then, install Visual Studio Code on the server:

Go to the Visual Studio Code [download page](https://code.visualstudio.com/Download) and download the binary for your OS.

Upload the binary to your server using `secure copy` then install vscode

```bash
scp FILE root@server:/root/Installed-Programs
cd /root/Installed-Programs
dpkg -i ./code*
```

Installation will fail, so now install the dependencies and retry installation:

```bash
sudo apt-get -f install
dpkg -i ./code*
```

Now, install **X11** dependencies

```bash
sudo apt install xauth libx11-xcb1 libasound2 x11-apps libice6 libsm6 libxaw7 libxft2 libxmu6 libxpm4 libxt6 x11-apps xbitmaps libxtst6
```

After running vscode, [install the C++/C extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).

#### Run vscode on the Server and Forward the GUI to a Desktop

Add `-XC -c aes128-gcm@openssh.com` to the front of any ssh command that you would normally use to connect to the server.

{% hint style="info" %}
On faster internet connections, you can optionally try **`-X -c aes128-gcm@openssh.com`** to run without compression.
{% endhint %}

Once connected type `code` to start **vscode**. If root you might have to run `code --user-data-dir PATH_TO_FOLDER` to get it to run. Make sure it's an empty folder only used for vscode.

{% hint style="info" %}
It might take 10-30 seconds for the **vscode** window to appear on your desktop.
{% endhint %}

You can now use **vscode** on your desktop but it will have the file system of the server. It will be a little laggy though, so its probably not good for writing new functions, but for quick file edits \(since you have the files panel\) and for debugging it should work fine.

#### Change vscode Keyboard Shortcuts

Optionally change the vscode keyboard shortcuts to the following

Open the keyboard shortcuts  
`File => Preferences => Keyboard Shortcuts`

At the search bar at the top type `saveall`. For the `File: Save All` edit it to `Control+S` and ignore the warning about there already being that keyboard shortcut

At the search bar at the top type `start debugging`. For the `Debug: Start Debugging` edit it to `Control+Shift+D` and ignore the warning about there already being that keyboard shortcut

This will now allow you to save all files with `Control+S`

To rebuild, and start the debugger use `Control+Shift+D`

If the keyboard shortcuts are not changed, then to rebuild and start the debugger it is `F5` and `Control+Shift+D` to switch to the debugger panel

The **vscode** files have already been written for you and are included in the `.vscode` directory

## **II. Alpha Tests**

### Step 1 - Register Delegate

#### Objectives

The aim of the first test is to make sure that the seed nodes accept registration for the first few users. As more users will register, they should get a `failed to regsiter` status since the `XCASH_DPOPS`process will not be running. With more users registering, it will slowly decrease the network data nodes to less than 67% of the nodes.

#### Components

* XCASH DPOPS Wallet commands: [`delegate_register`](configure-delegate.md#register-a-delegate), [`delegate_update`](configure-delegate.md#update-a-delegates-information), [`delegate_remove`](configure-delegate.md#remove-a-delegate) 
* XCASH\_DPOPS - [delegate\_server\_functions.c](https://github.com/X-CASH-official/XCASH_DPOPS/blob/master/functions/server_functions/delegate_server_functions/delegate_server_functions.c)

**Components Settings**

| **Service** | Status |
| :--- | :--- |
| **XCASH\_DPOPS** | OFF |
| **X-CASH Daemon** | ON |
| **X-CASH Wallet** | OFF |
| **MongoDB** | ON/OFF |

**Participants**

The test can be validated by a few numbers of users, but everyone is welcome to try.

**How to participate**

Once the network data nodes are turned on, open your wallet in the CLI and issue the [`register delegate`](configure-delegate.md#register-a-delegate) command:

```bash
delegate_register <test1_delegate_name> <ip_or_domain_name>
```

where:

* `delegate_name`: String; Name of your delegate
* `ip_or_domain_name`: String; Delegate's or IP domain name \(without `www` or `http`\)

Once the delegate is registered, try to [update a piece of data](configure-delegate.md#update-a-delegates-information):

```bash
delegate_update about <message>
```

When successful, you can try to [remove the delegate](configure-delegate.md#remove-a-delegate) from the database:

```text
delegate_remove
```

### Step 2 - DB Synchronization

#### Objectives

* Make sure that the seed nodes database is synced
* Showing all databases synced
* Delegate nodes synced with the same database as seed nodes

#### Components

* XCASH\_DPOPS - [block\_verifiers\_synchronize\_server\_functions.c](https://github.com/X-CASH-official/XCASH_DPOPS/blob/master/functions/block_verifiers_functions/block_verifiers_thread_server_functions/block_verifiers_thread_server_functions.c)
* XCASH\_DPOPS - [block\_verifiers\_synchronize\_functions.c](https://github.com/X-CASH-official/XCASH_DPOPS/blob/master/functions/block_verifiers_functions/block_verifiers_synchronize_functions/block_verifiers_synchronize_functions.c)

**Components Settings**

| **Service** | Status |
| :--- | :--- |
| **XCASH\_DPOPS** | ON |
| **X-CASH Daemon** | ON |
| **X-CASH Wallet** | ON |
| **MongoDB** | ON |

**Participants**

Every delegates test these functions.

**How to participate**

Start the XCASH\_DPOPS and view the logs in follow mode, wait for it to sync all databases successfully and then create the server.

```bash
journalctl --unit=XCASH_DPOPS --follow -n 100 --output cat
```

Once done, exit the logs and open mongoDB to view if your database has been synced

```bash
mongo
show dbs
use XCASH_PROOF_OF_STAKE
show collections
db.delegates.find()
```

You should see the 5 seed nodes and the other users who have registered, and compare them with the results of the delegates website.

### Step 3 - DB Synchronization and registering

#### Objectives

* Verify that that at least 67% of the delegates have the up to date database.

#### Components

* XCASH DPOPS Wallet commands: [`delegate_register`](configure-delegate.md#register-a-delegate), [`delegate_update`](configure-delegate.md#update-a-delegates-information), [`delegate_remove`](configure-delegate.md#remove-a-delegate) 
* XCASH\_DPOPS - [delegate\_server\_functions.c](https://github.com/X-CASH-official/XCASH_DPOPS/blob/master/functions/server_functions/delegate_server_functions/delegate_server_functions.c)
* XCASH\_DPOPS - [block\_verifiers\_synchronize\_server\_functions.c](https://github.com/X-CASH-official/XCASH_DPOPS/blob/master/functions/block_verifiers_functions/block_verifiers_thread_server_functions/block_verifiers_thread_server_functions.c)
* XCASH\_DPOPS - [block\_verifiers\_synchronize\_functions.c](https://github.com/X-CASH-official/XCASH_DPOPS/blob/master/functions/block_verifiers_functions/block_verifiers_synchronize_functions/block_verifiers_synchronize_functions.c)

**Components Settings**

| **Service** | Status |
| :--- | :--- |
| **XCASH\_DPOPS** | ON |
| **X-CASH Daemon** | ON |
| **X-CASH Wallet** | ON |
| **MongoDB** | ON |

**Participants**

Every delegates test these functions.

**How to participate**

If you have successfully participated in [step 2](set-up-the-test-alpha-phase.md#step-2-db-synchronization), make sure that you delete the databse to start fresh:

```bash
mongo
use XCASH_PROOF_OF_STAKE
db.dropDatabase()
```

Start the XCASH\_DPOPS and view the logs in follow mode, wait for it to sync all the databases successfully and then creating the server.

```bash
journalctl --unit=XCASH_DPOPS --follow -n 100 --output cat
```

Register your delegate:

```bash
curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"delegate_register","params":{"delegate_name":"DELEGATE_NAME","delegate_IP_address":"DELEGATE_IP_ADDRESS"}}' -H 'Content-Type: application/json'show dbs
```

where:

* `DELEGATE_NAME`: String; Name of your delegate
* `DELEGATE_IP_ADDRESS`: String; Delegate's or IP domain name \(without `www` or `http`\)

Watch to see if your DB is updated as new users register, and compare your DB results with the delegates website.

