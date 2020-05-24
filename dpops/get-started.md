---
description: >-
  X-Cash DPoPS (Delegated-Proof-of-Private-Stake) is a variation of DPoS and
  DBFT consensus.‌
---

# Introduction

## Get Started

| What are you looking for? | Description |
| :--- | :--- |
| **What is DPoPS? What's unique about this consensus?** | We recommend that you have a look at the [technical design](yellowpaper-delagated-proof-of-private-stake.md) of the consensus, which as been built from the ground up to work with X-Cash. |
| **I want to become a delegate and set up a validator node.** | Follow our [installation process](installation-process/) to get started.  We recommend that you have a look at our [server setup guide](server-setup.md) if it's your first time setting up a Linux server. |

## Key features <a id="key-features"></a>

#### Characteristics 📃

* The top 50 delegates \(currently\) are elected as block verifiers. 
* Using a variation of Delegated Byzantine Fault tolerance consensus where 67% consensus must be reached for a new block to be added to the network.
* DBFT allows for up to 33% of the elected block verifiers to be faulty. The system will still be able to produce a new block.
* Using Verifiable Random Functions \(VRF\) to select the next block producer in the system. This allows for a random, but verifiable way of selecting the next block producer.

#### Staking 💰

* Reserve proof-based voting/staking system: the XCASH's stake always stays in your wallet.
* There is no lockup period. The staked XCASH always remain in your wallet and you can use them at any time. However, moving them from your wallet will cancel your entire staking
* No need to keep your wallet/computer online if you are staking towards a shared delegate

#### Voting ❎

* The minimum stake to vote for a delegate is 2 million XCASH.
* The election process occurs every block.
* New votes will be taken into account for the next block.
* Your current vote will automatically get cancelled if you change your vote to a different delegate.
* There is no voting fees. You can cast a new vote or switch your vote as many time as you like.

#### Blocks ⬛

* Blocks can be verified in the X-Cash's Daemon with a detailed explanation of the block content.
* The voting and reserve bytes data is held in a decentralized database system.
* The block format is designed to store a hash of the reserve bytes data, pointing to the decentralized database, to reduce size increase of the blockchain.

## Useful Links <a id="key-features"></a>

* \*\*\*\*[**GitHub**](https://github.com/X-CASH-official/xcash-dpops) - Source code of the DPoPS program.
* \*\*\*\*[**Technical documentation**](yellowpaper-delagated-proof-of-private-stake.md) - Delegated Proof-of-Private-Stake, a DPoS implementation under X-Cash.
* \*\*\*\*[**Discord Server**](https://discord.gg/4CAahnd) - Join the Discord server to announce yourself as a delegate.

