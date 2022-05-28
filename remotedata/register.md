# Introduction

You can register a namespace to associate public addresses with a name.

#### Renewal

Please use the [website](website) as the website will step by step walk you through what you need to do, with custom commands, as you will need to see real time delegate prices and who to send the xcash to, but the general process is described below.

Open and fully sync your wallet that you want to register. This will tied to the NAME.xcash namespace. You will also need two other addresses. One that you want to be tied to NAME.sxcash that can only send and receive private transactions, and one that you want to be tied to NAME.pxcash that can only send and receive public transactions. It is highly reccomended you use two new wallets for this.

You can view if a name is avalible by using http://www.delegates.xcash.foundation//remotedatagetnamestatus?parameter1=NAME

You can view the previous block producer on the website with the price, but you can also use the [API](http://www.delegates.xcash.foundation/remotedatagetblockproducerinformation)

Once you have a delegate and price you want to use for name, run this command `remote_data_save_name <name>`  

Once the command is complete you will have 24 hours to register the domain. You will need to send a PUBLIC transaction to the delegate for the amount. You should then wait 30 minutes to make sure that transaction has been processed. Once this is done run this wallet command `remote_data_purchase_name <saddress> <saddress_signature> <paddress> <paddress_signature> <tx_hash>`  where saddress is the private only address and paddress is the public only address. To get the saddress_signature you need to open the saddress wallet and run `sign YOUR_SADDRESS_PUBLIC_ADDRESS` and it will save the signature in a file, that is the saddress_signature. You need to do the same for the paddress wallet. This is needed because it proves ownership of the wallets you want to register.

Once done you can confirm using the above API to see if your namespace was successfully registered.

