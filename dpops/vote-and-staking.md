---
description: >-
  As an X-Cash holder, you can stake your delegate and vote for a preferred
  delegate. Follow the guide below to learn more about voting & staking.
---

# Vote & Staking

{% hint style="warning" %}
During the registration period, you will only be able to vote for a delegate using the CLI wallet or the Android phone wallet. More practical ways of voting will be developed along the way.  
  
This also means that you won't be able to vote from the X-Bank or the 1.5.0 of the CLI and GUI wallet. Instructions to vote using the available methods are described below.
{% endhint %}

## Find Your Delegate

All delegates are listed on the [delegate explorer](http://delegates.xcash.foundation/delegates). Take time to get to learn more about them, check their reliability statistics, and their setup.   
You will find 3 types of delegates:

* **Shared Delegates**: The shared delegates are public delegates for whom you can vote. They will redistribute your share of the rewards minus their fees. As a voter, you should favor shared delegates.
* **Solo Delegates**: Solo delegates are elected by themselves and are keeping the reward for them. You can vote for solo delegates, but don't expect to receive a share of the block reward.
* **Private group**: Private delegates group are delegates that will only redistribute shares of the block reward to a select group of their voters. They are usually decided with the delegate in private, so you shouldn't vote for them if you haven't communicated with the delegate first.

{% hint style="danger" %}
The [delegate explorer](http://delegates.xcash.foundation/delegates)'s information is filled by the delegates. There are currently no checks to the veracity of the information provided. It is your duty to make sure that a delegate is trustworthy, that he will redistribute the reward and that the fees displayed are correct.
{% endhint %}

{% hint style="info" %}
Most if not all delegates are discussing freely on X-Cash official Discord server. We recommend that you [join the discussion](https://discord.gg/4CAahnd) there and engage discussion with the node manager to help you make a choice.
{% endhint %}

## Staking Calculation

You can calculate your expected ROI depending on several factor to help you with your vote.

Community member and delegate **UrsaMajor** is providing a comprehensive list of delegates to help you understand and calculate what reward you can expect by voting with your XCASH. 

Download locally this [excel sheet](https://onedrive.live.com/view.aspx?resid=166AE7FDFC13C5FD!10645&ithint=file%2Cxlsx&authkey=!AA6KXoOqlYWqRJA) \(updated every few days, but you can manually update it yourself\), and input your desired staking amount to see what reward you can expect depending on the delegate you are voting for, their current stake and fees.

## Vote & Staking

{% hint style="info" %}
There is a couple of rules to observe when voting:

* **You can only have one vote assigned per wallet.** If you want to vote for another delegate, you will need to create a new wallet, and send to it XCASH you plan to vote with.
* **Votes are taken into account at the top of the next hour.** If you apply a new vote at `XX:30`, it will be in effect at `XX+1:00`.
* **You need a minimum of `2,000,000 (2 Million) XCASH` in the wallet to vote.**
* **Spending any amount in your wallet will cancel the vote.** It is recommended to stake from a wallet you are not actively using.
{% endhint %}

To participate in the network, you will have to vote with your XCASH to elect a delegate that you see as trustworthy and you want to help to get a forging position. The DPOPS consensus has been designed so that the **XCASH you use to vote stays in your wallet, hence in your control** \(see [the challenge of voting in a privacy coin](https://docs.xcash.foundation/dpops/yellowpaper-delagated-proof-of-private-stake#the-challenges-of-staking-and-voting-in-a-privacy-coin)\).

By voting for shared delegates, or within a private group after making an agreement with the delegate, you will receive a share of the block reward, which is a form of staking of your coins.

### Desktop Wallet

#### Download the wallet program

Download the 2.0.0 version of the X-Cash binaries, on our [official website](https://www.xcash.foundation/wallet) or on the [GitHub Releases](https://github.com/X-CASH-official/xcash-core/releases/tag/2.0.0).

{% hint style="info" %}
Before running the wallet binaries, it is recommended to allow the X-Cash binaries folder in your firewall. Windows mistakenly picks up the executable as dangerous.
{% endhint %}

#### Synchronizing a wallet

You will need to synchronize your wallet to a node, or download your own blockchain locally. To synchronize, you have two options:  
1. Running the daemon and synchronizing the full blockchain \(slow but most secure\)  
2. Using a remote node to synchronize \(quick\)

#### 1. Local synchronization

To synchronize the blockchain locally, run the daemon `xcashd` executable with administrator rights. This will download and synchronize the blockchain to your computer \(blockchain location by default : `C:\ProgramData\X-CASH`\).

![](../.gitbook/assets/image%20%2829%29%20%283%29.png)

The daemon will connect to an X-Cash node and download the blockchain to your computer.

{% hint style="info" %}
This could take several hours depending on your connection. 
{% endhint %}

Once the blockchain is completely synchronized, leave the X-Cash daemon opened and go to the next step and restore your wallet.

#### 2. Remote node synchronization

To synchronize the wallet using a remote node, you can connect to a trusted node to synchronize your wallet without downloading the blockchain locally. To do that, you will need to open the wallet using the command line interface and adding the parameter `--daemon-address`. 

First, launch a terminal \(mac/linux\) or a command prompt with administrator rights \(windows\) in the folder where you have downloaded the wallet binaries. To do so, find the path to your X-Cash binaries and change your directory to this location.

```text
cd C:/your-path-to-xcash-binaries
```

Once your terminal is windows is located in the wallet binaries, you can run the CLI wallet with the following parameter:

```text
./xcash-wallet-cli.exe --daemon-address <daemon_address:port>
```

{% hint style="info" %}
During the registration period, you can chose from one of the following trusted daemon address:

`us1.xcash.foundation:18281  
europe1.xcash.foundation:18281  
europe2.xcash.foundation:18281  
europe3.xcash.foundation:18281  
oceania1.xcash.foundation:18281`
{% endhint %}

_Example:_ 

```text
./xcash-wallet-cli.exe --daemon-address us1.xcash.foundation:18281
```

You will be able to open a wallet file if you have one and let it synchronize the last block. If you don't have a wallet file, or wish to restore a wallet, follow the instructions below. If your wallet is synchronized, you can go to the **prepare your vote** section.

#### Restore your wallet

{% hint style="info" %}
Wallet files made using the binaries 1.5.0 and the GUI wallet can be used with the 2.0.0 binaries.
{% endhint %}

If you don't have the wallet files or want to restore a wallet, you can use the `--restore-deterministic-wallet` parameter and provide your mnemonic seed.

First, launch a terminal \(mac/linux\) or a command prompt with administrator rights \(windows\) in the folder where you have downloaded the wallet binaries. To do so, find the path to your X-Cash binaries and change your directory to this location.

```text
cd C:/your-path-to-xcash-binaries
```

Once your terminal is windows is located in the wallet binaries, you can run the CLI wallet with the following parameter:

```text
./xcash-wallet-cli.exe --restore-deterministic-wallet --daemon-address us1.xcash.foundation:18281
```

You will be then prompted to give your wallet mnemonic seed and a new password. Once entered, your wallet will synchronize blocks with the remote node. It could take a while as a newly created wallet needs to scan all the blocks until the last one. 

Once done, your wallet is ready and you can start to prepare your vote.

#### **Prepare your vote**

To vote, you will need to have at least 2,000,000 \(2 Millions\) XCASH unlocked in your wallet. As you can cast only **one vote from one given wallet address,** you will need to create and top up another wallet if you want to cast another vote.

When you cast your vote, your wallet will create a **reserve proof**, which is a cryptographic proof of the current amount stored in your wallet. If you spend any amount from your wallet, the reserve proof will be broken and your vote will become invalid. 

Before casting your vote, you will need to concatenate the **unspents** of your wallet, otherwise your vote could be invalid. To do that, you just need to use the function `sweep_all` to your wallet address.

{% hint style="danger" %}
Make sure to correctly input your wallet address \(the one starting with XCA...\) after the sweep\_all function. Putting an invalid XCASH address or any other address that isn't your own **will result in the permanent loss of all your wallet funds.**
{% endhint %}

```text
sweep_all <your_public_wallet_address>
```

This will take all your unspents, concatenate them into one unspent and send it back to you.

{% hint style="info" %}
The amount of your wallet will be 0 after a `sweep_all` until the transaction is complete. Your funds will come back on the next blocks, and you will have to wait about 10 blocks for your funds to become unlocked again.
{% endhint %}

Once your wallet is prepared with the amount you wish to vote with, you can cast your vote.

#### Cast a vote

To vote, it's quite easy. Use the **`vote`** command:

```text
vote <delegates_public_address|delegates_name>
```

You can either put the **`<delegates_public_address>`** which is a standard XCASH public address, or the **`<delegate_name>`**. This information is available on the [delegate explorer](http://delegates.xcash.foundation/).

The wallet will create a **reserve proof** with the entirety of the wallet and assign it to the designated delegate. Once your vote has been casted, you will have to **wait with your wallet running** until the **top of hour** before getting a success message.

You will get a success message when your vote ****has been taken into account.

### Android Wallet

{% hint style="info" %}
_Voting with Android wallet instructions made by Aquila-Audax 🙏_
{% endhint %}

{% hint style="danger" %}
Please be aware that the Android wallet is still in Beta so, be careful using it and save always in a secure place a backup of your public and private view/spend keys and/or the mnemonic seed. If this info is lost, there will be no way to restore it if necessary!
{% endhint %}

Download the wallet on your compatible android phone using the following link : [https://github.com/X-CASH-official/android-wallet/releases/tag/v1.1.0-beta](https://github.com/X-CASH-official/android-wallet/releases/tag/v1.1.0-beta)

In case you have already generated a wallet, voting from the Android X-Cash Wallet APP is a very simple process.

![](../.gitbook/assets/image%20%2831%29.png)

Tap to resume and type your wallet password:

![](../.gitbook/assets/image%20%2825%29.png)

Wait for your wallet to synchronize with the network.

![](../.gitbook/assets/image%20%2833%29.png)

Once synced, tap the DPOPS button in the upper right corner.

![](../.gitbook/assets/image%20%2826%29.png)

Scroll down the list to find your desired delegate and tap it’s name to select it, or type it’s name in the “Delegate Name” textbox near the “Vote” button.

![](../.gitbook/assets/image%20%2828%29.png)

With the desired delegate’s name in the text box, tap the Vote button.

![](../.gitbook/assets/image%20%2832%29.png)

In the next window, you will have two choices: 

a. If you are in the 3rd minute of the hour, you can tap Quick Vote and the vote will be placed right away 

b. If you are out of the 3rd minute, you can tap Auto Waiting and the app will place the vote after the needed waiting time \(you need to maintain the wallet opened till you will receive the confirmation\). Please note that in my version, it says that 35 mins. waiting time are needed to place the order \(which means that the app will try to place the vote on the 2nd minute of the hour and will throw an error showing “invalid vote time”. A minor change will be done on the next version to reflect the last change on the voting system, and apply the vote only on the 3rd minute instead of the 2nd and the 3rd minute which was the previous manner.

![](../.gitbook/assets/image%20%2835%29.png)

At the top of the next hour, your vote will be placed ! You will receive your share of the block reward on this wallet.

