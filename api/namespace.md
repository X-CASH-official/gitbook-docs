# X-Cash Namespace API

Note zachys (atomic units) are 10^6 in X-Cash

## Stats <a id="stats"></a>

This method gets the stats

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/stats/](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/stats/)

**Method**: GET

**Inputs**: _None_.

**Results**:

* _totalNamesRegisteredOrRenewed_ - unsigned int; The total registered or renewed names the delegates have processed.
* _totalVolume_ - unsigned long long; The total xcash paid to delegates using the namespace protocol in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/stats/ -H 'Accept: application/json'
{
  "totalNamesRegisteredOrRenewed": 100,
  "totalVolume": 10000000
}
```

## Registered Delegates <a id="registered-delegates"></a>

This method gets all of the registered delegates

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/delegates/registered](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/delegates/registered)

**Method**: GET

**Inputs**: _None_.

**Results**:

Array of objects with the following structure:

* _delegateName_ - string; The delegate name.
* _amount_ - unsigned long long; The amount to register or renew a name, using this delegate in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/delegates/registered/ -H 'Accept: application/json'
[
  {
    "delegateName": "us1_xcash_foundation",
    "amount": 100000000
  },
  {
    "delegateName": "europe1_xcash_foundation",
    "amount": 100000000
  }
]
```

## Delegates <a id="delegates"></a>

This method gets the delegates data

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/delegates/{delegateName}](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/delegates/{delegateName})

**Method**: GET

**Resources**:
* _delegateName_ - **Required** - The delegates name.

**Results**:

* _delegateName_ - string; The delegate name.
* _publicAddress_ - string; The public address.
* _amount_ - unsigned long long; The amount that a delegate charges to register or renew a name in zachys (atomic units).
* _totalNamesRegisteredOrRenewed_ - unsigned int; The total registered or renewed names that the delegate has processed.
* _totalVolume_ - unsigned long long; The total xcash paid to delegates using the namespace protocol in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/delegates/us1_xcash_foundation/ -H 'Accept: application/json'
{
  "delegateName": "us1_xcash_foundation",
  "publicAddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "amount": 100000000,
  "totalNamesRegisteredOrRenewed": 100,
  "totalVolume": 10000000
}
```

## Names <a id="names"></a>

This method gets the names data for a specific name

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/{name}](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/{name})

**Method**: GET

**Resources**:
* _name_ - **Required** - The namespace.

**Results**:

* _address_ - string; The address.
* _saddress_ - string; The saddress.
* _paddress_ - string; The paddress.
* _expires_ - unsigned int; The time the domain expires.
* _delegateName_ - string; The delegate that registered the namespace.
* _delegateAmount_ - unsigned long long; The total xcash paid to delegates to register or renew the namespace in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/xcash/ -H 'Accept: application/json'
{
  "address": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "saddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "paddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "expires": 1654228489,
  "delegateName": "us1_xcash_foundation",
  "delegateAmount": 100000000
}
```

## Name Status <a id="name-status"></a>

This method checks if a name can be purchased,either by it has not been registered yet, or it has expired

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/status/{name}](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/status/{name})

**Method**: GET

**Resources**:
* _name_ - **Required** - The namespace.

**Results**:

* _status_ - bool; True if one can register the name, otherwise false.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/status/xcash/ -H 'Accept: application/json'
{
  "status": false
}
```

## Address Status <a id="address-status"></a>

This method gets the address status

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/addresses/status/{address}](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/addresses/status/{address})

**Method**: GET

**Resources**:
* _address_ - **Required** - The address.

**Results**:

* _status_ - string; "not registered|address|saddress|paddress"

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/addresses/status/XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf/ -H 'Accept: application/json'
{
  "status": "address"
}
```

## Name To Address <a id="name-to-address"></a>

This method converts a name to an address

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/convert/{name}](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/convert/{name})

**Method**: GET

**Resources**:
* _name_ - **Required** - The namespace

**Results**:

* _address_ - string; The address.
* _saddress_ - string; The saddress.
* _paddress_ - string; The paddress.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/names/convert/xcash/ -H 'Accept: application/json'
{
  "address": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "saddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "paddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf"
}
```

## Address To Name <a id="address-to-name"></a>

This method converts an address to a name and extension

**URL**: [https://api.xcash.foundation/v1/xcash/namespace/unauthorized/addresses/convert/{address}](https://api.xcash.foundation/v1/xcash/namespace/unauthorized/addresses/convert/{address})

**Method**: GET

**Resources**:
* _address_ - **Required** - The address.

**Results**:

* _name_ - string; The name.
* _extension_ - string; The extension.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/namespace/unauthorized/addresses/convert/XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf/ -H 'Accept: application/json'
{
  "name": "xcash",
  "extension": ".pxcash"
}
```
