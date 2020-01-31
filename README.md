---
description: >-
  This documentation aims to aggregate all X-Cash related technical information
  while offering an overview of their implementation.
---

# Welcome

![](.gitbook/assets/header.png)

## About

X-Cash is an open-source decentralized cryptocurrency launched in 2018 with primary features being:

* Hybrid private/public transactions \(since Q4 2018\)  
* Delegated Proof-of-\(Private\)-Stake \(testnet Q3 2019\)  
* Sidechains \(2020\)

The document was written and aggregated by the X-Network team, the X-Cash development team and contributors. If you would like to learn more about contributing please the visit the [Github](https://github.com/X-CASH-official/).

## Easy onboarding

To help you navigate through this documentation, we aggregated the most common things people are looking for coming here.

### Getting Started

| What are you looking for? | Description |
| :--- | :--- |
| **I want to synchronize a node.** | Check the daemon documentation - [xcashd](documentation/xcashd.md) to understand how the daemon works. |
| **I want to create/open a wallet.** | Go to the CLI wallet documentation \([xcash-wallet-cli](documentation/xcash-wallet-cli.md)\) to learn everything about the X-CASH wallet and the different commands. |
| **Can I communicate to an XCASH wallet/daemon with RPC calls?** | Yes! Check out [xcash-daemon-rpc](documentation/json-rpc-methods/) and [xcash-wallet-rpc](documentation/xcash-wallet-rpc/) for a complete description of every RPC calls. |
| **I want to import a blockchain file.** | You can bootstrap your synchronization if you already have a RAW file of the blockchain. Check out the full documentation of [xcash-blockchain-import](documentation/xcash-blockchain-import.md) |
| **I want to install the DPoPS and test it.** | If you are part of the alpha tester panel, you can follow the [installation process](dpops/installation-process.md) and see the general information [there](dpops/get-started.md). |

### Useful Links

#### General links

* [**X-Network team website**](https://x-network.io) - Check the founder team company and current activities.
* [**X-Network blog**](https://medium.com/x-cash) - Read about the latest development and founding members public announcements.
* [**X-Cash website** ](https://x-network.io/xcash)- Get an overview of the X-Cash project.
* [**X-Cash explorer** ](https://explorer.x-cash.org/Explorer)- See your ongoing transactions and explorer the X-CASH blockchain.
* [**GitHub**](https://github.com/X-CASH-official) - The X-Cash source code and related repositories. 
* [**Help Center**](https://xcashteam.atlassian.net/servicedesk) - If you encounter a bug, or want to communicate on improvements.

#### Technical Documentation

* \*\*\*\*[**FlexPrivacy**](https://x-network.io/whitepaper/XCASH_Yellowpaper_Hybrid-tx.pdf) - Public and Private transactions on the X-Cash blockchain.
* [**DPoPS**](https://x-network.io/whitepaper/XCASH_Yellowpaper_DPoPS.pdf) - Delegated Proof-of-Private-Stake, a DPoS implementation under X-Cash.

#### Join the community

* [**Discord** ](https://discord.gg/4CAahnd)- Engage with the community, ask questions and talk with the team.
* [**Twitter**](https://twitter.com/home) - Follow us for weekly updates, contests and community polls.
* [**Reddit**](https://www.reddit.com/r/xcash) - Join the Reddit community
* [**Telegram**](https://t.me/xcashglobal) - Get the latest news and announcement of anything X-CASH related

## Sources

The technical documentation has been copied and adapted from Monero's documentation: [**https://monerodocs.org/interacting/monerod-reference/**](https://monerodocs.org/interacting/monerod-reference/)\*\*\*\*

Wallet RPC wallet calls and additional examples: [**https://www.getmonero.org/resources/developer-guides/wallet-rpc.html**](https://www.getmonero.org/resources/developer-guides/wallet-rpc.html)\*\*\*\*

Daemon RPC calls and additional examples: [**https://web.getmonero.org/resources/developer-guides/daemon-rpc.html**](https://web.getmonero.org/resources/developer-guides/daemon-rpc.html)\*\*\*\*

Implementation in python: [**https://moneroexamples.github.io/python-json-rpc/**](https://moneroexamples.github.io/python-json-rpc/)\*\*\*\*

## Versions

### 2.0.0-Alpha

* Added documentation for the [Delegated-Proof-of-Private-Stake](dpops/get-started.md)

### 1.4.0

The 1.4.0 release is a significant upgrade of the X-Cash network by notably introducing:

* **bulletproof transactions** \(reduction of transaction size by up to 80%\)  
* **hybrid public/private transactions**  
* **ASIC/NiceHash resistant mining algorithm**  

  For more information about the release, you can read the dedicated article [here](https://medium.com/x-cash/x-cash-major-update-1-4-0-flexprivacy-cnv2-bulletproof-and-fixed-ring-size-106a20ce0b06).  

  For more information about hybrid transactions, you can read the dedicated paper [here](https://x-network.io/whitepaper/XCASH_Yellowpaper_Hybrid-tx.pdf).

### 1.0.0

