---
description: This page contains every Daemon RPC calls related to DPoPS functions.
---

# DPoPS Daemon RPC Calls

{% hint style="info" %}
As of 30/09/2019, these calls will not work until the mainnet is released. Only alpha and beta testers that installed the testnet of DPoPS can try them.
{% endhint %}

## get\_path

TO COMPLETE

**Alias**: _getpath_.

**Inputs**: _None_.

**Outputs**:

* _path_ - string; TO COMPLETE
* _status_ - string; General RPC error code. "OK" means everything looks good.

```bash
$ curl -X POST http://localhost:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_path"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "path": "\/root\/xcash-official\/verify_block.txt",
    "status": "OK"
  }
}
```

