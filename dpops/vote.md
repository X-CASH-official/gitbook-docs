---
description: Instructions for stakers on how to apply your vote.
---

# 🗳️ Vote & Staking

## Find Your Delegate

All delegates are listed on the [delegate explorer](http://delegates.xcash.foundation/). Take time to get to learn more about them, check their reliability statistics, and their setup. You can vote towards delegate that have the `pool_mode` is `true`.

{% hint style="info" %}
Most delegates are discussing freely on our Discord server. We recommend that you [join the discussion](https://discord.gg/4CAahnd) there and talk to the node manager to help you make a choice.
{% endhint %}

## Apply Your Vote

To participate in the network, you will have to vote with your XCASH to elect a delegate that you see trustworthy and you want to help get a forging position. The DPOPS consensus has been designed so that the XCASH you use to vote stay in your wallet \(see [the challenge of voting in a privacy coin](https://docs.xcash.foundation/dpops/yellowpaper-delagated-proof-of-private-stake#the-challenges-of-staking-and-voting-in-a-privacy-coin)\)

### Download the wallet

Download the 2.0.0 version of the X-Cash CLI wallet, on our official website or on the GitHub Releases. 

### Vote

To vote, it's quite easy. Use the **`vote`** command: 

```text
vote <delegates_public_address|delegates_name>
```

You can either put the **`<delegates_public_address>`** which is a standard XCASH public address, or the **`<delegate_name>`**. Both of these information are available on the [delegate explorer](http://delegates.xcash.foundation/).

The wallet will create a reserve proof with the entirety of the wallet and assign it to the designated delegate.

{% hint style="danger" %}
There is a couple of rules to observe when voting: 

* You can only have one vote assigned per wallet. If you want to vote for another delegates, you will need to create a new wallet, and send it the XCASH you planned to stake.
* Votes are taken into account at the top of the next hour. 
{% endhint %}

