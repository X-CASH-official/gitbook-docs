---
description: Set of instructions to participate in the phase 2 of the DPOPS beta.
---

# DPOPS beta  V2

#### **Thank you for participating in the DPOPS beta! 🙏**

> We would also like to give a huge thanks to all the contributors who assisted us since the alpha to identify these bugs and also providing their feedback to improve DPOPS. We are able to move forward with the phase 2 of the beta with the help of all beta participant and engaged community members.

The following guide is split into two sections: the first part for becoming a delegate during the beta, and the second part dedicated to voting during the beta.

{% hint style="info" %}
The phase 2 will start on **Saturday 29th August, at 6 pm Paris time**.   
Until this time, you won't be able to perform any other operation on the blockchain apart from voting and updating delegate information.  
{% endhint %}

## Beta delegate

### 1. Prerequisites

{% hint style="info" %}
If you weren't part of phase 1 of the beta, you will first have to rent a server and install the node validation program. We recommend that you go through the [whole documentation to install the node program](get-started.md). 
{% endhint %}

If you have joined the beta during the first phase, you most certainly have your setup already prepared. You will just need to update your block verifier keys before moving forward. 

To do that without re-installing all your delegate setup, first run this command to update your `xcash-dpops` binaries:

```text
cd ~/xcash-official/xcash-dpops && git reset --hard 22827775be78e006781ba2ca5cc917c62117103a && git pull && make clean ; make release -j $(nproc) && sudo systemctl stop xcash-dpops
```

Then, to update your keys in your different service, copy and paste the following command and run it in your terminal:

```text
cd ~ && BLOCK_VERIFIERS_SECRET_KEY_LENGTH=128 && BLOCK_VERIFIERS_PUBLIC_KEY_LENGTH=64 && OLD_BLOCK_VERIFIER_SECRET_KEY=$(cat /lib/systemd/system/xcash-dpops.service | grep 'block-verifiers-secret-key' | awk {'print $3'}) && OLD_BLOCK_VERIFIER_PUBLIC_KEY="${OLD_BLOCK_VERIFIER_SECRET_KEY: -${BLOCK_VERIFIERS_PUBLIC_KEY_LENGTH}}" && DATA=$(~/xcash-official/xcash-dpops/build/xcash-dpops --generate-key 2>&1 >/dev/null) && BLOCK_VERIFIER_SECRET_KEY="${DATA: -132}" && BLOCK_VERIFIER_SECRET_KEY="${BLOCK_VERIFIER_SECRET_KEY:0:128}" && BLOCK_VERIFIER_PUBLIC_KEY="${BLOCK_VERIFIER_SECRET_KEY: -${BLOCK_VERIFIERS_PUBLIC_KEY_LENGTH}}" && sudo sed -i "s/$OLD_BLOCK_VERIFIER_SECRET_KEY/$BLOCK_VERIFIER_SECRET_KEY/g" /lib/systemd/system/xcash-dpops.service && sudo sed -i "s/$OLD_BLOCK_VERIFIER_SECRET_KEY/$BLOCK_VERIFIER_SECRET_KEY/g" /lib/systemd/system/xcash-daemon.service && echo -e "\n\nOld VRF public key: $OLD_BLOCK_VERIFIER_PUBLIC_KEY\nOld VRF secret key: $OLD_BLOCK_VERIFIER_SECRET_KEY\n\n\n\nVRF public key: $BLOCK_VERIFIER_PUBLIC_KEY\nVRF secret key: $BLOCK_VERIFIER_SECRET_KEY\n\n" && BLOCK_VERIFIERS_SECRET_KEY_LENGTH="" && BLOCK_VERIFIERS_PUBLIC_KEY_LENGTH="" && OLD_BLOCK_VERIFIER_SECRET_KEY="" && OLD_BLOCK_VERIFIER_PUBLIC_KEY="" && BLOCK_VERIFIER_SECRET_KEY="" && BLOCK_VERIFIER_PUBLIC_KEY=""
```

{% hint style="info" %}
This will generate a new pair of block verifier keys, as well as update the **`xcash-dpops.service`** file with the corresponding information.   
Be sure to securely save your block verifier keys.
{% endhint %}

Now, you will have to tell your program to start automatically at the right time on August 29th.

Edit the systemd unit file **`xcash-dpops.service`** from the **`/lib/systemd/system/`** folder :

```bash
nano /lib/systemd/system/xcash-dpops.service
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

At the `ExecStart` line, add the following option at the end:

```text
--start-time 7 31 15 56
```

{% hint style="warning" %}
The **`--block-verifiers-secret-key`** should always be the first parameter.
{% endhint %}

{% hint style="info" %}
This will tell the `xcash-dpops` program to start 5 minutes before 6 PM Paris time on August 29th.
{% endhint %}

Once the changes added, save and close the `xcash-dpops.service` file and update `systemctl` to take the changes into account: 

```bash
systemctl daemon-reload
```

### 2. Reset the blockchain and database

You will need to reset the current blockchain and data you have gathered in your local database during the first phase of the beta. First, copy/paste the following command to launch the auto-installer script and stop the running programs:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)" && systemctl stop xcash-dpops xcash-rpc-wallet
```

Choose `option 10` to automatically reset the local database and bring back your local blockchain to block 640,000. 

Finally, run the auto-installer's `option 7 - Restart Programs` to restart the different services.

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

You can now [register yourself as a delegate](dpops-beta.md#3-register-as-a-delegate).

{% hint style="info" %}
In case you don't have the blockchain downloaded or you are joining the beta for the first time, we have prepared a bootstrap version of the blockchain snapshot. To download it, you will need to run the auto-installer script:

```bash
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

And chose `option 4: Update/Install the blockchain`
{% endhint %}

### 3. Register as a delegate

Now that your installation of the program is ready, you can [register yourself as a delegate](register-delegate.md). Your information will be added to the [delegates' explorer](http://delegates.xcash.foundation/auth/tables/delegates) in the next minutes, and you will be ready to accept votes.

{% hint style="info" %}
Do not forget to update your [public information](register-delegate.md#3-update-public-information).
{% endhint %}

To make sure that you are correctly registered and every step has been done accordingly, check the live logging of the `xcash-dpops` program using the command: 

```bash
journalctl --unit=xcash-dpops -n 100 --follow --output cat
```

The system should display `Waiting for the specific start time` if you have followed every step correctly. 

{% hint style="info" %}
If you find yourself having difficulties, don't hesitate to join the[ Discord channel for delegates](https://discord.gg/D2PV2pb).
{% endhint %}

## Beta voting

### 1. Download Binaries

To vote with your XCASH, you will need to download the [beta version of the 2.0.0 binaries on GitHub](https://github.com/X-CASH-official/xcash-core/releases/tag/2.0.0-beta). Select the link relating to your Operating System and download the GitHub release. Unpack them in a folder \(preferably close to the **`root`** or **`C:/`** folder\).

### **2. Synchronizing your wallet**

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

### 3. Vote

Once your wallet as finished synchronizing completely, you will be able to vote for your desired delegate.

{% hint style="danger" %}
Initiating a **`transfer`** or a **`sweep_all`** function before the start of the beta on August 28th will not succeed and prevent you from voting.   
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

