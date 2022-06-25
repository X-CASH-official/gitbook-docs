---
description: This page contains every Wallet RPC calls related to DPoPS functions.
---

# DPoPS Wallet RPC calls

{% hint style="info" %}
As of 30/09/2019, these calls will not work until the mainnet is released. Only alpha and beta testers that installed the testnet of DPoPS can try them.
{% endhint %}

## **vote**

Place your vote for a delegate.

**Alias:** _vote_.

**Inputs:**

* _delegate\_data_ - String; Name or public address of the delegate to receive the vote.

**Outputs:**

* _vote\_status_ - string; Status of the vote call.

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"vote","params":{"delegate_data":"DELEGATES_NAME_OR_PUBLIC_ADDRESS"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "vote_status": "success"
  }
}
```

## **delegate\_register**

Register a delegate.

**Alias:** _delegateregister_.

**Inputs:**

* _delegate\_name_ - string; Name of the delegate to register.
* delegate\_IP\_address - string; IP of the delegate to register.

**Outputs:**

* _delegate\_register\_status_ - string; Status of the delegate registration call.

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"delegate_register","params":{"delegate_name":"delegate_name_1","delegate_IP_address":"delegate_IP_address_or_domain_name"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "delegate_register_status": "success"
  }
}
```

## **delegate\_update**

Update delegate information.

**Alias:** _delegateupdate_.

**Inputs:**

* _items_ - Parameters to change
  * _IP\_address_ - string; IP address of the delegate. 
  * _about_ - string; Description of the delegate.
  * _website -_ string; website address of the delegate.
  * _team -_ string; \_\_
  * _pool\_mode -_ string;
  * _fee\_structure -_ string; 
  * _server\_settings_ - string; 
* _value_ - string; Value of the field to update.

**Outputs:**

* _delegate\_update\_status_ - string; Status of the delegate update call.

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"delegate_update","params":{"item":"ITEM","value":"VALUE"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "delegate_update_status": "success"
  }
}
```

## **delegate\_remove**

Remove the delegate from the delegate list.

**Alias:** _delegateremove_.

**Inputs:** _None._

**Outputs:**

* _delegate\_remove\_status_ - string; Status of the delegate remove call.

**Example:**

```bash
$ curl -X POST http://localhost:18285/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"delegate_remove"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "delegate_remove_status": "success"
  }
}
```

