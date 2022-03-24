---
description: >-
  X-Cash DPoPS (Delegated-Proof-of-Private-Stake) is a variation of DPoS and
  DBFT consensus.‌
---

# Get Started

## Introduction

The X-Cash Public Network is governed by 50 delegates through a custom Delegated Proof of Stake \(DPoS\) consensus, the [Delegate Proof of Private Stake](yellowpaper-delagated-proof-of-private-stake.md). Anyone can run an X-Cash node and become a delegate. But to be able to forge new blocks and earn the reward, you will need to be voted into the top 50 by other members of the community.

Becoming a delegate is ambitious, and reaching and keeping a forging spot is rewarding but can be taken out at any time. As a delegate, you will have to prove that you are fit to keep this role. This documentation will help you get set up, register as a delegate, start forging, and securing the X-Cash Network.

## Get Started

<table>
  <thead>
    <tr>
      <th style="text-align:left">What are you looking for?</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>What is DPoPS? What&apos;s unique about this consensus?</b>
      </td>
      <td style="text-align:left">We recommend that you have a look at the <a href="yellowpaper-delagated-proof-of-private-stake.md">technical design</a> of
        the consensus, which as been built from the ground up to work with X-Cash.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>I want to become a delegate and set up a validator node.</b>
      </td>
      <td style="text-align:left">First, follow the <a href="server-setup.md">server setup guide</a> if you
        are not familiar with Linux instances. Then, follow the <a href="node-installation.md">node program installation</a> to
        install the necessary program to run the consensus. Finally, see the instructions
        to<a href="register-delegate.md"> register yourself as a delegate</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>I want to vote.</b>
      </td>
      <td style="text-align:left">Have a look at our quick <a href="vote-and-staking.md">vote &amp; staking</a> guide.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>I want to participate in the beta.</b>
      </td>
      <td style="text-align:left">
        <p>If you want to become a delegate during the beta, you will need to install
          a server as per<a href="server-setup.md"> the guide</a>, install the
          <a
          href="node-installation.md">node program</a>and <a href="register-delegate.md">register yourself</a> as
            a delegate. Then, follow up on the additional instructions to <a href="../archived/dpops-beta.md">join the beta</a>.</p>
        <p>If you just want to vote, the instructions are indicated in <a href="vote-and-staking.md">vote &amp; staking</a>.</p>
      </td>
    </tr>
  </tbody>
</table>

## Key features <a id="key-features"></a>

#### Characteristics

* The top 50 delegates are elected as block verifiers. 
* Using a variation of Delegated Byzantine Fault tolerance consensus where 55% consensus must be reached for a new block to be added to the network.
* DBFT allows for up to 45% of the elected block verifiers to be faulty. The system will still be able to produce a new block.
* Using Verifiable Random Functions \(VRF\) to select the next block producer in the system. This allows for a random, but a verifiable way of selecting the next block producer.

#### Staking

* Reserve proof-based voting/staking system: the XCASH's stake always stays in your wallet.
* There is no lockup period. The staked XCASH always remain in your wallet and you can use them at any time. However, moving them from your wallet will cancel your entire staking
* No need to keep your wallet/computer online if you are staking towards a shared delegate

#### Voting

* The minimum stake to vote for a delegate is 2 million XCASH.
* The election process occurs in every block.
* New votes will be taken into account for the next block.
* Your current vote will automatically get cancelled if you change your vote to a different delegate.
* There aer no voting fees. You can cast a new vote or switch your vote as many time as you like.

#### Blocks

* Blocks can be verified in the X-Cash's Daemon with a detailed explanation of the block content.
* The voting and reserve bytes data is held in a decentralized database system.
* The block format is designed to store a hash of the reserve bytes data, pointing to the decentralized database, to reduce size increase of the blockchain.

## Useful Links <a id="key-features"></a>

* \*\*\*\*[**GitHub**](https://github.com/X-CASH-official/xcash-dpops) - Source code of the DPoPS program.
* \*\*\*\*[**Technical documentation**](yellowpaper-delagated-proof-of-private-stake.md) - Delegated Proof-of-Private-Stake, a DPoS implementation under X-Cash.
* \*\*\*\*[**Discord Server**](https://discord.gg/4CAahnd) - Join the Discord server to announce yourself as a delegate.

