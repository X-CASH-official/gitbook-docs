---
description: >-
  Once your server is set up and the necessary programs installed, you will be
  able to set your register/renewal price as a delegate.
---

# Register Delegate

## Introduction

Once you have correctly [set up your instance](server-setup.md), installed the different programs, you can set your register/renewal price as a delegate. If your the previous block producer, users will see your delegate and your price on  <a href="website">website</a> and can choose to use your delegate or wait for another maybe cheaper delegate. They can also view all delegates prices on the website.

## 2. Update Register/Renewal Price

By default your price will be 100M xcash until you update it.

First of all, the wallet service should be running in the background. Stop it by using the command:

```text
systemctl stop xcash-rpc-wallet
```

Open and let synchronize the wallet you used to register as a delegate, either when using the [auto-installer](node-installation.md#quick-installation) or [created manually](node-installation.md#generate-a-wallet).

```text
~/xcash-official/xcash-core/build/release/bin/xcash-wallet-cli --wallet-file ~/xcash-official/xcash-wallets/<WALLET_NAME>
```

Replace the **`<WALLET_NAME>`** with your own.

{% hint style="info" %}
If you installed with the autoinstaller script, the wallet name will be **`delegate-wallet`**
{% endhint %}

Once your wallet is fully synchronized, you can use the **`remote_data_delegates_set_amount`** command with the following parameters:

```text
remote_data_delegates_set_amount <amount>
```

Replace the `<amount>` with the amount of xcash (has to be whole numbers and in regular units) you want to charge for registering and renewal of names in the xcash namespace.

You will be prompted to wait for the next valid data interval (at most 10 inutes). Once your request has been accepted, you will receive the message **`The remote data amount has been updated successfully`**.

You can **`exit`** the wallet and restart the wallet service:

```text
systemctl start xcash-rpc-wallet
```

