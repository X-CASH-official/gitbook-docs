---
description: >-
  X-Cash DPoPS (Delegated-Proof-of-Private-Stake) is a variation of DPoS and
  DBFT consensus.‌
---

# Get Started

## Getting Started

| What are you looking for? | Description |
| :--- | :--- |
| **I want to install the DPoPS program.** | All the steps and dependencies are indicated in the [installation process ](installation-process.md)page. |
| **I want to set up and configure my node.** | We have gathered the configuration steps in the set up your node page. |
| **I want to join the Alpha test.** | First, it is hasn't been done already, send your application by filling up this [form](https://forms.gle/eXUbeTzwmure6xsi9). Then, you can check the necessary steps to participate in the alpha. |

## Useful Links <a id="key-features"></a>

* [**Source code**](https://github.com/X-CASH-official/XCASH_DPOPS) - Open-source code of X-Cash DPOPS.
* [**Yellowpaper**](../library/yellowpaper-delagated-proof-of-private-stake.md) - Delegated Proof-of-Private-Stake, a DPoS implementation under X-Cash.
* \*\*\*\*[**Discord**](https://discord.gg/wXFGERr) - Join the Discord server and communicate directly with the team.

## Key features <a id="key-features"></a>

### Characteristics <a id="characteristics"></a>

* The top 100 delegates are elected as block verifiers.
* Using a variation of Delegated Byzantine Fault tolerance consensus where 67% consensus must be reached for a new block to be added to the network.
* DBFT allows for up to 33% of the elected block verifiers to be faulty. The system will still be able to produce a new block.
* Using Verifiable Random Functions \(VRF\) to select the next block producer in the system. This allows for a random, but verifiable way of selecting the next block producer.

### Staking <a id="staking"></a>

* **Reserve proof-**based voting/staking system: the XCASH's stake always stays in your wallet.
* There is no lockup period. The staked XCASH always remain in your wallet and you can use them at any time. However, moving them from your wallet will cancel your entire staking
* No need to keep your wallet/computer online if you are staking towards a shared delegate

### Voting <a id="voting"></a>

* The minimum stake to vote for a delegate is 2 million XCASH.
* The election process occurs every block.
* New votes will be taken into account for the next block.
* Your current vote will automatically get cancelled if you change your vote to a different delegate.
* There is no voting fees. You can cast a new vote or switch your vote as many time as you like.

### Blocks <a id="blocks"></a>

* Blocks can be verified in the X-Cash's Daemon with a detailed explanation of the block content.
* The voting and reserve bytes data is held in a decentralized database system.
* The block format is designed to store a hash of the reserve bytes data, pointing to the decentralized database, to reduce size increase of the blockchain.

If you are running a solo node, you don't need to keep your XCASH directly on the server. You can use an empty wallet on the server and vote for yourself.‌

