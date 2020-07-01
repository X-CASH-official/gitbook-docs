---
description: Set of instructions to participate in the beta.
---

# DPOPS beta 🚧

#### **Thank you for participating in the DPOPS beta! 🙏**

In case you missed the announcement, please refer to[ this article](https://medium.com/x-cash/delegated-proof-of-private-stake-beta-general-information-and-schedule-5d7e513b11).  
****The following guide is split into two sections: the first part for becoming a delegate during the beta, and the second part dedicated to voting during the beta.

{% hint style="info" %}
The forging of blocks will start on the **28th of July at 6PM Paris time**.   
Until this time, you won't be able to perform any other operation on the blockchain apart from voting and updating delegate information.  
{% endhint %}

## Beta delegate

### Prerequisites

If you want to become a delegate, you will have first to rent a server and install the node validation program. We recommend that you go through the whole documentation to install the node program. 

Once you have finished[ installing the node program](node-installation.md), you will have to tell your program to start at the right time. 

Edit the systemd unit file **`xcash-dpops.service`** from the **`/lib/systemd/system/`** folder :

```bash
nano nano /lib/systemd/system/xcash-dpops.service
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

At the `ExecStart` line, add the following option:

```text
--start-time 28 15 56
```

{% hint style="info" %}
This will tell the `xcash-dpops` program to start 5 minutes before 6PM Paris time on July 28th.
{% endhint %}

### Register as a delegate

Now that your installation of the program is ready, you can [register yourself as a delegate](register-delegate.md). Your information will be added to the [delegates' explorer](http://delegates.xcash.foundation/auth/tables/delegates) in the next minutes, and you will be ready to accept votes!

{% hint style="info" %}
Do not forget to update your [public information](register-delegate.md#3-update-public-information).
{% endhint %}

## Beta voting

### Download Binaries

To vote with your XCASH, you will need to download the beta version of the 2.0.0 binaries on GitHub. Select the link relating to your Operating System and download the GitHub release. Unpack them in a folder \(preferably close to the `root` or `C:/` folder\).

### **Synchronizing your wallet**

You will need to synchronize the wallet on the alternative chain \(the "beta-chain"\), either by downloading the alternative chain completely or by synchronizing through one of the seed nodes.   
We **highly recommend** synchronizing from a seed node, as downloading the whole blockchain will take time, data, and will be useless at the end of the beta phase because it will be discarded.

You will need to restore the wallet you will use to vote with and synchronize it with a seed node.

#### On Windows

Open a terminal window \(cmd or PowerShell\) as an administrator and change directory to your wallet folder. 

{% code title="Example" %}
```text
cd C:/xcash-cli-windows-2.0.0-beta/
```
{% endcode %}

Then, run the xcash-wallet-cli.exe with the following option:

```text
./xcash-wallet-cli.exe --restore-deterministic-wallet --daemon-address <daemon_address>
```

And replace **`<daemon_address>`** with either **`europe1.xcash.foundation`** or **`us2.xcash.foundation`** depending on your location. You will be then prompted to restore your wallet. Insert the seed of your wallet and let it synchronize to the current height.

**On Unix systems**

Open a terminal window and run the `xcash-wallet-cli` file with the following options:

```text
/path_to_folder/xcash-wallet-cli --restore-deterministic-wallet --daemon-address <daemon_address>
```

And replace **`<daemon_address>`** with either **`europe1.xcash.foundation`** or **`us2.xcash.foundation`** depending on your location. You will be then prompted to restore your wallet. Insert the seed of your wallet and let it synchronize to the current height.

### Vote

Once your wallet as finished synchronizing completely, you will be able to vote for your desired delegate.

{% hint style="danger" %}
Initiating a `transfer` or a `sweep_all` function before the start of the beta on July 28th will not succeed and prevent you from voting.   
You will have to restore your wallet again in that case. 
{% endhint %}

Use the **`vote`** command:

```text
vote <delegates_public_address|delegates_name>
```

You can either put the **`<delegates_public_address>`** which is a standard XCASH public address, or the **`<delegate_name>`**. This information is available on the [delegate explorer](http://delegates.xcash.foundation/).

The wallet will create a **reserve proof** with the entirety of the wallet and assign it to the designated delegate. You will get a success message when your vote is taken into account.

{% hint style="danger" %}
There is a couple of rules to observe when voting:

* **You can only have one vote assigned per wallet.** If you want to vote for another delegate, you will need to create a new wallet, and send to it XCASH you plan to stake.
* **Votes are taken into account at the top of the next hour.** If you apply a new vote at `XX:30`, it will be in effect at `XX+1:00`.
* **You need a minimum of `2,000,000 (2 Million) XCASH` in the wallet to vote.**
* **Spending any amount in your wallet will cancel the vote.** It is recommended to stake from a wallet you are not actively using.
{% endhint %}

