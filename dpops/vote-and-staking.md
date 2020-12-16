---
description: Instructions for stakers on how to apply your vote.
---

# Vote & Staking

## Find Your Delegate

All delegates are listed on the [delegate explorer](http://delegates.xcash.foundation/). Take time to get to learn more about them, check their reliability statistics, and their setup.   
You can/should only vote toward shared delegates, as solo delegates won't redistribute rewards to voters.

{% hint style="danger" %}
The [delegate explorer](http://delegates.xcash.foundation/)'s information is filled by the delegates. There are currently no checks to the veracity of the information provided. It is your duty to make sure that a delegate is trustworthy, that he will redistribute the reward and that the fees displayed are correct.
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

### Download the wallet

Download the 2.0.0 version of the X-Cash CLI wallet, on our [official website](https://www.xcash.foundation/wallet) or on the [GitHub Releases](https://github.com/X-CASH-official/xcash-core/releases).

_T**h**e download links will be added when the registration period starts_

### **Run the CLI wallet**

\*\*\*\*

### **Prepare your vote**

### Cast a vote

To vote, it's quite easy. Use the **`vote`** command:

```text
vote <delegates_public_address|delegates_name>
```

You can either put the **`<delegates_public_address>`** which is a standard XCASH public address, or the **`<delegate_name>`**. This information is available on the [delegate explorer](http://delegates.xcash.foundation/).

The wallet will create a **reserve proof** with the entirety of the wallet and assign it to the designated delegate. You will get a success message when your vote is taken into account.

{% hint style="info" %}
There is a couple of rules to observe when voting:

* **You can only have one vote assigned per wallet.** If you want to vote for another delegate, you will need to create a new wallet, and send to it XCASH you plan to stake.
* **Votes are taken into account at the top of the next hour.** If you apply a new vote at `XX:30`, it will be in effect at `XX+1:00`.
* **You need a minimum of `2,000,000 (2 Million) XCASH` in the wallet to vote.**
* **Spending any amount in your wallet will cancel the vote.** It is recommended to stake from a wallet you are not actively using.
{% endhint %}

