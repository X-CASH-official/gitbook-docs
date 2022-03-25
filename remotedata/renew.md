---
description: >-
  As an X-Cash namespace owner, you will need to renew your namespace every year.
---

# Introduction

You will need to renew your namespace, as they only last 1 year after every register or renewal. You can renew a namespace after it expires, but anyone else can as well, so make sure to renew it a few days before the expiration date. You can not currently renew early or for multiple years at this time.

You can always view all details (including registration time) of your namespace by visiting http://www.delegates.xcash.foundation/remotedatagetinformationfromname?parameter1=NAME (where NAME is your name of the namespace)

#### Renewal

Please use the [website](website) as the website will step by step walk you through what you need to do, with custom commands, as you will need to see real time delegate prices and who to send the xcash to, but the general process is described below.

Open and fully sync your wallet that is tied to the "address" field on your namespace.

You can view the previous block producer on the website with the price, but you can also use the [API](http://www.delegates.xcash.foundation/remotedatagetblockproducerinformation)

Once you have a delegate and price you want to use, run this command `remote_data_renewal_start`  

Once the command is complete you will have 24 hours to renew the domain. You will need to send a PUBLIC transaction to the delegate for the amount. You should then wait 30 minutes to make sure that transaction has been processed. Once this is done run this wallet command `remote_data_renewal_end <tx_hash>` 

Once done you can confirm using the above API to see if your namespace was successfully renewed.

