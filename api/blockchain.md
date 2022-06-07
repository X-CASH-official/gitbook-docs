# Blockchain API

Note zachys (atomic units) are 10^6 in X-Cash

## Stats <a id="stats"></a>

This method gets the stats

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/stats](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/stats)

**Method**: GET

**Inputs**: _None_.

**Results**:
* _height_ -  unsigned int; The current block height.
* _reward_ -  unsigned long long; The current block reward in zachys (atomic units).
* _size_ -  unsigned long long; The size of the blockchain in megabytes.
* _version_ - unsigned int; The hard fork version.
* _versionBlockHeight_ - unsigned int; The block height the hard fork version started on.
* _nextVersionBlockHeight_ - unsigned int; The block height of the next hard fork version.
* _totalPublicTx_ - unsigned int; The total public tx.
* _totalPrivateTx_ - unsigned int; The total private tx.
* _circulatingSupply_ - unsigned long long; The circulating supply in zachys (atomic units).
* _generatedSupply_ - unsigned long long; The generated supply in zachys (atomic units).
* _totalSupply_ - unsigned long long; The total supply in zachys (atomic units).
* _totalSupplyTime_ - unsigned long long; The estimated time that all xcash will be produced.
* _emissionBlockReward_ - unsigned long long; The estimated block reward after the total supply is produced.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/stats/ -H 'Accept: application/json'
{
  "height": 810000,
  "reward": 20000000,
  "size": 20000,
  "version": 13,
  "versionBlockHeight": 1000000,
  "nextVersionBlockHeight": 0,
  "totalPublicTx": 100000,
  "totalPrivateTx": 1000000,
  "circulatingSupply": 10000000,
  "generatedSupply": 100000000,
  "totalSupply": 100000000,
  "totalSupplyTime": 100000000,
  "emissionBlockReward": 1000000000,
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
  "tx": []
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
$ curl -X GET https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/d15005880f5f88b19fc88bdec29faaf57489ba85dd02d41ec87043a5eddf95a9/ -H 'Accept: application/json'
{
  "height": 810000,
  "confirmations": 100,
  "time": 10000000
  "type":" private,
  "sender": "",
  "receiver": "".
  "amount": 0
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
$ curl -X POST https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/tx/prove/ -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"tx":"d15005880f5f88b19fc88bdec29faaf57489ba85dd02d41ec87043a5eddf95a9","address":"XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf","key": "a15005880f5f88b19fc88bdec29faaf57489ba85dd02d41ec87043a5eddf95a9"}'
{
  "valid": true,
  "amount": 10000000
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

## Create Integrated Address <a id="create-integrated-address"></a>

This method creates an integrated address

**URL**: [https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/createintegrated/](https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/createintegrated/)

**Method**: POST

**Inputs**:

* _address_ **Required** - The public address.
* _paymentId_ Not required - The payment id (either encrypted or long format).

**Results**:

* _integratedAddress_ - string; The integrated address
* _paymentId_ - string; The payment id

```bash
$ curl -X POST https://api.xcash.foundation/v1/xcash/blockchain/unauthorized/address/createintegrated/ -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"address":"XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf","paymentId": "0000000000000000"}'
{
  "integratedAddress": "XCB000000000000000000000000000",
  "paymentId": "0000000000000000"
}
```
