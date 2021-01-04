---
description: Instructions for stakers on how to apply your vote.
---

# Vote & Staking

## Find Your Delegate

All delegates are listed on the [delegate explorer](http://delegates.xcash.foundation/). Take time to get to learn more about them, check their reliability statistics, and their setup.   
You can/should only vote toward shared delegates, as solo delegates won't redistribute rewards to voters.

{% hint style="danger" %}
The [delegate explorer](http://delegates.xcash.foundation/delegates)'s information is filled by the delegates. There are currently no checks to the veracity of the information provided. It is your duty to make sure that a delegate is trustworthy, that he will redistribute the reward and that the fees displayed are correct.
{% endhint %}

{% hint style="info" %}
Most if not all delegates are discussing freely on X-Cash offficial Discord server. We recommend that you [join the discussion](https://discord.gg/4CAahnd) there and engage discussion with the node manager to help you make a choice.
{% endhint %}

## Vote

{% hint style="info" %}
There is a couple of rules to observe when voting:

* **You can only have one vote assigned per wallet.** If you want to vote for another delegate, you will need to create a new wallet, and send to it XCASH you plan to vote with.
* **Votes are taken into account at the top of the next hour.** If you apply a new vote at `XX:30`, it will be in effect at `XX+1:00`.
* **You need a minimum of `2,000,000 (2 Million) XCASH` in the wallet to vote.**
* **Spending any amount in your wallet will cancel the vote.** It is recommended to stake from a wallet you are not actively using.
{% endhint %}

To participate in the network, you will have to vote with your XCASH to elect a delegate that you see as trustworthy and you want to help to get a forging position. The DPOPS consensus has been designed so that the **XCASH you use to vote stays in your wallet, hence in your control** \(see [the challenge of voting in a privacy coin](https://docs.xcash.foundation/dpops/yellowpaper-delagated-proof-of-private-stake#the-challenges-of-staking-and-voting-in-a-privacy-coin)\).

### Desktop Wallet

#### Download the wallet program

Download the 2.0.0 version of the X-Cash CLI wallet, on our [official website](https://www.xcash.foundation/wallet) or on the [GitHub Releases](https://github.com/X-CASH-official/xcash-core/releases).

{% hint style="info" %}
Before running the wallet binaries, it is recommended to allow the wallet folder in your firewall. Windows mistakenly picks up the executable as dangerous.
{% endhint %}

#### Synchronizing a wallet

You will need to synchronize your wallet to a node, or download your own blockchain locally. To synchronize, you have two options:  
1. Running the daemon and synchronizing the full blockchain \(slow but most secure\)  
2. Using a remote node to synchronize \(quick\)

#### 1. Local synchronization

To synchronize the blockchain locally, run the daemon `xcashd` executable with administrator rights. This will download and synchronize the blockchain to your computer \(blockchain location by default : `C:\ProgramData\X-CASH`\).

![](../.gitbook/assets/image%20%2828%29.png)

The daemon will connect to an X-Cash node and download the blockchain to your computer.

{% hint style="info" %}
This could take several hours depending on your connection. 
{% endhint %}

Once the blockchain is completely synchronized, leave the xcash daemon opened and go to the next step and restore your wallet.

#### 2. Remote node 

To synchronize the wallet using a remote node, you can connect to a trusted node to synchronize your wallet without downloading the blockchain locally

`us1.xcash.foundation:18281  
europe1.xcash.foundation:18281  
europe2.xcash.foundation:18281  
europe3.xcash.foundation:18281  
oceania1.xcash.foundation:18281`

#### Restore a wallet



#### **Prepare your vote**

To vote, you will need to have at least 2,000,000 \(2 Millions\) XCASH unlocked in your wallet. As you can cast only **one vote from one given wallet address,** you will need to create and top up another wallet if you want to cast another vote.

When you cast your vote, your wallet will create a reserve proof, which is a cryptographic proof of the current amount stored in your wallet. If you spend any amount from your wallet, the reserve proof will be broken and your vote will become invalid. 

Before casting your vote, you will need to concatenate the **unspents** of your wallet, otherwise your vote could be invalid. To do that, you just need to use the function `sweep_all` to your wallet address.

```text
sweep_all <your_public_wallet_address>
```

This will take all your unspents, concatenate them into one unspent and send it back to you.

{% hint style="info" %}
The amount of your wallet will be 0 after a `sweep_all` until the transaction is complete. Do not worry it will come back at once!
{% endhint %}

Once your wallet is prepared with the amount you wish to vote with, you can cast your vote.

#### Cast a vote

To vote, it's quite easy. Use the **`vote`** command:

```text
vote <delegates_public_address|delegates_name>
```

You can either put the **`<delegates_public_address>`** which is a standard XCASH public address, or the **`<delegate_name>`**. This information is available on the [delegate explorer](http://delegates.xcash.foundation/).

The wallet will create a **reserve proof** with the entirety of the wallet and assign it to the designated delegate. Once your vote has been casted, you will have to **wait with your wallet running** until the top of hour before getting a success message.

You will get a success message when your vote ****has been taken into account.

### Android Wallet

