---
description: >-
  Use X-Cash RPC Wallet (xcash-wallet-rpc) to interact with a wallet through
  JSON-RPC commands
---

# xcash-wallet-rpc

## JSON RPC Methods <a id="json-rpc-methods"></a>

## Introduction <a id="introduction"></a>

‌

This is a list of the xcash-wallet-rpc calls, their inputs and outputs, and examples of each. The program xcash-wallet-rpc replaced the rpc interface that was in simplewallet and then xcash-wallet-rpc.‌

All xcash-wallet-rpc methods use the same JSON RPC interface. For example:

```text
IP=127.0.0.1PORT=18082METHOD="make_integrated_address"PARAMS="{\"payment_id\":\"1234567890123456789012345678900012345678901234567890123456789000\"}"curl \    -X POST http://$IP:$PORT/json_rpc \    -d '{"jsonrpc":"2.0","id":"0","method":"'$METHOD'","params":'"$PARAMS"'}' \    -H 'Content-Type: application/json'
```

‌

If the xcash-wallet-rpc was executed with the `--rpc-login` argument as `username:password`, then follow this example:

```text
IP=127.0.0.1PORT=18082METHOD="make_integrated_address"PARAMS="{\"payment_id\":\"1234567890123456789012345678900012345678901234567890123456789000\"}"curl \    -u username:password --digest \    -X POST http://$IP:$PORT/json_rpc \    -d '{"jsonrpc":"2.0","id":"0","method":"'$METHOD'","params":'"$PARAMS"'}' \    -H 'Content-Type: application/json'
```

‌

Note: "[atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html)" refer to the smallest fraction of 1 XCASH according to the xcashd implementation. **1 XCASH = 1e6** [**atomic units**](https://web.getmonero.org/resources/moneropedia/atomic-units.html)**.**‌

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

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_balance"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "balance": 562072541031,    "multisig_import_needed": false,    "per_subaddress": [{      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "address_index": 0,      "balance": 562072541031,      "label": "Primary account",      "num_unspent_outputs": 3,      "unlocked_balance": 562072541031    }],    "unlocked_balance": 562072541031  }}
```

‌

## **get\_address** <a id="get_address"></a>

‌

Return the wallet's addresses for an account. Optionally filter for specific set of subaddresses.‌

**Alias:** _getaddress_.‌

**Inputs:**‌

* _account\_index_ - unsigned int; Return subaddresses for this account.
* _address\_index_ - array of unsigned int; \(Optional\) List of subaddresses to return from an account.

‌

**Outputs:**‌

* _address_ - string; The 95-character hex address string of the xcash-wallet-rpc in session.
* _addresses_ array of addresses informations
  * _address_ string; The 95-character hex \(sub\)address string.
  * _label_ string; Label of the \(sub\)address
  * _address\_index_ unsigned int; index of the subaddress
  * _used_ boolean; states if the \(sub\)address has already received funds

‌

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_address"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",    "addresses": [{      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "address_index": 0,      "label": "Primary account",      "used": true    }]  }}
```

‌

## **get\_address\_index** <a id="get_address_index"></a>

‌

Get account and address indexes from a specific \(sub\)address‌

**Alias:** _None_.‌

**Inputs:**‌

* _address_ - String; \(sub\)address to look for.

‌

**Outputs:**‌

* _index_ - subaddress informations
  * _major_ unsigned int; Account index.
  * _minor_ unsigned int; Address index.

‌

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_address_index","params":{"address":"83nWyFM8HtqikdcFPXhfj3Z7Jvh1LDjfe7B1Fo6bUNUZMjm1JTXcnyzb3iiqUHLkcAQ5t6b5u8WhgTFNuzEpL27C3CvD1d8"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "index": {      "major": 0,      "minor": 1    }  }}
```

‌

## **create\_address** <a id="create_address"></a>

‌

Create a new address for an account. Optionally, label the new address.‌

**Alias:** _None_.‌

**Inputs:**‌

* _account\_index_ - unsigned int; Create a new address for this account.
* _label_ - string; \(Optional\) Label for the new address.

‌

**Outputs:**‌

* _address_ - string; Newly created address. Base58 representation of the public keys.
* _address\_index_ - unsigned int; Index of the new address under the input account.

‌

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"create_address","params":{"account_index":0,"label":"new-sub"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "address": "83nWyFM8HtqikdcFPXhfj3Z7Jvh1LDjfe7B1Fo6bUNUZMjm1JTXcnyzb3iiqUHLkcAQ5t6b5u8WhgTFNuzEpL27C3CvD1d8",    "address_index": 1  }}
```

‌

## **label\_address** <a id="label_address"></a>

‌

Label an address.‌

**Alias:** _None_.‌

**Inputs:**‌

* _index_ - subaddress index; JSON Object containing the major & minor address index:
  * _major_ - unsigned int; Account index for the subaddress.
  * _minor_ - unsigned int; Index of the subaddress in the account.
* _label_ - string; Label for the address.

‌

**Outputs:** _None_.‌

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"label_address","params":{"index":{"major":0,"minor":1},"label":"myLabel"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **get\_accounts** <a id="get_accounts"></a>

‌

Get all accounts for a wallet. Optionally filter accounts by tag.‌

**Alias**: _None_.‌

**Inputs**:‌

* _tag_ - string; \(Optional\) Tag for filtering accounts.

‌

**Outputs**:‌

* _subaddress\_accounts_ - array of subaddress account information:
  * _account\_index_ - unsigned int; Index of the account.
  * _balance_ - unsigned int; Balance of the account \(locked or unlocked\).
  * _base\_address_ - string; Base64 representation of the first subaddress in the account.
  * _label_ - string; \(Optional\) Label of the account.
  * _tag_ - string; \(Optional\) Tag for filtering accounts.
  * _unlocked\_balance_ - unsigned int; Unlocked balance for the account.
* _total\_balance_ - unsigned int; Total balance of the selected accounts \(locked or unlocked\).
* _total\_unlocked\_balance_ - unsigned int; Total unlocked balance of the selected accounts.

‌

**Example**:

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_accounts","params":{"tag":"myTag"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "subaddress_accounts": [{      "account_index": 0,      "balance": 100000000000,      "base_address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "label": "Primary account",      "tag": "myTag",      "unlocked_balance": 100000000000    },{      "account_index": 1,      "balance": 0,      "base_address": "83nWyFM8HtqikdcFPXhfj3Z7Jvh1LDjfe7B1Fo6bUNUZMjm1JTXcnyzb3iiqUHLkcAQ5t6b5u8WhgTFNuzEpL27C3CvD1d8",      "label": "Secondary account",      "tag": "myTag",      "unlocked_balance": 0    }],    "total_balance": 100000000000,    "total_unlocked_balance": 100000000000  }}
```

‌

## **create\_account** <a id="create_account"></a>

‌

Create a new account with an optional label.‌

**Alias**: _None_.‌

**Inputs**:‌

* _label_ - string; \(Optional\) Label for the account.

‌

**Outputs**:‌

* _account\_index_ - unsigned int; Index of the new account.
* _address_ - string; Address for this account. Base58 representation of the public keys.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"create_account","params":{"label":"Secondary account"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "account_index": 1,    "address": "83nWyFM8HtqikdcFPXhfj3Z7Jvh1LDjfe7B1Fo6bUNUZMjm1JTXcnyzb3iiqUHLkcAQ5t6b5u8WhgTFNuzEpL27C3CvD1d8"  }}
```

‌

## **label\_account** <a id="label_account"></a>

‌

Label an account.‌

**Alias**: _None_.‌

**Inputs**:‌

* _account\_index_ - unsigned int; Apply label to account at this index.
* _label_ - string; Label for the account.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"label_account","params":{"account_index":0,"label":"Primary account"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "account_tags": [{      "accounts": [0,1],      "label": "",      "tag": "label"    }]  }}
```

‌

## **get\_account\_tags** <a id="get_account_tags"></a>

‌

Get a list of user-defined account tags.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _account\_tags_ - array of account tag information:
  * _tag_ - string; Filter tag.
  * _label_ - string; Label for the tag.
  * _accounts_ - array of int; List of tagged account indices.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"label_account","params":{"account_index":0,"label":"Primary account"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "account_tags": [{      "accounts": [0,1],      "label": "",      "tag": "myTag"    }]  }}
```

‌

## **tag\_accounts** <a id="tag_accounts"></a>

‌

Apply a filtering tag to a list of accounts.‌

**Alias**: _None_.‌

**Inputs**:‌

* _tag_ - string; Tag for the accounts.
* _accounts_ - array of unsigned int; Tag this list of accounts.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"tag_accounts","params":{"tag":"myTag","accounts":[0,1]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **untag\_accounts** <a id="untag_accounts"></a>

‌

Remove filtering tag from a list of accounts.‌

**Alias**: _None_.‌

**Inputs**:‌

* _accounts_ - array of unsigned int; Remove tag from this list of accounts.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"untag_accounts","params":{"accounts":[1]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **set\_account\_tag\_description** <a id="set_account_tag_description"></a>

‌

Set description for an account tag.‌

Alias: _None_.‌

**Inputs**:‌

* _tag_ - string; Set a description for this tag.
* _description_ - string; Description for the tag.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_account_tag_description","params":{"tag":"myTag","description":"Test tag"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **get\_height** <a id="get_height"></a>

‌

Returns the wallet's current block height.‌

**Alias**: _getheight_‌

**Inputs**: _None_.‌

**Outputs**:‌

* _height_ - unsigned int; The current xcash-wallet-rpc's blockchain height. If the wallet has been offline for a long time, it may need to catch up with the daemon.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_height"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "height": 425000  }}
```

‌

## **transfer** <a id="transfer"></a>

‌

Send XCASH to a number of recipients.‌

**Alias**: _None_.‌

**Inputs**:‌

* _destinations_ - array of destinations to receive XCASH:
  * _amount_ - unsigned int; Amount to send to each destination, in [atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html).
  * _address_ - string; Destination public address.
* _account\_index_ - unsigned int; \(Optional\) Transfer from this account index. \(Defaults to 0\)
* _subaddr\_indices_ - array of unsigned int; \(Optional\) Transfer from this set of subaddresses. \(Defaults to empty - all indices\)
* _priority_ - unsigned int; Set a priority for the transaction. Accepted Values are: 0-3 for: default, unimportant, normal, elevated, priority.
* _mixin_ - unsigned int; Number of outputs from the blockchain to mix with \(0 means no mixing\).
* _ring\_size_ - unsigned int; Number of outputs to mix in the transaction \(this output + N decoys from the blockchain\).
* _unlock\_time_ - unsigned int; Number of blocks before the XCASH can be spent \(0 to not add a lock\).
* _payment\_id_ - string; \(Optional\) Random 32-byte/64-character hex string to identify a transaction.
* _get\_tx\_key_ - boolean; \(Optional\) Return the transaction key after sending.
* _do\_not\_relay_ - boolean; \(Optional\) If true, the newly created transaction will not be relayed to the X-Cash network. \(Defaults to false\)
* _get\_tx\_hex_ - boolean; Return the transaction as hex string after sending \(Defaults to false\)
* _get\_tx\_metadata_ - boolean; Return the metadata needed to relay the transaction. \(Defaults to false\)

‌

**Outputs**:‌

* _amount_ - Amount transferred for the transaction.
* _fee_ - Integer value of the fee charged for the txn.
* _multisig\_txset_ - Set of multisig transactions in the process of being signed \(empty for non-multisig\).
* _tx\_blob_ - Raw transaction represented as hex string, if get\_tx\_hex is true.
* _tx\_hash_ - String for the publically searchable transaction hash.
* _tx\_key_ - String for the transaction key if get\_tx\_key is true, otherwise, blank string.
* _tx\_metadata_ - Set of transaction metadata needed to relay this transfer later, if get\_tx\_metadata is true.
* _unsigned\_txset_ - String. Set of unsigned tx for cold-signing purposes.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"transfer","params":{"destinations":[{"amount":100000000000,"address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe"}],"ring_size":21,"get_tx_keys": true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "amount": 100000000000,    "fee": 1348040,    "multisig_txset": "",    "tx_hash": "cd97adf7372e620cda46ed623f89fca1411873f5d7251359ab468c287d8f9fee",    "tx_key": "8037f31fd7b9c76d432e6c0c14b549ea68fdd84537f75c6e9a066890f7840d0b",    "unsigned_txset": ""  }}
```

‌

## **transfer\_split** <a id="transfer_split"></a>

‌

Same as transfer, but can split into more than one tx if necessary.‌

**Alias**: _None_.‌

**Inputs**:‌

* _destinations_ - array of destinations to receive XCASH:
  * _amount_ - unsigned int; Amount to send to each destination, in [atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html).
  * _address_ - string; Destination public address.
* _account\_index_ - unsigned int; \(Optional\) Transfer from this account index. \(Defaults to 0\)
* _subaddr\_indices_ - array of unsigned int; \(Optional\) Transfer from this set of subaddresses. \(Defaults to empty - all indices\)
* _mixin_ - unsigned int; Number of outputs from the blockchain to mix with \(0 means no mixing\).
* _ring\_size_ - unsigned int; Sets ringsize to n \(mixin + 1\).
* _unlock\_time_ - unsigned int; Number of blocks before the XCASH can be spent \(0 to not add a lock\).
* _payment\_id_ - string; \(Optional\) Random 32-byte/64-character hex string to identify a transaction.
* _get\_tx\_keys_ - boolean; \(Optional\) Return the transaction keys after sending.
* _priority_ - unsigned int; Set a priority for the transactions. Accepted Values are: 0-3 for: default, unimportant, normal, elevated, priority.
* _do\_not\_relay_ - boolean; \(Optional\) If true, the newly created transaction will not be relayed to the X-Cash network. \(Defaults to false\)
* _get\_tx\_hex_ - boolean; Return the transactions as hex string after sending
* _new\_algorithm_ - boolean; True to use the new transaction construction algorithm, defaults to false.
* _get\_tx\_metadata_ - boolean; Return list of transaction metadata needed to relay the transfer later.

‌

**Outputs**:‌

* _tx\_hash\_list_ - array of: string. The tx hashes of every transaction.
* _tx\_key\_list_ - array of: string. The transaction keys for every transaction.
* _amount\_list_ - array of: integer. The amount transferred for every transaction.
* _fee\_list_ - array of: integer. The amount of fees paid for every transaction.
* _tx\_blob\_list_ - array of: string. The tx as hex string for every transaction.
* _tx\_metadata\_list_ - array of: string. List of transaction metadata needed to relay the transactions later.
* _multisig\_txset_ - string. The set of signing keys used in a multisig transaction \(empty for non-multisig\).
* _unsigned\_txset_ - string. Set of unsigned tx for cold-signing purposes.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"transfer_split","params":{"destinations":[{"amount":100000000000,"address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe"}],"ring_size":21,"get_tx_keys": true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "amount_list": [100000000000],    "fee_list": [1348040],    "multisig_txset": "",    "tx_hash_list": ["cd97adf7372e620cda46ed623f89fca1411873f5d7251359ab468c287d8f9fee"],    "tx_key_list": ["8037f31fd7b9c76d432e6c0c14b549ea68fdd84537f75c6e9a066890f7840d0b"],    "unsigned_txset": ""  }}
```

‌

## **sign\_transfer** <a id="sign_transfer"></a>

‌

Sign a transaction created on a read-only wallet \(in cold-signing process\)‌

**Alias**: _None_.‌

**Inputs**:‌

* _unsigned\_txset_ - string. Set of unsigned tx returned by "transfer" or "transfer\_split" methods.
* _export\_raw_ - boolean; \(Optional\) If true, return the raw transaction data. \(Defaults to false\)

‌

**Outputs**:‌

* _signed\_txset_ - string. Set of signed tx to be used for submitting transfer.
* _tx\_hash\_list_ - array of: string. The tx hashes of every transaction.
* _tx\_raw\_list_ - array of: string. The tx raw data of every transaction.

‌

In the example below, we first generate an unsigned\_txset on a read only wallet before signing it:‌

Generate unsigned\_txset using the above "transfer" method on read-only wallet:

```text
curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sign_transfer","params":{"unsigned_txset":"0200010200158b9c3dc885308d9024ec8302c2e5109ad803d3f801de53d21493738e1f90e701dc5e801302d8161a8105be042aa807cb560557a7d0d4898cc6fd0c0d35e77bcf839ba9b235ec3674bf00cb90f32a8b0200020b85f8bb5eae185399c32fee2ef11e3a10fed9b998280d9ef2161c70b1f353c50002417be46e074735ed3d5c7147aa472e9c14a401b9ebd58985d20e5f6eeb70e4d72101f27d6926626deaf4506c94ff24f0abe117dbd54c4f28da7a1b13047b6b2878a403fae733aa829ee9d1fc948e393f3d7368cb537e00290732c46adc46e441d2c275cdd606c19730f020ee0da02dbd90d4cd4ce7abac0413c8d51f21f7fea951a963cb33083359a00a3180d734589aec044d7d37278fb3f469fdde70156a8803df822a1e0f441426e7d933e18aeb25e58acc8e58b36d33927775501bbf1bbf32483a1ce00b21795f315cd231b11fe58e6c8407bb21d19250b91754a94606e73e12a88c7600115b5af4112e3830928dec18a552ecee1640ecdd7157f37b5fe9aeb83321c7c7010000004fda960fdb48fd706038d36af0e134ff6e770993295261af08feca3ef13394a32999b0ed41d8628b3d474a91fbec325cfef5ed11e8f3bd77e7d9a9a66e4adafc0385491182a5ef73957497f1dd3c2b680b8e63ad869efa209f43b6322770d58220ed31a3f7a9489535a75a88f9f06c5a63d1028a5b56e72e2ae152f15ac65e12e3bd1cc54f2a6bb8383c8850c29a525a30d5f99d41788fb5a7f5eef69244ec06820d2ec4507b4ca2d15970d063fc8e1edef757a647f225b865092295252ab10d07559ce962bab5b5317d9e5714b2423c7b2181fe989071f858dec30ecf1ab5e4430b8764c2a6f6190c9806563ae80090adf044d28316731f7e4e56a75dcee9e5640f27cf889413e22971496edacf434ea5dc1acee029be37f71c1531ccf5af45632f658a5406a6c0de9804fc12ccb40168f2dbddf7f086e58ac6acd79c868ef865f037cd75d072466b892df28812b4ad0c397b0dc1bc1b2ba6a924117f210851419a1f95fb63f88afdf1bdd52e1d65d010632b119bedbc2ea91ea99886bec9612047f4210c3b608edaf97ee6734a7cf22a6dde09c6a36b628f67f7058614f67444076003291b8df917842102167bbf28a9c6d074e68ecfc40ba55371355f2231b5363304836b9faf7fdeeb16f5739c05b310d434235044bf1216a242fcc6faeee198a12480283e8b438c02a3f0ff9cae741a1d4207c26799f0df255639d36ba3a0546f0e7017ca557ad680f77d5ff77ba434ae55db821c5579cf00d63185c1c477bee35e8a2cce838939b0e849182c486d8915f003f130f16d233f3f404ffac45f8436e80058e1b81ce3c3b326f57bd878d6b748914d670557353bca7f98d53fdf57782c65223aed33d0aeadc777120979ade3fb429b5a5486c967f4c2c63b5686a6a38cf329858f82b4d33be6df08419e97c1872a6c6191d5811b679d2827fb3306f9b634f7898b0207868c77e0a6033df229ec2ee28cd2c79ba01e49b67955300891e95765c53c25d3a7ceca2d4fab3944d30407797e01996d23b1ef2188bfd00662251c3f137bab02e4a831c47d722043ba4c780e5fa7d70e36968b7254b36d04ed82577fffb738086390eae9be1b800e8c19df14565ab13f4cda135220bf4a087604caa94cb26bf32ec920a2d19899b16c7d48acf285b80ebf412ed76b485f0c0b87cd272fe6fc80a82c98326397bcd408013113cbecca24ba03c36142ceb40d9d7b3ce7c9eeb71fa8c21e52cade31567c20acfa535250d1d95493c6f1e18902a5afbb5efc96a62b9d03a782536399f67a068c60d5b87144702debde82652c0f60e8298b66130176e39ef2dc754e11e498363b43a50655fc5d8a0ad07b93710d653e9ba5bda4772b2fef001990babfcaef513cb2b4f7dfc4d14cd79ca7cc6306545ac279b87a08dcedfabba270505fa6b60de12c29dd62d6bda33614e0ff6d00fd143259ae424d7a74b506b13cec4d16cd6519199862dfe2d768b6024c2a0703260e696f84a755b96272b314d2c9614c90ed4389beae781b9abed3801e047b042c5f6c1c7273737835b94c281766a7086692c339c9a18425cbc4060f1c1b9f0e3eab4bc3f891bc7c245aec18045eb0ed133acec748c390469470efc2f1a33c04f0879e027f878b7407ca7f42a847b180972d918d8eb2f8d06369d9dba6c5990b845ead58e131475206ff350764eebc4139f19d154eb08a7de8cb462f8d844f04f62a2548dd3db4810adf6cecbf310567e37c024e7f0593819b76c1c762b41c022a761c2dc9908e2dc478413ca6856b2436df96fc642e16766dfe531f19153c05923e3400b0f901194368f58bbcd3484fff84c470d883fc92568d8c3a7148aa07710fcfe514c713656dd14a088affafab9dede3a944690a4829d1a68e1edc7600c383de90edd1953fbf6a3c15e84ec051099748972b8f500e802ae4863bdde60240423a915ecec3eab9510d8aca86799400d8cd04e973f80d63431aeec1566d0240e744ba43acdb16c5b7207f42709f3880c2ac29610612bc182a81a85979ad0f4f2fdbc1c473910b9c3ae4266e72f84a08ccd566fa784d6da09d1666678eba0ad949d8c7bc57a81ba1b2fcff0fc5ecdacf42a04536fbac5293e27311aa5d0203fca71abb60ed9a6d0061855a6df3eb4a99dda032c3556a9dd4acffa42757230aa24d4cae9dae3e81e1e426ff8ae861691c08e62af5c9b7fee1bb589c98f42f0cc4ce92b77c6e4e5831cc7f1ea0e25dd5bcb9d348eee806867fc80345c2e0e80ff842faf0c679f972278112e2d12a6639229df57df429a49874ce2f419a60f801a50026424729413455372aa1adf14b8c33b194e91ec303a4448c4eadf9a1070d668c71c5b8bae28fbb4a7b6ee9924a351ddbd6cef575b5a68afcb0cd218d480861a3fa8dc4075acd9efa7909da095a030e2fc03be2400ae1a4e93778c0ca7f0ba4daf0f80c4308012f209d1ae36970ac8e70a17378ae981b8f12ea534948fe056b51a3dc4e2b69090d8dac19d80b00bf9f4f0a3d895e8df59c62c9eda4919e0e9080d32848a4b390b106ae3584c552b75719bd43bbeacb6632ba0938ecf5500998beae51b9993323ab0f9757f3b0c0495f5869d3b0baefac8fc41796290df30b8890f75a1d814329e26851c510cf8e8c4f38492638426491e05c68ad08c0480faf2f0713de768f50e4d98aeb16b6b40943bf4e59f872e8e97a3ff18efae11e0593af8a09401e3afbb43256e1514001c529bfa6b6b2ef1c0822baa5ede92c2c08a0464f4330811390bfe0e52bd296225b5ab0c232078961f2d27dc834b1f61c05b06f213c0623e4d924ff7e1658680ef61263b17a16b3730b2f007f1e01a39e00c7048f8bb0ad785c8f3b0e54f0453dc47f96103d4440cda66f6c508a251907034d7c051d328006bf561133f3f13972fdfc0ba2392baf6852bc106ffc7c2d3d0f9d487e464971ca43827b60cee910a5e51c81235c3b0c8fc217ccba75698f6a02aa5fba840617f4f412e288f90f3b5a26cee7ebe25d603b8d69ac4ee98e6e927d"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "signed_txset": "...long_hex...",    "tx_hash_list": ["ff2e2d49fbfb1c9a55754f786576e171c8bf21b463a74438df604b7fa6cebc6d"]  }}
```

‌

## **submit\_transfer** <a id="submit_transfer"></a>

‌

Submit a previously signed transaction on a read-only wallet \(in cold-signing process\).‌

**Alias**: _None_.‌

**Inputs**:‌

* _tx\_data\_hex_ - string; Set of signed tx returned by "sign\_transfer"

‌

**Outputs**:‌

* _tx\_hash\_list_ - array of: string. The tx hashes of every transaction.

‌

In the example below, we submit the transfer using the signed\_txset generated above:

```text
curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"submit_transfer","params":{"tx_data_hex":"0200010200158b9c3dc885308d9024ec8302c2e5109ad803d3f801de53d21493738e1f90e701dc5e801302d8161a8105be042aa807cb560557a7d0d4898cc6fd0c0d35e77bcf839ba9b235ec3674bf00cb90f32a8b0200020b85f8bb5eae185399c32fee2ef11e3a10fed9b998280d9ef2161c70b1f353c50002417be46e074735ed3d5c7147aa472e9c14a401b9ebd58985d20e5f6eeb70e4d72101f27d6926626deaf4506c94ff24f0abe117dbd54c4f28da7a1b13047b6b2878a403fae733aa829ee9d1fc948e393f3d7368cb537e00290732c46adc46e441d2c275cdd606c19730f020ee0da02dbd90d4cd4ce7abac0413c8d51f21f7fea951a963cb33083359a00a3180d734589aec044d7d37278fb3f469fdde70156a8803df822a1e0f441426e7d933e18aeb25e58acc8e58b36d33927775501bbf1bbf32483a1ce00b21795f315cd231b11fe58e6c8407bb21d19250b91754a94606e73e12a88c7600115b5af4112e3830928dec18a552ecee1640ecdd7157f37b5fe9aeb83321c7c7010000004fda960fdb48fd706038d36af0e134ff6e770993295261af08feca3ef13394a32999b0ed41d8628b3d474a91fbec325cfef5ed11e8f3bd77e7d9a9a66e4adafc0385491182a5ef73957497f1dd3c2b680b8e63ad869efa209f43b6322770d58220ed31a3f7a9489535a75a88f9f06c5a63d1028a5b56e72e2ae152f15ac65e12e3bd1cc54f2a6bb8383c8850c29a525a30d5f99d41788fb5a7f5eef69244ec06820d2ec4507b4ca2d15970d063fc8e1edef757a647f225b865092295252ab10d07559ce962bab5b5317d9e5714b2423c7b2181fe989071f858dec30ecf1ab5e4430b8764c2a6f6190c9806563ae80090adf044d28316731f7e4e56a75dcee9e5640f27cf889413e22971496edacf434ea5dc1acee029be37f71c1531ccf5af45632f658a5406a6c0de9804fc12ccb40168f2dbddf7f086e58ac6acd79c868ef865f037cd75d072466b892df28812b4ad0c397b0dc1bc1b2ba6a924117f210851419a1f95fb63f88afdf1bdd52e1d65d010632b119bedbc2ea91ea99886bec9612047f4210c3b608edaf97ee6734a7cf22a6dde09c6a36b628f67f7058614f67444076003291b8df917842102167bbf28a9c6d074e68ecfc40ba55371355f2231b5363304836b9faf7fdeeb16f5739c05b310d434235044bf1216a242fcc6faeee198a12480283e8b438c02a3f0ff9cae741a1d4207c26799f0df255639d36ba3a0546f0e7017ca557ad680f77d5ff77ba434ae55db821c5579cf00d63185c1c477bee35e8a2cce838939b0e849182c486d8915f003f130f16d233f3f404ffac45f8436e80058e1b81ce3c3b326f57bd878d6b748914d670557353bca7f98d53fdf57782c65223aed33d0aeadc777120979ade3fb429b5a5486c967f4c2c63b5686a6a38cf329858f82b4d33be6df08419e97c1872a6c6191d5811b679d2827fb3306f9b634f7898b0207868c77e0a6033df229ec2ee28cd2c79ba01e49b67955300891e95765c53c25d3a7ceca2d4fab3944d30407797e01996d23b1ef2188bfd00662251c3f137bab02e4a831c47d722043ba4c780e5fa7d70e36968b7254b36d04ed82577fffb738086390eae9be1b800e8c19df14565ab13f4cda135220bf4a087604caa94cb26bf32ec920a2d19899b16c7d48acf285b80ebf412ed76b485f0c0b87cd272fe6fc80a82c98326397bcd408013113cbecca24ba03c36142ceb40d9d7b3ce7c9eeb71fa8c21e52cade31567c20acfa535250d1d95493c6f1e18902a5afbb5efc96a62b9d03a782536399f67a068c60d5b87144702debde82652c0f60e8298b66130176e39ef2dc754e11e498363b43a50655fc5d8a0ad07b93710d653e9ba5bda4772b2fef001990babfcaef513cb2b4f7dfc4d14cd79ca7cc6306545ac279b87a08dcedfabba270505fa6b60de12c29dd62d6bda33614e0ff6d00fd143259ae424d7a74b506b13cec4d16cd6519199862dfe2d768b6024c2a0703260e696f84a755b96272b314d2c9614c90ed4389beae781b9abed3801e047b042c5f6c1c7273737835b94c281766a7086692c339c9a18425cbc4060f1c1b9f0e3eab4bc3f891bc7c245aec18045eb0ed133acec748c390469470efc2f1a33c04f0879e027f878b7407ca7f42a847b180972d918d8eb2f8d06369d9dba6c5990b845ead58e131475206ff350764eebc4139f19d154eb08a7de8cb462f8d844f04f62a2548dd3db4810adf6cecbf310567e37c024e7f0593819b76c1c762b41c022a761c2dc9908e2dc478413ca6856b2436df96fc642e16766dfe531f19153c05923e3400b0f901194368f58bbcd3484fff84c470d883fc92568d8c3a7148aa07710fcfe514c713656dd14a088affafab9dede3a944690a4829d1a68e1edc7600c383de90edd1953fbf6a3c15e84ec051099748972b8f500e802ae4863bdde60240423a915ecec3eab9510d8aca86799400d8cd04e973f80d63431aeec1566d0240e744ba43acdb16c5b7207f42709f3880c2ac29610612bc182a81a85979ad0f4f2fdbc1c473910b9c3ae4266e72f84a08ccd566fa784d6da09d1666678eba0ad949d8c7bc57a81ba1b2fcff0fc5ecdacf42a04536fbac5293e27311aa5d0203fca71abb60ed9a6d0061855a6df3eb4a99dda032c3556a9dd4acffa42757230aa24d4cae9dae3e81e1e426ff8ae861691c08e62af5c9b7fee1bb589c98f42f0cc4ce92b77c6e4e5831cc7f1ea0e25dd5bcb9d348eee806867fc80345c2e0e80ff842faf0c679f972278112e2d12a6639229df57df429a49874ce2f419a60f801a50026424729413455372aa1adf14b8c33b194e91ec303a4448c4eadf9a1070d668c71c5b8bae28fbb4a7b6ee9924a351ddbd6cef575b5a68afcb0cd218d480861a3fa8dc4075acd9efa7909da095a030e2fc03be2400ae1a4e93778c0ca7f0ba4daf0f80c4308012f209d1ae36970ac8e70a17378ae981b8f12ea534948fe056b51a3dc4e2b69090d8dac19d80b00bf9f4f0a3d895e8df59c62c9eda4919e0e9080d32848a4b390b106ae3584c552b75719bd43bbeacb6632ba0938ecf5500998beae51b9993323ab0f9757f3b0c0495f5869d3b0baefac8fc41796290df30b8890f75a1d814329e26851c510cf8e8c4f38492638426491e05c68ad08c0480faf2f0713de768f50e4d98aeb16b6b40943bf4e59f872e8e97a3ff18efae11e0593af8a09401e3afbb43256e1514001c529bfa6b6b2ef1c0822baa5ede92c2c08a0464f4330811390bfe0e52bd296225b5ab0c232078961f2d27dc834b1f61c05b06f213c0623e4d924ff7e1658680ef61263b17a16b3730b2f007f1e01a39e00c7048f8bb0ad785c8f3b0e54f0453dc47f96103d4440cda66f6c508a251907034d7c051d328006bf561133f3f13972fdfc0ba2392baf6852bc106ffc7c2d3d0f9d487e464971ca43827b60cee910a5e51c81235c3b0c8fc217ccba75698f6a02aa5fba840617f4f412e288f90f3b5a26cee7ebe25d603b8d69ac4ee98e6e927d"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "tx_hash_list": ["f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8"]  }}
```

‌

## **sweep\_dust** <a id="sweep_dust"></a>

‌

Send all dust outputs back to the wallet's, to make them easier to spend \(and mix\).‌

**Alias**: _sweep\_unmixable_.‌

**Inputs**:‌

* _get\_tx\_keys_ - boolean; \(Optional\) Return the transaction keys after sending.
* _do\_not\_relay_ - boolean; \(Optional\) If true, the newly created transaction will not be relayed to the X-Cash network. \(Defaults to false\)
* _get\_tx\_hex_ - boolean; \(Optional\) Return the transactions as hex string after sending. \(Defaults to false\)
* _get\_tx\_metadata_ - boolean; \(Optional\) Return list of transaction metadata needed to relay the transfer later. \(Defaults to false\)

‌

**Outputs**:‌

* _tx\_hash\_list_ - array of: string. The tx hashes of every transaction.
* _tx\_key\_list_ - array of: string. The transaction keys for every transaction.
* _amount\_list_ - array of: integer. The amount transferred for every transaction.
* _fee\_list_ - array of: integer. The amount of fees paid for every transaction.
* _tx\_blob\_list_ - array of: string. The tx as hex string for every transaction.
* _tx\_metadata\_list_ - array of: string. List of transaction metadata needed to relay the transactions later.
* _multisig\_txset_ - string. The set of signing keys used in a multisig transaction \(empty for non-multisig\).
* _unsigned\_txset_ - string. Set of unsigned tx for cold-signing purposes.

‌

**Example** \(In this example, `sweep_dust` returns nothing because there are no funds to sweep\):

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sweep_dust","params":{"get_tx_keys":true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "multisig_txset": "",    "unsigned_txset": ""  }}
```

‌

## **sweep\_all** <a id="sweep_all"></a>

‌

Send all unlocked balance to an address.‌

**Alias**: _None_.‌

**Inputs**:‌

* _address_ - string; Destination public address.
* _account\_index_ - unsigned int; Sweep transactions from this account.
* _subaddr\_indices_ - array of unsigned int; \(Optional\) Sweep from this set of subaddresses in the account.
* _priority_ - unsigned int; \(Optional\) Priority for sending the sweep transfer, partially determines fee.
* _mixin_ - unsigned int; Number of outputs from the blockchain to mix with \(0 means no mixing\).
* _ring\_size_ - unsigned int; Sets ringsize to n \(mixin + 1\).
* _unlock\_time_ - unsigned int; Number of blocks before the XCASH can be spent \(0 to not add a lock\).
* _payment\_id_ - string; \(Optional\) Random 32-byte/64-character hex string to identify a transaction.
* _get\_tx\_keys_ - boolean; \(Optional\) Return the transaction keys after sending.
* _below\_amount_ - unsigned int; \(Optional\) Include outputs below this amount.
* _do\_not\_relay_ - boolean; \(Optional\) If true, do not relay this sweep transfer. \(Defaults to false\)
* _get\_tx\_hex_ - boolean; \(Optional\) return the transactions as hex encoded string. \(Defaults to false\)
* _get\_tx\_metadata_ - boolean; \(Optional\) return the transaction metadata as a string. \(Defaults to false\)

‌

**Outputs**:‌

* _tx\_hash\_list_ - array of: string. The tx hashes of every transaction.
* _tx\_key\_list_ - array of: string. The transaction keys for every transaction.
* _amount\_list_ - array of: integer. The amount transferred for every transaction.
* _fee\_list_ - array of: integer. The amount of fees paid for every transaction.
* _tx\_blob\_list_ - array of: string. The tx as hex string for every transaction.
* _tx\_metadata\_list_ - array of: string. List of transaction metadata needed to relay the transactions later.
* _multisig\_txset_ - string. The set of signing keys used in a multisig transaction \(empty for non-multisig\).
* _unsigned\_txset_ - string. Set of unsigned tx for cold-signing purposes.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sweep_all","params":{"address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe","ring_size":21}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "amount_list": [100000000000],    "fee_list": [1348040],    "multisig_txset": "",    "tx_hash_list": ["cd97adf7372e620cda46ed623f89fca1411873f5d7251359ab468c287d8f9fee"],    "tx_key_list": ["8037f31fd7b9c76d432e6c0c14b549ea68fdd84537f75c6e9a066890f7840d0b"],    "unsigned_txset": ""  }}
```

‌

## **sweep\_single** <a id="sweep_single"></a>

‌

Send all of a specific unlocked output to an address.‌

**Alias**: _None_.‌

**Inputs**:‌

* _address_ - string; Destination public address.
* _account\_index_ - unsigned int; Sweep transactions from this account.
* _subaddr\_indices_ - array of unsigned int; \(Optional\) Sweep from this set of subaddresses in the account.
* _priority_ - unsigned int; \(Optional\) Priority for sending the sweep transfer, partially determines fee.
* _mixin_ - unsigned int; Number of outputs from the blockchain to mix with \(0 means no mixing\).
* _ring\_size_ - unsigned int; Sets ringsize to n \(mixin + 1\).
* _unlock\_time_ - unsigned int; Number of blocks before the XCASH can be spent \(0 to not add a lock\).
* _payment\_id_ - string; \(Optional\) Random 32-byte/64-character hex string to identify a transaction.
* _get\_tx\_keys_ - boolean; \(Optional\) Return the transaction keys after sending.
* _key\_image_ - string; Key image of specific output to sweep.
* _below\_amount_ - unsigned int; \(Optional\) Include outputs below this amount.
* _do\_not\_relay_ - boolean; \(Optional\) If true, do not relay this sweep transfer. \(Defaults to false\)
* _get\_tx\_hex_ - boolean; \(Optional\) return the transactions as hex encoded string. \(Defaults to false\)
* _get\_tx\_metadata_ - boolean; \(Optional\) return the transaction metadata as a string. \(Defaults to false\)

‌

**Outputs**:‌

* _tx\_hash\_list_ - array of: string. The tx hashes of every transaction.
* _tx\_key\_list_ - array of: string. The transaction keys for every transaction.
* _amount\_list_ - array of: integer. The amount transferred for every transaction.
* _fee\_list_ - array of: integer. The amount of fees paid for every transaction.
* _tx\_blob\_list_ - array of: string. The tx as hex string for every transaction.
* _tx\_metadata\_list_ - array of: string. List of transaction metadata needed to relay the transactions later.
* _multisig\_txset_ - string. The set of signing keys used in a multisig transaction \(empty for non-multisig\).
* _unsigned\_txset_ - string. Set of unsigned tx for cold-signing purposes.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sweep_single","params":{"destinations":[{"amount":100000000000,"address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe"}],"ring_size":21,"get_tx_keys": true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "amount": 100000000000,    "fee": 1348040,    "multisig_txset": "",    "tx_hash": "cd97adf7372e620cda46ed623f89fca1411873f5d7251359ab468c287d8f9fee",    "tx_key": "8037f31fd7b9c76d432e6c0c14b549ea68fdd84537f75c6e9a066890f7840d0b",    "unsigned_txset": ""  }}
```

‌

## **relay\_tx** <a id="relay_tx"></a>

‌

Relay a transaction previously created with `"do_not_relay":true`.‌

**Alias**: _None_.‌

**Inputs**:‌

* _hex_ - string; transaction metadata returned from a `transfer` method with `get_tx_metadata` set to `true`.

‌

**Outputs**:‌

* _tx\_hash_ - String for the publically searchable transaction hash.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"relay_tx","params":{"hex":"0200010200158b9c3dc885308d9024ec8302c2e5109ad803d3f801de53d21493738e1f90e701dc5e801302d8161a8105be042aa807cb560557a7d0d4898cc6fd0c0d35e77bcf839ba9b235ec3674bf00cb90f32a8b0200020b85f8bb5eae185399c32fee2ef11e3a10fed9b998280d9ef2161c70b1f353c50002417be46e074735ed3d5c7147aa472e9c14a401b9ebd58985d20e5f6eeb70e4d72101f27d6926626deaf4506c94ff24f0abe117dbd54c4f28da7a1b13047b6b2878a403fae733aa829ee9d1fc948e393f3d7368cb537e00290732c46adc46e441d2c275cdd606c19730f020ee0da02dbd90d4cd4ce7abac0413c8d51f21f7fea951a963cb33083359a00a3180d734589aec044d7d37278fb3f469fdde70156a8803df822a1e0f441426e7d933e18aeb25e58acc8e58b36d33927775501bbf1bbf32483a1ce00b21795f315cd231b11fe58e6c8407bb21d19250b91754a94606e73e12a88c7600115b5af4112e3830928dec18a552ecee1640ecdd7157f37b5fe9aeb83321c7c7010000004fda960fdb48fd706038d36af0e134ff6e770993295261af08feca3ef13394a32999b0ed41d8628b3d474a91fbec325cfef5ed11e8f3bd77e7d9a9a66e4adafc0385491182a5ef73957497f1dd3c2b680b8e63ad869efa209f43b6322770d58220ed31a3f7a9489535a75a88f9f06c5a63d1028a5b56e72e2ae152f15ac65e12e3bd1cc54f2a6bb8383c8850c29a525a30d5f99d41788fb5a7f5eef69244ec06820d2ec4507b4ca2d15970d063fc8e1edef757a647f225b865092295252ab10d07559ce962bab5b5317d9e5714b2423c7b2181fe989071f858dec30ecf1ab5e4430b8764c2a6f6190c9806563ae80090adf044d28316731f7e4e56a75dcee9e5640f27cf889413e22971496edacf434ea5dc1acee029be37f71c1531ccf5af45632f658a5406a6c0de9804fc12ccb40168f2dbddf7f086e58ac6acd79c868ef865f037cd75d072466b892df28812b4ad0c397b0dc1bc1b2ba6a924117f210851419a1f95fb63f88afdf1bdd52e1d65d010632b119bedbc2ea91ea99886bec9612047f4210c3b608edaf97ee6734a7cf22a6dde09c6a36b628f67f7058614f67444076003291b8df917842102167bbf28a9c6d074e68ecfc40ba55371355f2231b5363304836b9faf7fdeeb16f5739c05b310d434235044bf1216a242fcc6faeee198a12480283e8b438c02a3f0ff9cae741a1d4207c26799f0df255639d36ba3a0546f0e7017ca557ad680f77d5ff77ba434ae55db821c5579cf00d63185c1c477bee35e8a2cce838939b0e849182c486d8915f003f130f16d233f3f404ffac45f8436e80058e1b81ce3c3b326f57bd878d6b748914d670557353bca7f98d53fdf57782c65223aed33d0aeadc777120979ade3fb429b5a5486c967f4c2c63b5686a6a38cf329858f82b4d33be6df08419e97c1872a6c6191d5811b679d2827fb3306f9b634f7898b0207868c77e0a6033df229ec2ee28cd2c79ba01e49b67955300891e95765c53c25d3a7ceca2d4fab3944d30407797e01996d23b1ef2188bfd00662251c3f137bab02e4a831c47d722043ba4c780e5fa7d70e36968b7254b36d04ed82577fffb738086390eae9be1b800e8c19df14565ab13f4cda135220bf4a087604caa94cb26bf32ec920a2d19899b16c7d48acf285b80ebf412ed76b485f0c0b87cd272fe6fc80a82c98326397bcd408013113cbecca24ba03c36142ceb40d9d7b3ce7c9eeb71fa8c21e52cade31567c20acfa535250d1d95493c6f1e18902a5afbb5efc96a62b9d03a782536399f67a068c60d5b87144702debde82652c0f60e8298b66130176e39ef2dc754e11e498363b43a50655fc5d8a0ad07b93710d653e9ba5bda4772b2fef001990babfcaef513cb2b4f7dfc4d14cd79ca7cc6306545ac279b87a08dcedfabba270505fa6b60de12c29dd62d6bda33614e0ff6d00fd143259ae424d7a74b506b13cec4d16cd6519199862dfe2d768b6024c2a0703260e696f84a755b96272b314d2c9614c90ed4389beae781b9abed3801e047b042c5f6c1c7273737835b94c281766a7086692c339c9a18425cbc4060f1c1b9f0e3eab4bc3f891bc7c245aec18045eb0ed133acec748c390469470efc2f1a33c04f0879e027f878b7407ca7f42a847b180972d918d8eb2f8d06369d9dba6c5990b845ead58e131475206ff350764eebc4139f19d154eb08a7de8cb462f8d844f04f62a2548dd3db4810adf6cecbf310567e37c024e7f0593819b76c1c762b41c022a761c2dc9908e2dc478413ca6856b2436df96fc642e16766dfe531f19153c05923e3400b0f901194368f58bbcd3484fff84c470d883fc92568d8c3a7148aa07710fcfe514c713656dd14a088affafab9dede3a944690a4829d1a68e1edc7600c383de90edd1953fbf6a3c15e84ec051099748972b8f500e802ae4863bdde60240423a915ecec3eab9510d8aca86799400d8cd04e973f80d63431aeec1566d0240e744ba43acdb16c5b7207f42709f3880c2ac29610612bc182a81a85979ad0f4f2fdbc1c473910b9c3ae4266e72f84a08ccd566fa784d6da09d1666678eba0ad949d8c7bc57a81ba1b2fcff0fc5ecdacf42a04536fbac5293e27311aa5d0203fca71abb60ed9a6d0061855a6df3eb4a99dda032c3556a9dd4acffa42757230aa24d4cae9dae3e81e1e426ff8ae861691c08e62af5c9b7fee1bb589c98f42f0cc4ce92b77c6e4e5831cc7f1ea0e25dd5bcb9d348eee806867fc80345c2e0e80ff842faf0c679f972278112e2d12a6639229df57df429a49874ce2f419a60f801a50026424729413455372aa1adf14b8c33b194e91ec303a4448c4eadf9a1070d668c71c5b8bae28fbb4a7b6ee9924a351ddbd6cef575b5a68afcb0cd218d480861a3fa8dc4075acd9efa7909da095a030e2fc03be2400ae1a4e93778c0ca7f0ba4daf0f80c4308012f209d1ae36970ac8e70a17378ae981b8f12ea534948fe056b51a3dc4e2b69090d8dac19d80b00bf9f4f0a3d895e8df59c62c9eda4919e0e9080d32848a4b390b106ae3584c552b75719bd43bbeacb6632ba0938ecf5500998beae51b9993323ab0f9757f3b0c0495f5869d3b0baefac8fc41796290df30b8890f75a1d814329e26851c510cf8e8c4f38492638426491e05c68ad08c0480faf2f0713de768f50e4d98aeb16b6b40943bf4e59f872e8e97a3ff18efae11e0593af8a09401e3afbb43256e1514001c529bfa6b6b2ef1c0822baa5ede92c2c08a0464f4330811390bfe0e52bd296225b5ab0c232078961f2d27dc834b1f61c05b06f213c0623e4d924ff7e1658680ef61263b17a16b3730b2f007f1e01a39e00c7048f8bb0ad785c8f3b0e54f0453dc47f96103d4440cda66f6c508a251907034d7c051d328006bf561133f3f13972fdfc0ba2392baf6852bc106ffc7c2d3d0f9d487e464971ca43827b60cee910a5e51c81235c3b0c8fc217ccba75698f6a02aa5fba840617f4f412e288f90f3b5a26cee7ebe25d603b8d69ac4ee98e6e927d"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "tx_hash": "f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8"  }}
```

‌

## **store** <a id="store"></a>

‌

Save the wallet file.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"store"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **get\_payments** <a id="get_payments"></a>

‌

Get a list of incoming payments using a given payment id.‌

**Alias**: _None_.‌

**Inputs**:‌

* _payment\_id_ - string; Payment ID used to find the payments \(16 characters hex\).

‌

**Outputs**:‌

* _payments_ - list of:
  * _payment\_id_ - string; Payment ID matching the input parameter.
  * _tx\_hash_ - string; Transaction hash used as the transaction ID.
  * _amount_ - unsigned int; Amount for this payment.
  * _block\_height_ - unsigned int; Height of the block that first confirmed this payment.
  * _unlock\_time_ - unsigned int; Time \(in block height\) until this payment is safe to spend.
  * _subaddr\_index_ - subaddress index:
    * _major_ - unsigned int; Account index for the subaddress.
    * _minor_ - unsigned int; Index of the subaddress in the account.
  * _address_ - string; Address receiving the payment; Base58 representation of the public keys.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_payments","params":{"payment_id":"0000000000000000000000000000000000000000000000000000000000000000"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "payments": [{      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "amount": 12000000000,      "block_height": 358092,      "payment_id": "0000000000000000000000000000000000000000000000000000000000000000",      "subaddr_index": {        "major": 0,        "minor": 0      },    }]  }}
```

‌

## **get\_bulk\_payments** <a id="get_bulk_payments"></a>

‌

Get a list of incoming payments using a given payment id, or a list of payments ids, from a given height. This method is the preferred method over `get_payments`because it has the same functionality but is more extendable. Either is fine for looking up transactions by a single payment ID.‌

**Alias**: _None_.‌

**Inputs**:‌

* _payment\_ids_ - array of: string; Payment IDs used to find the payments \(16 characters hex\).
* _min\_block\_height_ - unsigned int; The block height at which to start looking for payments.

‌

**Outputs**:‌

* _payments_ - list of:
  * _payment\_id_ - string; Payment ID matching one of the input IDs.
  * _tx\_hash_ - string; Transaction hash used as the transaction ID.
  * _amount_ - unsigned int; Amount for this payment.
  * _block\_height_ - unsigned int; Height of the block that first confirmed this payment.
  * _unlock\_time_ - unsigned int; Time \(in block height\) until this payment is safe to spend.
  * _subaddr\_index_ - subaddress index:
    * _major_ - unsigned int; Account index for the subaddress.
    * _minor_ - unsigned int; Index of the subaddress in the account.
  * _address_ - string; Address receiving the payment; Base58 representation of the public keys.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_bulk_payments","params":{"payment_ids":["0000000000000000000000000000000000000000000000000000000000000000"],"min_block_height":"120000"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "payments": [{      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "amount": 12000000000,      "block_height": 358092,      "payment_id": "0000000000000000000000000000000000000000000000000000000000000000",      "subaddr_index": {        "major": 0,        "minor": 0      },    }]  }}
```

‌

## **incoming\_transfers** <a id="incoming_transfers"></a>

‌

Return a list of incoming transfers to the wallet.‌

**Inputs**:‌

* _transfer\_type_ - string; "all": all the transfers, "available": only transfers which are not yet spent, OR "unavailable": only transfers which are already spent.
* _account\_index_ - unsigned int; \(Optional\) Return transfers for this account. \(defaults to 0\)
* _subaddr\_indices_ - array of unsigned int; \(Optional\) Return transfers sent to these subaddresses.
* _verbose_ - boolean; \(Optional\) Enable verbose output, return key image if true.

‌

**Outputs**:‌

* _transfers_ - list of:
  * _amount_ - unsigned int; Amount of this transfer.
  * _global\_index_ - unsigned int; Mostly internal use, can be ignored by most users.
  * _key\_image_ - string; Key image for the incoming transfer's unspent output \(empty unless verbose is true\).
  * _spent_ - boolean; Indicates if this transfer has been spent.
  * _subaddr\_index_ - unsigned int; Subaddress index for incoming transfer.
  * _tx\_hash_ - string; Several incoming transfers may share the same hash if they were in the same transaction.
  * _tx\_size_ - unsigned int; Size of transaction in bytes.

‌

**Example**, get all transfers:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"incoming_transfers","params":{"transfer_type":"all"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "transfers": [{      "amount": 362070234101,      "global_index": 2864114,      "key_image": "1fbc053e189764f562afcb27299f4b48c8c8658d08dd528abe2e0826ee5e91d7",      "spent": false,      "subaddr_index": {        "major": 0,        "minor": 0      },      "tx_hash": "f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8"    }]  }}
```

‌

**Example**, get available transfers:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"incoming_transfers","params":{"transfer_type":"available","account_index":0,"subaddr_indices":[3],"verbose":true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "transfers": [{      "amount": 27126892247503,      "global_index": 594994,      "key_image": "7e561394806afd1be61980cc3431f6ef3569fa9151cd8d234f8ec13aa145695e",      "spent": false,      "subaddr_index": 3,      "tx_hash": "106d4391a031e5b735ded555862fec63233e34e5fa4fc7edcfdbe461c275ae5b",      "tx_size": 157    },{      "amount": 27169374733655,      "global_index": 594997,      "key_image": "e76c0a3bfeaae35e4173712f782eb34011198e26b990225b71aa787c8ba8a157",      "spent": false,      "subaddr_index": 3,      "tx_hash": "0bd959b59117ee1254bd8e5aa8e77ec04ef744144a1ffb2d5c1eb9380a719621",      "tx_size": 158    }]  }}
```

‌

**Example**, get unavailable transfers:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"incoming_transfers","params":{"transfer_type":"unavailable","account_index":0,"subaddr_indices":[3],"verbose":true}}' -H 'Content-Type: application/json'{"id": "0","jsonrpc": "2.0","result": {  "transfers": [{    "amount": 60000000000000,    "global_index": 122405,    "key_image": "768f5144777eb23477ab7acf83562581d690abaf98ca897c03a9d2b900eb479b",    "spent": true,    "subaddr_index": 3,    "tx_hash": "f53401f21c6a43e44d5dd7a90eba5cf580012ad0e15d050059136f8a0da34f6b",    "tx_size": 159  }]}}
```

‌

## **query\_key** <a id="query_key"></a>

‌

Return the spend or view private key.‌

**Alias**: _None_.‌

**Inputs**:‌

* _key\_type_ - string; Which key to retrieve: "mnemonic" - the mnemonic seed \(older wallets do not have one\) OR "view\_key" - the view key

‌

**Outputs**:‌

* _key_ - string; The view key will be hex encoded, while the mnemonic will be a string of words.

‌

**Example** \(Query view key\):

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"query_key","params":{"key_type":"view_key"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "key": "93d7bf52dfea9da0b35b7da923bd1ca9d4db4cf5c925c261ba0b6274c3fa9208"  }}
```

‌

**Example** \(Query mnemonic key\):

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"query_key","params":{"key_type":"mnemonic"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "key": "arena eight cool bugs sieve bovine feline pledge affair aptitude yeti gnaw gadget saved cunning sadness apricot pruned foxes gags eagle pelican hippo hunter gags"  }}
```

‌

## **make\_integrated\_address** <a id="make_integrated_address"></a>

‌

Make an integrated address from the wallet address and a payment id.‌

**Alias**: _None_.‌

**Inputs**:‌

* _standard\_address_ - string; \(Optional, defaults to primary address\) Destination public address.
* _payment\_id_ - string; \(Optional, defaults to a random ID\) 16 characters hex encoded.

‌

**Outputs**:‌

* _integrated\_address_ - string
* _payment\_id_ - string; hex encoded;

‌

**Example** \(Payment ID is empty, use a random ID\):

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"make_integrated_address"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "integrated_address": "XCBzxZ866QF438An6FUXHM1RGetpTT4Ek9kaA9afPTXaetKq9wsacaEc3rb5TM85kJBn1qv2NxPw12Mr24uB2cDwBt6ZwDMxS9rfRf6W4oTcwC",    "payment_id": "f8784e5de5ba9075"  }}
```

‌

## **split\_integrated\_address** <a id="split_integrated_address"></a>

‌

Retrieve the standard address and payment id corresponding to an integrated address.‌

**Alias**: _None_.‌

**Inputs**:‌

* _integrated\_address_ - string

‌

**Outputs**:‌

* _is\_subaddress_ - boolean; States if the address is a subaddress
* _payment_ - string; hex encoded
* _standard\_address_ - string

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"split_integrated_address","params":{"integrated_address": "XCBzxZ866QF438An6FUXHM1RGetpTT4Ek9kaA9afPTXaetKq9wsacaEc3rb5TM85kJBn1qv2NxPw12Mr24uB2cDwBt6ZwDMxS9rfRf6W4oTcwC"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "is_subaddress": false,    "payment_id": "f8784e5de5ba9075",    "standard_address": "XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R"  }}
```

‌

## **stop\_wallet** <a id="stop_wallet"></a>

‌

Stops the wallet, storing the current state.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"stop_wallet"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **rescan\_blockchain** <a id="rescan_blockchain"></a>

‌

Rescan the blockchain from scratch, losing any information which can not be recovered from the blockchain itself. This includes destination addresses, tx secret keys, tx notes, etc.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"rescan_blockchain"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **set\_tx\_notes** <a id="set_tx_notes"></a>

‌

Set arbitrary string notes for transactions.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txids_ - array of string; transaction ids
* _notes_ - array of string; notes for the transactions

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_tx_notes","params":{"txids":["f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8"],"notes":["This is an example"]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **get\_tx\_notes** <a id="get_tx_notes"></a>

‌

Get string notes for transactions.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txids_ - array of string; transaction ids

‌

**Outputs**:‌

* _notes_ - array of string; notes for the transactions

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_tx_notes","params":{"txids":["f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8"]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "notes": ["This is an example"]  }}
```

‌

## **set\_attribute** <a id="set_attribute"></a>

‌

Set arbitrary attribute.‌

**Alias**: _None_.‌

**Inputs**:‌

* _key_ - string; attribute name
* _value_ - string; attribute value

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_attribute","params":{"key":"my_attribute","value":"my_value"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **get\_attribute** <a id="get_attribute"></a>

‌

Get attribute value by name.‌

**Alias**: _None_.‌

**Inputs**:‌

* _key_ - string; attribute name

‌

**Outputs**:‌

* _value_ - string; attribute value

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_attribute","params":{"key":"my_attribute"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "value": "my_value"  }}
```

‌

## **get\_tx\_key** <a id="get_tx_key"></a>

‌

Get transaction secret key from transaction id.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; transaction id.

‌

**Outputs**:‌

* _tx\_key_ - string; transaction secret key.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_tx_key","params":{"txid":"f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "tx_key": "81ba662cf8fb6d0d0da18fc9b70ab28e01cc76311278fdd7fe7ab16360762b06"  }}
```

‌

## **check\_tx\_key** <a id="check_tx_key"></a>

‌

Check a transaction in the blockchain with its secret key.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; transaction id.
* _tx\_key_ - string; transaction secret key.
* _address_ - string; destination public address of the transaction.

‌

**Outputs**:‌

* _confirmations_ - unsigned int; Number of block mined after the one with the transaction.
* _in\_pool_ - boolean; States if the transaction is still in pool or has been added to a block.
* _received_ - unsigned int; Amount of the transaction.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"check_tx_key","params":{"txid":"f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8","tx_key":"81ba662cf8fb6d0d0da18fc9b70ab28e01cc76311278fdd7fe7ab16360762b06","address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "confirmations": 0,    "in_pool": false,    "received": 100000000000  }}
```

‌

## **get\_tx\_proof** <a id="get_tx_proof"></a>

‌

Get transaction signature to prove it.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; transaction id.
* _address_ - string; destination public address of the transaction.
* _message_ - string; \(Optional\) add a message to the signature to further authenticate the prooving process.

‌

**Outputs**:‌

* _signature_ - string; transaction signature.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_tx_proof","params":{"txid":"f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8","address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe","message":"this is my transaction"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "signature": "InProofV13vqBCT6dpSAXkypZmSEMPGVnNRFDX2vscUYeVS4WnSVnV5BwLs31T9q6Etfj9Wts6tAxSAS4gkMeSYzzLS7Gt4vvCSQRh9niGJMUDJsB5hTzb2XJiCkUzWkkcjLFBBRVD5QZ"  }}
```

‌

## **check\_tx\_proof** <a id="check_tx_proof"></a>

‌

Prove a transaction by checking its signature.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; transaction id.
* _address_ - string; destination public address of the transaction.
* _message_ - string; \(Optional\) Should be the same message used in `get_tx_proof`.
* _signature_ - string; transaction signature to confirm.

‌

**Outputs**:‌

* _confirmations_ - unsigned int; Number of block mined after the one with the transaction.
* _good_ - boolean; States if the inputs proves the transaction.
* _in\_pool_ - boolean; States if the transaction is still in pool or has been added to a block.
* _received_ - unsigned int; Amount of the transaction.

‌

In the example below, the transaction has been proven:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"check_tx_proof","params":{"txid":"f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8","address":"XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe","message":"this is my transaction","signature":"InProofV13vqBCT6dpSAXkypZmSEMPGVnNRFDX2vscUYeVS4WnSVnV5BwLs31T9q6Etfj9Wts6tAxSAS4gkMeSYzzLS7Gt4vvCSQRh9niGJMUDJsB5hTzb2XJiCkUzWkkcjLFBBRVD5QZ"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "confirmations": 482,    "good": true,    "in_pool": false,    "received": 1000000000000  }}
```

‌

## **get\_spend\_proof** <a id="get_spend_proof"></a>

‌

Generate a signature to prove a spend. Unlike proving a transaction, it does not requires the destination public address.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; transaction id.
* _message_ - string; \(Optional\) add a message to the signature to further authenticate the prooving process.

‌

**Outputs**:‌

* _signature_ - string; spend signature.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_spend_proof","params":{"txid":"f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8","message":"this is my transaction"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "signature": "SpendProofV1aSh8Todhk54736iXgV6vJAFP7egxByuMWZeyNDaN2JY737S95X5zz5mNMQSuCNSLjjhi5HJCsndpNWSNVsuThxwv285qy1KkUrLFRkxMSCjfL6bbycYN33ScZ5UB4Fzseceo1ndpL393T1q638VmcU3a56dhNHF1RPZFiGPS61FA78nXFSqE9uoKCCoHkEz83M1dQVhxZV5CEPF2P6VioGTKgprLCH9vvj9k1ivd4SX19L2VSMc3zD1u3mkR24ioETvxBoLeBSpxMoikyZ6inhuPm8yYo9YWyFtQK4XYfAV9mJ9knz5fUPXR8vvh7KJCAg4dqeJXTVb4mbMzYtsSZXHd6ouWoyCd6qMALdW8pKhgMCHcVYMWp9X9WHZuCo9rsRjRpg15sJUw7oJg1JoGiVgj8P4JeGDjnZHnmLVa5bpJhVCbMhyM7JLXNQJzFWTGC27TQBbthxCfQaKdusYnvZnKPDJWSeceYEFzepUnsWhQtyhbb73FzqgWC4eKEFKAZJqT2LuuSoxmihJ9acnFK7Ze23KTVYgDyMKY61VXADxmSrBvwUtxCaW4nQtnbMxiPMNnDMzeixqsFMBtN72j5UqhiLRY99k6SE7Qf5f29haNSBNSXCFFHChPKNTwJrehkofBdKUhh2VGPqZDNoefWUwfudeu83t85bmjv8Q3LrQSkFgFjRT5tLo8TMawNXoZCrQpyZrEvnodMDDUUNf3NL7rxyv3gM1KrTWjYaWXFU2RAsFee2Q2MTwUW7hR25cJvSFuB1BX2bfkoCbiMk923tHZGU2g7rSKF1GDDkXAc1EvFFD4iGbh1Q5t6hPRhBV8PEncdcCWGq5uAL5D4Bjr6VXG8uNeCy5oYWNgbZ5JRSfm7QEhPv8Fy9AKMgmCxDGMF9dVEaU6tw2BAnJavQdfrxChbDBeQXzCbCfep6oei6n2LZdE5Q84wp7eoQFE5Cwuo23tHkbJCaw2njFi3WGBbA7uGZaGHJPyB2rofTWBiSUXZnP2hiE9bjJghAcDm1M4LVLfWvhZmFEnyeru3VWMETnetz1BYLUC5MJGFXuhnHwWh7F6r74FDyhdswYop4eWPbyrXMXmUQEccTGd2NaT8g2VHADZ76gMC6BjWESvcnz2D4n8XwdmM7ZQ1jFwhuXrBfrb1dwRasyXxxHMGAC2onatNiExyeQ9G1W5LwqNLAh9hvcaNTGaYKYXoceVzLkgm6e5WMkLsCwuZXvB"  }}
```

‌

## **check\_spend\_proof** <a id="check_spend_proof"></a>

‌

Prove a spend using a signature. Unlike proving a transaction, it does not requires the destination public address.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; transaction id.
* _message_ - string; \(Optional\) Should be the same message used in `get_spend_proof`.
* _signature_ - string; spend signature to confirm.

‌

**Outputs**:‌

* _good_ - boolean; States if the inputs proves the spend.

‌

In the example below, the spend has been proven:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"check_spend_proof","params":{"txid":"f00426a332ab8eea7b0d8d219fd955b87debb49b1ff4d18d831bbe25426152e8","message":"this is my transaction","signature":"SpendProofV1aSh8Todhk54736iXgV6vJAFP7egxByuMWZeyNDaN2JY737S95X5zz5mNMQSuCNSLjjhi5HJCsndpNWSNVsuThxwv285qy1KkUrLFRkxMSCjfL6bbycYN33ScZ5UB4Fzseceo1ndpL393T1q638VmcU3a56dhNHF1RPZFiGPS61FA78nXFSqE9uoKCCoHkEz83M1dQVhxZV5CEPF2P6VioGTKgprLCH9vvj9k1ivd4SX19L2VSMc3zD1u3mkR24ioETvxBoLeBSpxMoikyZ6inhuPm8yYo9YWyFtQK4XYfAV9mJ9knz5fUPXR8vvh7KJCAg4dqeJXTVb4mbMzYtsSZXHd6ouWoyCd6qMALdW8pKhgMCHcVYMWp9X9WHZuCo9rsRjRpg15sJUw7oJg1JoGiVgj8P4JeGDjnZHnmLVa5bpJhVCbMhyM7JLXNQJzFWTGC27TQBbthxCfQaKdusYnvZnKPDJWSeceYEFzepUnsWhQtyhbb73FzqgWC4eKEFKAZJqT2LuuSoxmihJ9acnFK7Ze23KTVYgDyMKY61VXADxmSrBvwUtxCaW4nQtnbMxiPMNnDMzeixqsFMBtN72j5UqhiLRY99k6SE7Qf5f29haNSBNSXCFFHChPKNTwJrehkofBdKUhh2VGPqZDNoefWUwfudeu83t85bmjv8Q3LrQSkFgFjRT5tLo8TMawNXoZCrQpyZrEvnodMDDUUNf3NL7rxyv3gM1KrTWjYaWXFU2RAsFee2Q2MTwUW7hR25cJvSFuB1BX2bfkoCbiMk923tHZGU2g7rSKF1GDDkXAc1EvFFD4iGbh1Q5t6hPRhBV8PEncdcCWGq5uAL5D4Bjr6VXG8uNeCy5oYWNgbZ5JRSfm7QEhPv8Fy9AKMgmCxDGMF9dVEaU6tw2BAnJavQdfrxChbDBeQXzCbCfep6oei6n2LZdE5Q84wp7eoQFE5Cwuo23tHkbJCaw2njFi3WGBbA7uGZaGHJPyB2rofTWBiSUXZnP2hiE9bjJghAcDm1M4LVLfWvhZmFEnyeru3VWMETnetz1BYLUC5MJGFXuhnHwWh7F6r74FDyhdswYop4eWPbyrXMXmUQEccTGd2NaT8g2VHADZ76gMC6BjWESvcnz2D4n8XwdmM7ZQ1jFwhuXrBfrb1dwRasyXxxHMGAC2onatNiExyeQ9G1W5LwqNLAh9hvcaNTGaYKYXoceVzLkgm6e5WMkLsCwuZXvB"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "good": true  }}
```

‌

## **get\_reserve\_proof** <a id="get_reserve_proof"></a>

‌

Generate a signature to prove of an available amount in a wallet.‌

**Alias**: _None_.‌

**Inputs**:‌

* _all_ - boolean; Proves all wallet balance to be disposable.
* _account\_index_ - unsigned int; Specify the account from witch to prove reserve. \(ignored if `all` is set to true\)
* _amount_ - unsigned int; Amount \(in [atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html)\) to prove the account has for reserve. \(ignored if `all` is set to true\)
* _message_ - string; \(Optional\) add a message to the signature to further authenticate the prooving process.

‌

**Outputs**:‌

* _signature_ - string; reserve signature.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_reserve_proof","params":{"all":true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "signature": "ReserveProofV11BZ23sBt9sZJeGccf84mzyAmNCP3KzYbE1111112VKmH111118NwZ1xM1HTAd84ePcZSqFX6xBVieiDSoxYpCckL4DReogi9MTV113UXUtguEFV1zrU82oHgTV2eBjikYMnJEowkxibHPkk18K4Gye8tcr4u8ZBoTvdAV347nXpxxGjmUVvN7f9jnthihEETtjHWgP113kpYmq9tnad9iWuq79GzfaYSnCJwTSELA7ZYpovkgUTCGimj7eXR5JcC7tdrnFNT2yZyrvwdkhhU5f1QhYASkT8bEUjmm8FWMEfzPzq5A5pFFMdv8NQNYap7HZnQRCkxvEZUByvoDUUVoWSf9Fjnrs8tCyXh28yo3cjf8gg1SNtPXo6kohKwxNgaL1Ak9UAcoRJR7dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSV6nnejGkeMJFg8VicB8CCAQDJ2rkUXiKwJsBuutypLEBu6frBYGmAVRSFt224VKfJfnVbx8M6k6Xq6Yu4pmcBJ4gZCDy"  }}
```

‌

## **check\_reserve\_proof** <a id="check_reserve_proof"></a>

‌

Proves a wallet has a disposable reserve using a signature.‌

**Alias**: _None_.‌

**Inputs**:‌

* _address_ - string; Public address of the wallet.
* _message_ - string; \(Optional\) Should be the same message used in `get_reserve_proof`.
* _signature_ - string; reserve signature to confirm.

‌

**Outputs**:‌

* _good_ - boolean; States if the inputs proves the reserve.

‌

In the example below, the reserve has been proven:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"check_reserve_proof","params":{"address":"XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R","signature":"ReserveProofV11BZ23sBt9sZJeGccf84mzyAmNCP3KzYbE1111112VKmH111118NwZ1xM1HTAd84ePcZSqFX6xBVieiDSoxYpCckL4DReogi9MTV113UXUtguEFV1zrU82oHgTV2eBjikYMnJEowkxibHPkk18K4Gye8tcr4u8ZBoTvdAV347nXpxxGjmUVvN7f9jnthihEETtjHWgP113kpYmq9tnad9iWuq79GzfaYSnCJwTSELA7ZYpovkgUTCGimj7eXR5JcC7tdrnFNT2yZyrvwdkhhU5f1QhYASkT8bEUjmm8FWMEfzPzq5A5pFFMdv8NQNYap7HZnQRCkxvEZUByvoDUUVoWSf9Fjnrs8tCyXh28yo3cjf8gg1SNtPXo6kohKwxNgaL1Ak9UAcoRJR7dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSV6nnejGkeMJFg8VicB8CCAQDJ2rkUXiKwJsBuutypLEBu6frBYGmAVRSFt224VKfJfnVbx8M6k6Xq6Yu4pmcBJ4gZCDy"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "good": true,    "spent": 0,    "total": 1000000000  }}
```

‌

## **get\_transfers** <a id="get_transfers"></a>

‌

Returns a list of transfers.‌

**Alias**: _None_.‌

**Inputs**:‌

* _in_ - boolean; \(Optional\) Include incoming transfers.
* _out_ - boolean; \(Optional\) Include outgoing transfers.
* _pending_ - boolean; \(Optional\) Include pending transfers.
* _failed_ - boolean; \(Optional\) Include failed transfers.
* _pool_ - boolean; \(Optional\) Include transfers from the daemon's transaction pool.
* _filter\_by\_height_ - boolean; \(Optional\) Filter transfers by block height.
* _min\_height_ - unsigned int; \(Optional\) Minimum block height to scan for transfers, if filtering by height is enabled.
* _max\_height_ - unsigned int; \(Opional\) Maximum block height to scan for transfers, if filtering by height is enabled \(defaults to max block height\).
* _account\_index_ - unsigned int; \(Optional\) Index of the account to query for transfers. \(defaults to 0\)
* _subaddr\_indices_ - array of unsigned int; \(Optional\) List of subaddress indices to query for transfers. \(Defaults to empty - all indices\)

‌

**Outputs**:‌

* _in_ array of transfers:
  * _address_ - string; Public address of the transfer.
  * _amount_ - unsigned int; Amount transferred.
  * _confirmations_ - unsigned int; Number of block mined since the block containing this transaction \(or block height at which the transaction should be added to a block if not yet confirmed\).
  * _double\_spend\_seen_ - boolean; True if the key image\(s\) for the transfer have been seen before.
  * _fee_ - unsigned int; Transaction fee for this transfer.
  * _height_ - unsigned int; Height of the first block that confirmed this transfer \(0 if not mined yet\).
  * _note_ - string; Note about this transfer.
  * _payment\_id_ - string; Payment ID for this transfer.
  * _subaddr\_index_ - JSON object containing the major & minor subaddress index:
    * _major_ - unsigned int; Account index for the subaddress.
    * _minor_ - unsigned int; Index of the subaddress under the account.
  * _suggested\_confirmations\_threshold_ - unsigned int; Estimation of the confirmations needed for the transaction to be included in a block.
  * _timestamp_ - unsigned int; POSIX timestamp for when this transfer was first confirmed in a block \(or timestamp submission if not mined yet\).
  * _txid_ - string; Transaction ID for this transfer.
  * _type_ - string; Transfer type: "in"
  * _unlock\_time_ - unsigned int; Number of blocks until transfer is safely spendable.
* _out_ array of transfers \(see above\).
* _pending_ array of transfers \(see above\).
* _failed_ array of transfers \(see above\).
* _pool_ array of transfers \(see above\).

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_transfers","params":{"in":true}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "in": [{      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "amount": 50000000,      "confirmations": 102498,      "double_spend_seen": false,      "fee": 1953096,      "height": 328037,      "note": "",      "payment_id": "19295438056b27dd16fc6cb42a1c26eedff915e1210e1596dec67fd787912f3b",      "subaddr_index": {        "major": 0,        "minor": 0      },      "suggested_confirmations_threshold": 1,      "timestamp": 1555972854,      "txid": "35f9dccaf21dfe1df0945ebfc8b3ef28977b4ea1a78b3726c9f866facd27f7ad",      "type": "in",      "unlock_time": 0    }]  }}
```

‌

## **get\_transfer\_by\_txid** <a id="get_transfer_by_txid"></a>

‌

Show information about a transfer to/from this address.‌

**Alias**: _None_.‌

**Inputs**:‌

* _txid_ - string; Transaction ID used to find the transfer.
* _account\_index_ - unsigned int; \(Optional\) Index of the account to query for the transfer.

‌

**Outputs**:‌

* _transfer_ - JSON object containing payment information:
  * _address_ - string; Address that transferred the funds. Base58 representation of the public keys.
  * _amount_ - unsigned int; Amount of this transfer.
  * _confirmations_ - unsigned int; Number of block mined since the block containing this transaction \(or block height at which the transaction should be added to a block if not yet confirmed\).
  * _destinations_ - array of JSON objects containing transfer destinations:
    * _amount_ - unsigned int; Amount transferred to this destination.
    * _address_ - string; Address for this destination. Base58 representation of the public keys.
  * _double\_spend\_seen_ - boolean; True if the key image\(s\) for the transfer have been seen before.
  * _fee_ - unsigned int; Transaction fee for this transfer.
  * _height_ - unsigned int; Height of the first block that confirmed this transfer.
  * _note_ - string; Note about this transfer.
  * _payment\_id_ - string; Payment ID for this transfer.
  * _subaddr\_index_ - JSON object containing the major & minor subaddress index:
    * _major_ - unsigned int; Account index for the subaddress.
    * _minor_ - unsigned int; Index of the subaddress under the account.
  * _suggested\_confirmations\_threshold_ - unsigned int; Estimation of the confirmations needed for the transaction to be included in a block.
  * _timestamp_ - unsigned int; POSIX timestamp for the block that confirmed this transfer \(or timestamp submission if not mined yet\).
  * _txid_ - string; Transaction ID of this transfer \(same as input TXID\).
  * _type_ - string; Type of transfer, one of the following: "in", "out", "pending", "failed", "pool"
  * _unlock\_time_ - unsigned int; Number of blocks until transfer is safely spendable.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_transfer_by_txid","params":{"txid":"c36258a276018c3a4bc1f195a7fb530f50cd63a4fa765fb7c6f7f49fc051762a"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "transfer": {      "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",      "amount": 300000000000,      "confirmations": 1,      "destinations": [{        "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",        "amount": 100000000000      },{        "address": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",        "amount": 200000000000      }],      "double_spend_seen": false,      "fee": 21650200000,      "height": 153624,      "note": "",      "payment_id": "0000000000000000",      "subaddr_index": {        "major": 0,        "minor": 0      },      "suggested_confirmations_threshold": 1,      "timestamp": 1535918400,      "txid": "c36258a27600253a4bc1f195a7fb530f50cd63a4fa765fb7c6f7f49fc051762a",      "type": "out",      "unlock_time": 0    }  }}
```

‌

## **sign** <a id="sign"></a>

‌

Sign a string.‌

**Alias**: _None_.‌

**Inputs**:‌

* _data_ - string; Anything you need to sign.

‌

**Outputs**:‌

* _signature_ - string; Signature generated against the "data" and the account public address.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sign","params":{"data":"This is sample data to be signed"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "signature": "SigV13jebbGm9a1H4PbfXd1SZyPPmRtnJAJaoyf6Q3pE1ABsCT1MmiG3VyALNYmHEnjYhx71Z5Yx2TQenjb18C3DKRkGz"  }}
```

‌

## **verify** <a id="verify"></a>

‌

Verify a signature on a string.‌

**Alias**: _None_.‌

**Inputs**:‌

* _data_ - string; What should have been signed.
* _address_ - string; Public address of the wallet used to `sign` the data.
* _signature_ - string; signature generated by `sign` method.

‌

**Outputs**:‌

* _good_ - boolean;

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"verify","params":{"data":"This is sample data to be signed","address":"XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R","signature":"SigV13jebbGm9a1H4PbfXd1SZyPPmRtnJAJaoyf6Q3pE1ABsCT1MmiG3VyALNYmHEnjYhx71Z5Yx2TQenjb18C3DKRkGz"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "good": true  }}
```

‌

## **export\_outputs** <a id="export_outputs"></a>

‌

Export all outputs in hex format.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _outputs\_data\_hex_ - string; wallet outputs in hex format.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"export_outputs"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "outputs_data_hex": "...outputs..."  }}
```

‌

## **import\_outputs** <a id="import_outputs"></a>

‌

Import outputs in hex format.‌

**Alias**: _None_.‌

**Inputs**:‌

* _outputs\_data\_hex_ - string; wallet outputs in hex format.

‌

**Outputs**:‌

* _num\_imported_ - unsigned int; number of outputs imported.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"import_outputs","params":{"outputs_data_hex":"...outputs..."}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "num_imported": 6400  }}
```

‌

## **export\_key\_images** <a id="export_key_images"></a>

‌

Export a signed set of key images.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _signed\_key\_images_ - array of signed key images:
  * _key\_image_ - string;
  * _signature_ - string;

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"export_key_images"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "signed_key_images": [{      "key_image": "f3174f34054d51c55e0c474cce70a38434a4674cfd425c9edaf94509b93e5848",      "signature": "8ef6ce0f5267abd85021c3eeb18b5663fadd016ebdb0023c1657f2048fb1ed01d8d278cb1a71984c6ebe6fc053f46506c8267cfe19a1b5f1a3ee3dac89f7130b"    }]  }}
```

‌

## **import\_key\_images** <a id="import_key_images"></a>

‌

Import signed key images list and verify their spent status.‌

**Alias**: _None_.‌

**Inputs**:‌

* _signed\_key\_images_ - array of signed key images:
  * _key\_image_ - string;
  * _signature_ - string;

‌

**Outputs**:‌

* _height_ - unsigned int;
* _spent_ - unsigned int; Amount \(in [atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html)\) spent from those key images.
* _unspent_ - unsigned int; Amount \(in [atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html)\) still available from those key images.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"import_key_images", "params":{"signed_key_images":[{"key_image":"f3174f34054d51c55e0c474cce70a38434a4674cfd425c9edaf94509b93e5848","signature":"8ef6ce0f5267abd85021c3eeb18b5663fadd016ebdb0023c1657f2048fb1ed01d8d278cb1a71984c6ebe6fc053f46506c8267cfe19a1b5f1a3ee3dac89f7130b"}]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "height": 430787,    "spent": 0,    "unspent": 1000000000  }}
```

‌

## **make\_uri** <a id="make_uri"></a>

‌

Create a payment URI using the official URI spec.‌

**Alias**: _None_.‌

**Inputs**:‌

* _address_ - string; Wallet address
* _amount_ - unsigned int; \(optional\) the integer amount to receive, in [**atomic units**](https://web.getmonero.org/resources/moneropedia/atomic-units.html)​
* _payment\_id_ - string; \(optional\) 16 or 64 character hexadecimal payment id
* _recipient\_name_ - string; \(optional\) name of the payment recipient
* _tx\_description_ - string; \(optional\) Description of the reason for the tx

‌

**Outputs**:‌

* _uri_ - string; This contains all the payment input information as a properly formatted payment URI

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"make_uri","params":{"address":"XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R","amount":10,"payment_id":"420fa29b2d9a49f5","tx_description":"Testing out the make_uri function.","recipient_name":"XCASH"}}'  -H 'Content-Type: application/json'​{  "id": "0",  "jsonrpc": "2.0",  "result": {    "uri": "xcash:XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R?tx_payment_id=420fa29b2d9a49f5&tx_amount=0.000010&recipient_name=el00ruobuob%20Stagenet%20wallet&tx_description=Testing%20out%20the%20make_uri%20function."  }}
```

‌

## **parse\_uri** <a id="parse_uri"></a>



Parse a payment URI to get payment information.‌

**Alias**: _None_.‌

**Inputs**:‌

* _uri_ - string; This contains all the payment input information as a properly formatted payment URI

‌

**Outputs**:‌

* _uri_ - JSON object containing payment information:
  * _address_ - string; Wallet address
  * _amount_ - unsigned int; Integer amount to receive, in [**atomic units**](https://web.getmonero.org/resources/moneropedia/atomic-units.html) \(0 if not provided\)
  * _payment\_id_ - string; 16 or 64 character hexadecimal payment id \(empty if not provided\)
  * _recipient\_name_ - string; Name of the payment recipient \(empty if not provided\)
  * _tx\_description_ - string; Description of the reason for the tx \(empty if not provided\)

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"parse_uri","params":{"uri":"xcash:XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R?tx_payment_id=420fa29b2d9a49f5&tx_amount=0.000010&recipient_name=el00ruobuob%20Stagenet%20wallet&tx_description=Testing%20out%20the%20make_uri%20function."}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "uri": {      "address": "XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R",      "amount": 10,      "payment_id": "420fa29b2d9a49f5",      "recipient_name": "XCASH",      "tx_description": "Testing out the make_uri function."    }  }}
```

‌

## **get\_address\_book** <a id="get_address_book"></a>

Retrieves entries from the address book.‌

**Alias**: _None_.‌

**Inputs**:‌

* _entries_ - array of unsigned int; indices of the requested address book entries

‌

**Outputs**:‌

* _entries_ - array of entries:
  * _address_ - string; Public address of the entry
  * _description_ - string; Description of this address entry
  * _index_ - unsigned int;
  * _payment\_id_ - string;

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_address_book","params":{"entries":[0,1]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "entries": [{      "address": "XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R",      "description": "Second account",      "index": 0,      "payment_id": "0000000000000000000000000000000000000000000000000000000000000000"    },{      "address": "XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R",      "description": "Third account",      "index": 1,      "payment_id": "0000000000000000000000000000000000000000000000000000000000000000"    }]  }}
```

‌

## **add\_address\_book** <a id="add_address_book"></a>

‌

Add an entry to the address book.‌

**Alias**: _None_.‌

**Inputs**:‌

* _address_ - string;
* _payment\_id_ - \(optional\) string, defaults to "0000000000000000000000000000000000000000000000000000000000000000";
* _description_ - \(optional\) string, defaults to "";

‌

**Outputs**:‌

* _index_ - unsigned int; The index of the address book entry.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"add_address_book","params":{"address":"XCA1TzWy4E57dGZtVdciKsNV3rbAVTxgsEiH1bqv5aX3NNSVGavL2zUQN3k1i7pFifKFQ91ZtDjzf6TC7i6FwABA1Wr2ffSy6R","description":"Third account"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "index": 1  }}
```

‌

## **delete\_address\_book** <a id="delete_address_book"></a>

‌

Delete an entry from the address book.‌

**Alias**: _None_.‌

**Inputs**:‌

* _index_ - unsigned int; The index of the address book entry.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"delete_address_book","params":{"index":1}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **refresh** <a id="refresh"></a>

‌

Refresh a wallet after openning.‌

**Alias**: _None_.‌

**Inputs**:‌

* _start\_height_ - unsigned int; \(Optional\) The block height from which to start refreshing.

‌

**Outputs**:‌

* _blocks\_fetched_ - unsigned int; Number of new blocks scanned.
* _received\_money_ - boolean; States if transactions to the wallet have been found in the blocks.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"refresh","params":{"start_height":425000}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "blocks_fetched": 25,    "received_money": true  }}
```

‌

## **rescan\_spent** <a id="rescan_spent"></a>

‌

Rescan the blockchain for spent outputs.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"rescan_spent"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **start\_mining** <a id="start_mining"></a>

‌

Start mining in the X-Cash daemon.‌

**Alias**: _None_.‌

**Inputs**:‌

* _threads\_count_ - unsigned int; Number of threads created for mining.
* _do\_background\_mining_ - boolean; Allow to start the miner in [smart mining](https://web.getmonero.org/resources/moneropedia/smartmining.html)mode.
* _ignore\_battery_ - boolean; Ignore battery status \(for [smart mining](https://web.getmonero.org/resources/moneropedia/smartmining.html) only\)

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"start_mining","params":{"threads_count":1,"do_background_mining":true,"ignore_battery":false}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **stop\_mining** <a id="stop_mining"></a>

‌

Stop mining in the X-Cash daemon.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"stop_mining"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **get\_languages** <a id="get_languages"></a>

‌

Get a list of available languages for your wallet's seed.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _languages_ - array of string; List of available languages

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_languages"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "languages": ["Deutsch","English","Español","Français","Italiano","Nederlands","Português","русский язык","日本語","简体中文 (中国)","Esperanto","Lojban"]  }}
```

‌

## **create\_wallet** <a id="create_wallet"></a>

‌

Create a new wallet. You need to have set the argument "–wallet-dir" when launching xcash-wallet-rpc to make this work.‌

**Alias**: _None_.‌

**Inputs**:‌

* _filename_ - string; Wallet file name.
* _password_ - string; \(Optional\) password to protect the wallet.
* _language_ - string; Language for your wallets' seed.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"create_wallet","params":{"filename":"wallet1","password":"password","language":"English"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **open\_wallet** <a id="open_wallet"></a>

‌

Open a wallet. You need to have set the argument "–wallet-dir" when launching xcash-wallet-rpc to make this work.‌

**Alias**: _None_.‌

**Inputs**:‌

* _filename_ - string; wallet name stored in –wallet-dir.
* _password_ - string; \(Optional\) only needed if the wallet has a password defined.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"open_wallet","params":{"filename":"wallet1","password":"password"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **close\_wallet** <a id="close_wallet"></a>

‌

Close the currently opened wallet, after trying to save it.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"close_wallet"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **change\_wallet\_password** <a id="change_wallet_password"></a>

‌

Change a wallet password.‌

**Alias**: _None_.‌

**Inputs**:‌

* _old\_password_ - string; \(Optional\) Current wallet password, if defined.
* _new\_password_ - string; \(Optional\) New wallet password, if not blank.

‌

**Outputs**: _None_.‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"change_wallet_password","params":{"old_password":"password","new_password":"password2"}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {  }}
```

‌

## **is\_multisig** <a id="is_multisig"></a>

‌

Check if a wallet is a multisig one.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _multisig_ - boolean; States if the wallet is multisig
* _ready_ - boolean;
* _threshold_ - unsigned int; Amount of signature needed to sign a transfer.
* _total_ - unsigned int; Total amount of signature in the multisig wallet.

‌

Example for a non-multisig wallet:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"is_multisig"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "multisig": false,    "ready": false,    "threshold": 0,    "total": 0  }}
```

‌

Example for a multisig wallet:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"is_multisig"}' -H 'Content-Type: application/json'                  {  "id": "0",  "jsonrpc": "2.0",  "result": {    "multisig": true,    "ready": true,    "threshold": 2,    "total": 2  }}
```

‌

## **prepare\_multisig** <a id="prepare_multisig"></a>

‌

Prepare a wallet for multisig by generating a multisig string to share with peers.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _multisig\_info_ - string; Multisig string to share with peers to create the multisig wallet.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"prepare_multisig"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "multisig_info": "MultisigV1BFdxQ653cQHB8wsj9WJQd2VdnjxK89g5M94dKPBNw22reJnyJYKrz6rJeXdjFwJ3Mz6n4qNQLd6eqUZKLiNzJFi3UPNVcTjtkG2aeSys9sYkvYYKMZ7chCxvoEXVgm74KKUcUu4V8xveCBFadFuZs8shnxBWHbcwFr5AziLr2mE7KHJT"  }}
```

‌

## **make\_multisig** <a id="make_multisig"></a>

‌

Make a wallet multisig by importing peers multisig string.‌

**Alias**: _None_.‌

**Inputs**:‌

* _multisig\_info_ - array of string; List of multisig string from peers.
* _threshold_ - unsigned int; Amount of signatures needed to sign a transfer. Must be less or equal than the amount of signature in `multisig_info`.
* _password_ - string; Wallet password

‌

**Outputs**:‌

* _address_ - string; multisig wallet address.
* _multisig\_info_ - string; Multisig string to share with peers to create the multisig wallet \(extra step for N-1/N wallets\).

‌

**Example** for 2/2 Multisig Wallet:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"make_multisig","params":{"multisig_info":["MultisigV1K4tGGe8QirZdHgTYoBZMumSug97fdDyM3Z63M3ZY5VXvAdoZvx16HJzPCP4Rp2ABMKUqLD2a74ugMdBfrVpKt4BwD8qCL5aZLrsYWoHiA7JJwDESuhsC3eF8QC9UMvxLXEMsMVh16o98GnKRYz1HCKXrAEWfcrCHyz3bLW1Pdggyowop"],"threshold":2}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "address": "55SoZTKH7D39drxfgT62k8T4adVFjmDLUXnbzEKYf1MoYwnmTNKKaqGfxm4sqeKCHXQ5up7PVxrkoeRzXu83d8xYURouMod",    "multisig_info": ""  }}
```

‌

**Example** for 2/3 Multisig Wallet:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"make_multisig","params":{"multisig_info":["MultisigV1MTVm4DZAdJw1PyVutpSy8Q4WisZBCFRAaZY7hhQnMwr5AZ4swzThyaSiVVQM5FHj1JQi3zPKhQ4k81BZkPSEaFjwRJtbfqfJcVvCqRnmBVcWVxhnihX5s8fZWBCjKrzT3CS95spG4dzNzJSUcjheAkLzCpVmSzGtgwMhAS3Vuz9Pas24","MultisigV1TEx58ycKCd6ADCfxF8hALpcdSRAkhZTi1bu4Rs6FdRC98EdB1LY7TAkMxasM55khFgcxrSXivaSr5FCMyJGHmojm1eE4HpGWPeZKv6cgCTThRzC4u6bkkSoFQdbzWN92yn1XEjuP2XQrGHk81mG2LMeyB51MWKJAVF99Pg9mX2BpmYFj"],"threshold":2}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "address": "51sLpF8fWaK1111111111111111111111111111111111ABVbHNf1JFWJyFp5YZgZRQ44RiviJi1sPHgLVMbckRsDkTRgKS",    "multisig_info": "MultisigxV18jCaYAQQvzCMUJaAWMCaAbAoHpAD6WPmYDmLtBtazD654E8RWkLaGRf29fJ3stU471MELKxwufNYeigP7LoE4tn2Sscwn5g7PyCfcBc1V4ffRHY3Kxqq6VocSCUTncpVeUskaDKuTAWtdB9VTBGW7iG1cd7Zm1dYgur3CiemkGjRUAj9bL3xTEuyaKGYSDhtpFZFp99HQX57EawhiRHk3qq4hjWX"  }}
```

‌

## **export\_multisig\_info** <a id="export_multisig_info"></a>

‌

Export multisig info for other participants.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _info_ - string; Multisig info in hex format for other participants.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"export_multisig_info"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "info": "4d6f6e65726f206d756c7469736967206578706f72740105cf6442b09b75f5eca9d846771fe1a879c9a97ab0553ffbcec64b1148eb7832b51e7898d7944c41cee000415c5a98f4f80dc0efdae379a98805bb6eacae743446f6f421cd03e129eb5b27d6e3b73eb6929201507c1ae706c1a9ecd26ac8601932415b0b6f49cbbfd712e47d01262c59980a8f9a8be776f2bf585f1477a6df63d6364614d941ecfdcb6e958a390eb9aa7c87f056673d73bc7c5f0ab1f74a682e902e48a3322c0413bb7f6fd67404f13fb8e313f70a0ce568c853206751a334ef490068d3c8ca0e"  }}
```

‌

## **import\_multisig\_info** <a id="import_multisig_info"></a>

‌

Import multisig info from other participants.‌

**Alias**: _None_.‌

**Inputs:**‌

* _info_ - array of string; List of multisig info in hex format from other participants.

‌

**Outputs**:‌

* _n\_outputs_ - unsigned int; Number of outputs signed with those multisig info.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"import_multisig_info","params":{"info":["...multisig_info..."]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "n_outputs": 35  }}
```

‌

## **finalize\_multisig** <a id="finalize_multisig"></a>

‌

Turn this wallet into a multisig wallet, extra step for N-1/N wallets.‌

**Alias**: _None_.‌

**Inputs**:‌

* _multisig\_info_ - array of string; List of multisig string from peers.
* _password_ - string; Wallet password

‌

**Outputs**:‌

* _address_ - string; multisig wallet address.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"finalize_multisig","params":{"multisig_info":["MultisigxV1JNC6Ja2oBt5Sqea9LN2YEF7WYZCpHqr2EKvPG89Trf3X4E8RWkLaGRf29fJ3stU471MELKxwufNYeigP7LoE4tn2McPr4SbL9q15xNvZT5uwC9YRr7UwjXqSZHmTWN9PBuZEKVAQ4HPPyQciSCdNjgwsuFRBzrskMdMUwNMgKst1debYfm37i6PSzDoS2tk4kYTYj83kkAdR7kdshet1axQPd6HQ","MultisigxV1Unma7Ko4zdd8Ps3Af4oZwtj2JdWKzwNfP6s2G9ZvXhMoSscwn5g7PyCfcBc1V4ffRHY3Kxqq6VocSCUTncpVeUskMcPr4SbL9q15xNvZT5uwC9YRr7UwjXqSZHmTWN9PBuZE1LTpWxLoC3vPMSrqVVcjnmL9LYfdCZz3fECjNZbCEDq3PHDiUuY5jurQTcNoGhDTio5WM9xaAdim9YByiS5KyqF4"]}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "address": "5B9gZUTDuHTcGGuY3nL3t8K2tDnEHeRVHSBQgLZUTQxtFYVLnho5JJjWJyFp5YZgZRQ44RiviJi1sPHgLVMbckRsDqDx1gV"  }}
```

‌

## **sign\_multisig** <a id="sign_multisig"></a>

‌

Sign a transaction in multisig.‌

**Alias**: _None_.‌

**Inputs**:‌

* _tx\_data\_hex_ - string; Multisig transaction in hex format, as returned by `transfer` under `multisig_txset`.

‌

**Outputs**:‌

* _tx\_data\_hex_ - string; Multisig transaction in hex format.
* _tx\_hash\_list_ - array of string; List of transaction Hash.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sign_multisig","params":{"tx_data_hex":"...multisig_txset..."}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "tx_data_hex": "...multisig_txset...",    "tx_hash_list": ["4996091b61c1be112c1097fd5e97d8ff8b28f0e5e62e1137a8c831bacf034f2d"]  }}
```

‌

## **submit\_multisig** <a id="submit_multisig"></a>

‌

Submit a signed multisig transaction.‌

**Alias**: _None_.‌

**Inputs**:‌

* _tx\_data\_hex_ - string; Multisig transaction in hex format, as returned by `sign_multisig` under `tx_data_hex`.

‌

**Outputs**:‌

* _tx\_hash\_list_ - array of string; List of transaction Hash.

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"submit_multisig","params":{"tx_data_hex":"...tx_data_hex..."}}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "tx_hash_list": ["4996091b61c1be112c1097fd5e97d8ff8b28f0e5e62e1137a8c831bacf034f2d"]  }}
```

‌

## **get\_version** <a id="get_version"></a>

‌

Get RPC version Major & Minor integer-format, where Major is the first 16 bits and Minor the last 16 bits.‌

**Alias**: _None_.‌

**Inputs**: _None_.‌

**Outputs**:‌

* _version_ - unsigned int; RPC version, formatted with `Major * 2^16 + Minor`\(Major encoded over the first 16 bits, and Minor over the last 16 bits\).

‌

**Example**:

```text
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_version"}' -H 'Content-Type: application/json'{  "id": "0",  "jsonrpc": "2.0",  "result": {    "version": 65540  }}
```

## recover

Get the delegate information.

```text
curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"recover","params":{"domain_name":"DELEGATES_DOMAIN_NAME"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```
