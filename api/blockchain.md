# Blockchain API

Note zachys (atomic units) are 10^6 in X-Cash

## Stats <a id="stats"></a>

This method gets the stats

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/stats](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/stats)

**Method**: GET

**Inputs**: _None_.

**Results**:
* _height_ -  unsigned int; The current block height.
* _hash_ -  string; The current block hash.
* _reward_ -  unsigned long long; The current block reward in zachys (atomic units).
* _size_ -  unsigned long long; The size of the blockchain in bytes.
* _version_ - unsigned int; The hard fork version.
* _versionBlockHeight_ - unsigned int; The block height the hard fork version started on.
* _nextVersionBlockHeight_ - unsigned int; The block height of the next hard fork version.
* _totalPublicTx_ - unsigned int; The total public tx.
* _totalPrivateTx_ - unsigned int; The total private tx.
* _circulatingSupply_ - unsigned long long; The circulating supply in zachys (atomic units).
* _generatedSupply_ - unsigned long long; The generated supply in zachys (atomic units).
* _totalSupply_ - unsigned long long; The total supply in zachys (atomic units).
* _emissionReward_ - unsigned long long; The estimated constant block reward in zachys (atomic units).
* _emissionHeight_ - unsigned int; The estimated block height that will start the emission (when the constant block reward is hit).
* _emissionTime_ - unsigned int; The estimated time when the emission will start (when the constant block reward is hit).
* _inflationHeight_ - unsigned int; The estimated block height that will start the inflation (when the total supply increases).
* _inflationTime_ - unsigned int; The estimated time when the inflation will start (when the total supply increases).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/stats/ -H 'Accept: application/json'
{
  "height": 810000,
  "hash":"c7aa6eb38c47e7f013a5f8042477d1734ff9808fdc8608fb088085d624d2d509",
  "reward": 20000000,
  "size": 20000,
  "version": 13,
  "versionBlockHeight": 1000000,
  "nextVersionBlockHeight": 0,
  "totalPublicTx": 100000,
  "totalPrivateTx": 100000,
  "circulatingSupply": 10000000,
  "generatedSupply": 100000000,
  "totalSupply": 100000000,
  "emissionReward": 1000000000,
  "emissionHeight": 1000000000,
  "emissionTime": 1000000000,
  "inflationHeight": 1000000000,
  "inflationTime": 1000000000
}
```

## Block Data <a id="block-data"></a>

This method gets the block data

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/blocks/{blockHeight}](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/blocks/{blockHeight})

**Method**: GET

**Resources**:
* _blockHeight_ - Not required - The block height (default is current block height).

**Inputs**: _None_.

**Results**:

* _height_ -  unsigned int; The block height.
* _hash_ -  string; The block hash.
* _reward_ -  unsigned long long; The block reward in zachys (atomic units).
* _time_ - unsigned int; The time the block was produced.
* _xcashDPOPS_ - bool;
* _delegateName_ - string; The delegate name who produced the block.
* _tx_ - an array of transactions; The list of all transactions included in the block.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/blocks/810000/ -H 'Accept: application/json'
{
  "height": 810000,
  "hash": "c7aa6eb38c47e7f013a5f8042477d1734ff9808fdc8608fb088085d624d2d509",
  "reward": 47903837995,
  "time": 1615984630,
  "xcashDPOPS": false,
  "delegateName": "",
  "tx": ["c7aa6eb38c47e7f013a5f8042477d1734ff9808fdc8608fb088085d624d2d509","b7aa6eb38c47e7f013a5f8042477d1734ff9808fdc8608fb088085d624d2d509"]
}
```

## Transaction Data <a id="transaction-data"></a>

This method gets the transaction data

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/{txHash}](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/{txHash})

**Method**: GET

**Resources**:
* _tx_ - **Required** - The tx hash.

**Inputs**: _None_.

**Results**:

* _height_ -  unsigned int; The block height (0 if not added to a block).
* _confirmations_ -  unsigned int; The block confirmations.
* _time_ -  unsigned int; The time.
* _type_ -  string; "private | public".
* _sender_ -  string; The sender, if public.
* _receiver_ -  string; The receiver, if public.
* _amount_ -  unsigned long long; The amount, if public.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/18a5046994bec4e75d46fd17de3315592aa69d11f4b1a530717ea45a01d49312/ -H 'Accept: application/json'
{
  "height": 87004,
  "confirmations": 851116,
  "time": 1538097417,
  "type": "public",
  "sender": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",
  "receiver": "XCA1cH8Qs5hLYnzQTDuJqkJiQEZbgQsUM3BgA6vBod5T5Eindas5sikKJaLbkhM3YBW7PtoJY6BtNLkZuahksLFX5eSPDcmCLL",
  "amount": 5000000
}
```

## Prove Transaction <a id="prove-transaction"></a>

This method proves a tx and its amount

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/prove/](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/prove/)

**Method**: POST

**Inputs (All are required)**:

* _tx_ - The tx hash.
* _address_ - The destination public address.
* _key_ - The tx key or tx proof.

**Results**:

* _valid_ -  bool; Confirms that some amount was sent to the destination public address in this tx.
* _amount_ -  unsigned long long; The amount sent to the destination public address.

```bash
$ curl -X POST https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/prove/ -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"tx":"18a5046994bec4e75d46fd17de3315592aa69d11f4b1a530717ea45a01d49312","address":"XCA1cH8Qs5hLYnzQTDuJqkJiQEZbgQsUM3BgA6vBod5T5Eindas5sikKJaLbkhM3YBW7PtoJY6BtNLkZuahksLFX5eSPDcmCLL","key": "c10a439b706949e86146ca17c2fc41e24e4348d6b7a6d6af0623cfc5037fe20c"}'
{
  "valid": true,
  "amount": 5000000
}
```

## Prove Balance <a id="prove-balance"></a>

This method proves that a wallet has at least a certain balance

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/prove/](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/prove/)

**Method**: POST

**Inputs (All are required)**:

* _address_ - The public address to prove has a certain balance.
* _signature_ - The signature.

**Results**:

* _amount_ -  unsigned long long; The amount that the address has proven that it holds. Note the address can hold more than this amount, but not less than this amount.

```bash
$ curl -X POST https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/prove/ -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"address":"XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf","signature": "ReserveProofV1a15005880f5f88b19fc88bdec29faaf57489ba85dd02d41ec87043a5eddf95a9"}'
{
  "amount": 10000000
}
```

## Transaction History <a id="transaction-history"></a>

This method gets the public transaction history of an address

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/history/{type}/{address}](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/history/{type}/{address})

**Method**: GET

**Resources**:
* _type_ - **Required** - "sender | receiver".
* _address_ - **Required** - The address.

**Inputs**: _None_.

**Results**:

An array of objects with the following structure:

* _tx_ -  string; The tx.
* _key_ -  string; The tx key.
* _sender_ -  string; The sender.
* _receiver_ -  string; The receiver.
* _amount_ -  unsigned long long; The amount.
* _height_ -  unsigned int; The block height.
* _time_ -  unsigned int; The block timestamp.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/history/sender/XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe/ -H 'Accept: application/json'
[
  {
    "tx": "865fa7eeea471c99b85ff05f018ee315df0c95e64c11241a5ca0a40ab9b9909f",
    "key": "6b71ee1752c1228c8e239841071ef4b3a37b39d1c42b9ca801827aff5c2c3507",
    "sender": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",
    "receiver": "XCA1i6Lbx9JfZCE4di25V8VLwecPAHaH1CA37kC4k3S3gktV9i84R2G4jGnRqjJVMtPMA1GUKYZZCCPct7XGCPUh5qhSR89GUw",
    "amount": 15000000,
    "height": 524735,
    "time": 1579561606
  },
  {
    "tx": "7ecd79d7ead6c2f23cb404409aca01f8c9e30d7b07bcd4e9332a1c4872d1dd93",
    "key": "4a5fb3c39caf223eacfe4bcc2db84811c333b52ea30df5c16a6d62b0163cce0b",
    "sender": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",
    "receiver": "XCA1cH8Qs5hLYnzQTDuJqkJiQEZbgQsUM3BgA6vBod5T5Eindas5sikKJaLbkhM3YBW7PtoJY6BtNLkZuahksLFX5eSPDcmCLL",
    "amount": 5000000,
    "height": 160269,
    "time": 1542988620
  },
  {
    "tx": "961b0ded408fc94f79014ea15eb5844c79953e34d62905752b3e328c46cc3c15",
    "key": "8f68bb4aebe2f95f1a429611d15a57cff525e64478f2709eaadc0abdfc881302",
    "sender": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",
    "receiver": "XCA1cH8Qs5hLYnzQTDuJqkJiQEZbgQsUM3BgA6vBod5T5Eindas5sikKJaLbkhM3YBW7PtoJY6BtNLkZuahksLFX5eSPDcmCLL",
    "amount": 9746442,
    "height": 137722,
    "time": 1541632326
  },
  {
    "tx": "18a5046994bec4e75d46fd17de3315592aa69d11f4b1a530717ea45a01d49312",
    "key": "c10a439b706949e86146ca17c2fc41e24e4348d6b7a6d6af0623cfc5037fe20c",
    "sender": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",
    "receiver": "XCA1cH8Qs5hLYnzQTDuJqkJiQEZbgQsUM3BgA6vBod5T5Eindas5sikKJaLbkhM3YBW7PtoJY6BtNLkZuahksLFX5eSPDcmCLL",
    "amount": 5000000,
    "height": 87004,
    "time": 1538097417
  },
  {
    "tx": "c87228611c6db287a447b421cac5002867e27bc5d9976f451fa02700e9969142",
    "key": "60e1631d0f1095c833aba8963190a6ff22d450883b797a0e1543d6ee879d560a",
    "sender": "XCA1kzoR3ZLNg5zxNmxrY8FYKtgEvPZqC2xoRpm1axCpQcrrZfoKTSkSNsASDspdt3j1WcEnQJyuuB5VPSB56WWy36A4sQtQhe",
    "receiver": "XCA1cH8Qs5hLYnzQTDuJqkJiQEZbgQsUM3BgA6vBod5T5Eindas5sikKJaLbkhM3YBW7PtoJY6BtNLkZuahksLFX5eSPDcmCLL",
    "amount": 1000000,
    "height": 72893,
    "time": 1537243575
  }
]
```

## Validate Address <a id="validate-address"></a>

This method validates an address

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/validate/{address}](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/validate/{address})

**Method**: GET

**Resources**:
* _address_ - **Required** - The address.

**Inputs**: _None_.

**Results**:

An array of objects with the following structure:

* _valid_ -  bool; The validation status.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/validate/XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf/ -H 'Accept: application/json'
{
  "valid": true
}
```

## Create Integrated Address <a id="create-integrated-address"></a>

This method creates an integrated address

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/createIntegrated/](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/createIntegrated/)

**Method**: POST

**Inputs**:

* _address_ - **Required** - The public address.
* _paymentId_ - Not required - The payment id (either encrypted or long format).

**Results**:

* _integratedAddress_ - string; The integrated address
* _paymentId_ - string; The payment id

```bash
$ curl -X POST https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/createIntegrated/ -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"address":"XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf","paymentId": "0000000000000000"}'
{
  "integratedAddress": "XCBzxaX4V6yZMYzmKuEPrmZBws3NCxUVEcMtybS5KY6UPbPjTiW7dbdPHVgDmgU5gpRX1g1DYysUY2N3hafhYd6jBw9V9bSQqSo111116d3rEy",
  "paymentId": "0000000000000000"
}
```
