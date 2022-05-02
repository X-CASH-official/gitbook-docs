---
description: >-
  How to send a turbo tx.
---

# Introduction

You can send turbo tx from any wallet. If using a GUI based wallet their should be a slider to do this. In the CLI wallet you can specifiy the public or private tx style by using `transfer public XCA 100` or `transfer private XCA 100`. If left blank it will default to private.

You now can specifiy the turbo tx settings by using `transfer tpublic XCA 100` or `transfer tprivate XCA 100`. If left blank it will default to a non turbo private tx.

You can also use the `tpublic` and `tprivate` fields for the `tx_privacy_settings` field on the RPC.

Once you send a turbo transaction, you will receive the same details like any other transaction, but will also receive a link to a url and a "turbo tx id". You can give this url to the receiver to have them instantly verify your tx.
