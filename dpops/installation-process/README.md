---
description: >-
  A step-by-step guide to install and set up the DPoPS program on your XCASH
  node and become a delegate.
---

# Delegate Node Installation

## System Requirements

{% hint style="info" %}
In the first beta version, X-Cash's DPoPS will only run on a Linux/Unix OS. **We recommend installing it on a Ubuntu VPS/dedicated server \(18.04\) for best compatibility.**
{% endhint %}

The delegate node will need to transit a lot of information, notably messages to the other delegates to verify the block informations, As time goes by, the features and information that the delegates handle will increase \(notably when we will develop **token creation**, **sidechains** and other exciting features ⭐\). 

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

{% hint style="warning" %}
 It is estimated that the blockchain size will increase by **18GB per year.**
{% endhint %}

## Dependencies

The following table summarizes the tools and libraries required to run X-Cash's DPoPS program.

| Dependencies | Min. version | Ubuntu package |
| :--- | :--- | :--- |
| **GCC** | 4.7.3 | `build-essential` |
| **CMake** | 3.0.0 | `cmake` |
| **pkg**-**config** | any | `pkg-config` |
| **OpenSSL** | any | `libssl-dev` |
| **Git** | any | `git` |
| **MongoDB** | 4.0.3 | Install from [binaries](https://www.mongodb.com/download-center/community) |
| **MongoDB C Driver** \(includes BSON libary\) | 1.13.1 | Build from source |
| **XCASH** | Latest version | [download the latest release](https://github.com/X-CASH-official/X-CASH/releases) or [build from source](https://github.com/X-CASH-official/X-CASH#compiling-x-cash-from-source) |

## Auto-installer Script



## Manual Installation Process

### Installation path

{% hint style="info" %}
It is recommended to install the XCASH\_DPOPS folder, MongoDB and MongoDB C Driver in the home directory \(`/home/$USER/`\) or root directory \(`/root/`\) in a `Installed-Programs` folder.
{% endhint %}

Create the `Installed-Programs`folder and the `xcash_wallets`folder

```bash
mkdir /root/Installed-Programs
mkdir /root/Installed-Programs/xcash_wallets
```

### Install dependencies

#### Install System Packages

Make sure the systems packages list is up to date and install the necessary packages.

```bash
sudo apt update
sudo apt install build-essential cmake pkg-config libssl-dev git
```

{% hint style="info" %}
_\(Optional\)_ Install the packages for XCASH if you plan to [build XCASH from source](https://github.com/X-CASH-official/X-CASH#compiling-x-cash-from-source)
{% endhint %}

#### Installing MongoDB from Binaries

Visit [https://www.mongodb.com/download-center/community](https://www.mongodb.com/download-center/community)

Then choose your OS, and make sure the version is the current version and the package is server. Then click on All version binaries. Now find the current version to download. You do not want the debug symbols or the rc version, just the regular current version.

Once you have downloaded the file move the file to a location where you want to keep the binaries, then run this set of commands

```bash
tar -xf mongodb-linux-x86_64-*.tgz
rm mongodb-linux-x86_64-*.tgz
sudo mkdir -p /data/db
sudo chmod 770 /data/db
sudo chown $USER /data/db
```

Then add MongoDB to your path

```bash
echo -e '\export PATH=MongoDB_folder:$PATH' >> ~/.profile && source ~/.profile
```

{% hint style="info" %}
If you want to install MongoDB on a different hard drive, make sure to change the path of the `/data/db`
{% endhint %}

#### Building the MongoDB C Driver From Source

Visit the offical websites installation instructions at [http://mongoc.org/libmongoc/current/installing.html](http://mongoc.org/libmongoc/current/installing.html) Follow the instructions for [Building from a release tarball](http://mongoc.org/libmongoc/current/installing.html#building-from-a-release-tarball) or [Building from git](http://mongoc.org/libmongoc/current/installing.html#building-from-git) since you need the header files, not just the library files.

After you have built the MongoDB C driver from source, you will need to run

```bash
sudo ldconfig
```

### Build Instructions

Now that the dependencies are all installed, you can clone the repository in the `Installed-Programs` folder.

```bash
cd ~Installed-Programs 
git clone https://github.com/X-CASH-official/XCASH_DPOPS.git
```

X-CASH Proof of stake uses a Make file to build. After cloning the repository, navigate to the folder then use the make file to build the binary file.

```bash
cd ~/Installed-Programs/XCASH_DPOPS
make clean ; make release -j `nproc`
```

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

