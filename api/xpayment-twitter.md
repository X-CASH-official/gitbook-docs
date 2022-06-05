# X-payment Twitter API

## Global Stats

This method gets the global stats

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/)

**Method**: GET

**Inputs**: _None_.

**Results**:

* _height_ - unsigned int; Current length of longest chain known to daemon.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/ -H 'Content-Type: application/json'
{
  "height": 425000,
  "status": "OK",
  "untrusted": false
}
```
