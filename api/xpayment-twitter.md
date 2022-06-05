# X-payment Twitter API

## Global Stats <a id="global-stats"></a>

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

## **get\_balance** <a id="get_balance"></a>

‌

Return the wallet's balance.‌

**Alias:** _getbalance_.‌

**Inputs:**‌

* _account\_index_ - unsigned int; Return balance for this account.
* _address\_indices_ - array of unsigned int; \(Optional\) Return balance detail for those subaddresses.

‌

**Outputs:**‌

* _balance_ - unsigned int; The total balance of the current xcash-wallet-rpc in session.
* _unlocked\_balance_ - unsigned int; Unlocked funds are those funds that are sufficiently deep enough in the X-Cash blockchain to be considered safe to spend.
* _multisig\_import\_needed_ - boolean; True if importing multisig data is needed for returning a correct balance.
* _per\_subaddress_ - array of subaddress information; Balance information for each subaddress in an account.
  * _address\_index_ - unsigned int; Index of the subaddress in the account.
  * _address_ - string; Address at this index. Base58 representation of the public keys.
  * _balance_ - unsigned int; Balance for the subaddress \(locked or unlocked\).
  * _unlocked\_balance_ - unsigned int; Unlocked balance for the subaddress.
  * _label_ - string; Label for the subaddress.
  * _num\_unspent\_outputs_ - unsigned int; Number of unspent outputs available for the subaddress.

‌

**Example:**

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_balance"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "balance": 562072541031,    "multisig_import_needed": false,    "per_subaddress": [{      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "address_index": 0,      "balance": 562072541031,      "label": "Primary account",      "num_unspent_outputs": 3,      "unlocked_balance": 562072541031    }],    "unlocked_balance": 562072541031  }}
```
