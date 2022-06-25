---
description: This page contains every Wallet RPC calls related to DPoPS functions.
---

# Namespace Wallet RPC calls

## **update\_remote\_data**

Update the remote data for namespace

**Alias:** _update_remote_data_.

**Inputs:**

* _item_ - String; The item to update (website or smart_contract_hash)
* _value_ - String; The value

**Outputs:**

* _status_ - string; Status

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"update_remote_data","params":{"item":"website","value":"websitename"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```

## **remote\_data\_save\_name**

Saves a name for 1 hour, so one can purchase it

**Alias:** _remote_data_save_name_.

**Inputs:**

* _name_ - String; The namespace name to save

**Outputs:**

* _status_ - string; Status

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"remote_data_save_name","params":{"name":"name"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```

## **remote\_data\_purchase\_name**

Pruchases the name once it has been saved

**Alias:** _remote_data_purchase_name_.

**Inputs:**

* _saddress_ - String; The saddress (private only address)
* _saddress_signature_ - String; The signature from the saddress wallet to proove access
* _paddress_ - String; The paddress (public only address)
* _paddress_signature_ - String; The signature from the paddress wallet to proove access
* _tx_hash_ - String; The tx to show the delegate was paid

**Outputs:**

* _status_ - string; Status

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"remote_data_purchase_name","params":{"saddress":"XCA","saddress_signature":"XCA","paddress":"XCA","paddress_signature":"XCA","tx_hash":"0000000000000000000000000000000000000000000000000000000000000000"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```

## **remote\_data\_delegate\_set\_amount**

Sets the amount the delegate wants to charge per namespace register or renew

**Alias:** _remote_data_delegate_set_amount.

**Inputs:**

* _amount_ - String; The amount in zachy's (10^6)

**Outputs:**

* _status_ - string; Status

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"remote_data_delegate_set_amount","params":{"amount":"100"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```

## **remote\_data\_renewal\_start**

Starts the renewal process

**Alias:** _remote_data_renewal_start.

**Outputs:**

* _status_ - string; Status

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"remote_data_renewal_start"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```

## **remote\_data\_renewal\_end**

Ends the renewal process

**Alias:** _remote_data_renewal_end.

**Inputs:**

* _tx_hash_ - String; The tx hash that paid the delegate to renew the name

**Outputs:**

* _status_ - string; Status

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"remote_data_renewal_end","params":{"tx_hash":"0000000000000000000000000000000000000000000000000000000000000000"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "success"
  }
}
```
