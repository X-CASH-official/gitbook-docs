---
description: >-
  Use X-Cash RPC Daemon (xcash-daemon-rpc) to interact with a daemon through
  JSON-RPC commands
---

# xcash-daemon-rpc

## JSON RPC Methods

This is a list of the xcashd daemon RPC calls, their inputs and outputs, and examples for each.

Many RPC calls use the daemon's JSON RPC interface while others use their own interfaces, as illustrated below.

Note: "atomic units" refer to the smallest fraction of 1 XCASH according to the xcashd implementation. **1 XCASH = 1e6 atomic units of XCASH.**

## get\_block\_count

Look up how many blocks are in the longest chain known to the node.

**Alias**: _getblockcount_.

**Inputs**: _None_.

**Outputs**:

* _count_ - unsigned int; Number of blocks in longest chain seen by the node.
* _status_ - string; General RPC error code. "OK" means everything looks good.

**Example:**

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_count"}' -H 'Content-Type: application/json' 
{  
  "id": "0",  
  "jsonrpc": "2.0",  
  "result": {  
    "count": 425000,  
    "status": "OK"  
  }  
}
```

## on\_get\_block\_hash

Look up a block's hash by its height.Block header information can be retrieved using either a block's hash or height. This method includes a block's hash as an input parameter to retrieve basic inform

**Alias**: _on\_getblockhash_.

**Inputs**:

* block height \(int array of length 1\)

**Outputs**:

* block hash \(string\)

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"on_get_block_hash","params":[425000]}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": "c8580494eca2ce274dcd39a43bb0f0e22328bdf805037b5fe878784f44e64b08"
}
```

## get\_block\_template

Get a block template on which mining a new block.

**Alias**: _getblocktemplate_.

**Inputs**:

* _wallet\_address_ - string; Address of wallet to receive coinbase transactions if block is successfully mined.
* _reserve\_size_ - unsigned int; Reserve size.

**Outputs**:

* _blocktemplate\_blob_ - string; Blob on which to try to mine a new block.
* _blockhashing\_blob_ - string; Blob on which to try to find a valid nonce.
* _difficulty_ - unsigned int; Difficulty of next block.
* _expected\_reward_ - unsigned int; Coinbase reward expected to be received if block is successfully mined.
* _height_ - unsigned int; Height on which to mine.
* _prev\_hash_ - string; Hash of the most recent block on which to mine the next block.
* _reserved\_offset_ - unsigned int; Reserved offset.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_template","params":{"wallet_address":"XCA1XPzaSeXgwrBrGbh96UD5bk21a4WabcrgtB14A7WGGdcagjVQVV1PMAg5Rj1SM3ca8ZPDvysi78HyZF9imGg48wRK2Ntqov","reserve_size":128}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "blockhashing_blob": "0c0ceadfe6eb051eb5164dfe508a6d7681d8b490a915cde9d0762d1b936beda7d4c9ee8bc9dc280000000093764ab48e856bb76960191001b5a22405ac317156acf4a0e8f9d4947123484303",
    "blocktemplate_blob": "0c0ceadfe6eb051eb5164dfe508a6d7681d8b490a915cde9d0762d1b936beda7d4c9ee8bc9dc280000000002c7a31a01ff8ba31a01f69a92cabb0102ecb13a092850dd0387b40a162e6b154c677e6a4a5ab6530b9c508fac4c5b9168a30101b53cfbe508ca940be5544d91c7cb6f34d4a62af1faa16453a3406f443979ca52028000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002acc77f92091e5ebccdd904da4a03e4eb9153f13e338add4b314b436858fb7ea33bc2ea30b4982c4336311f38caed30601f72ae22a44f11ae04ccd59663e135df",
    "difficulty": 272538347,
    "expected_reward": 50352917878,
    "height": 430475,
    "prev_hash": "1eb5164dfe508a6d7681d8b490a915cde9d0762d1b936beda7d4c9ee8bc9dc28",
    "reserved_offset": 129,
    "status": "OK",
    "untrusted": false
  }
}
```

## submit\_block

Submit a mined block to the network.

**Alias**: _submitblock_.

**Inputs**:

* Block blob data - array of strings; list of block blobs which have been mined. See get\_block\_template to get a blob on which to mine.

**Outputs**:

* _status_ - string; Block submit status.

### In this example, a block blob which has not been mined is submitted:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"submit_block","params":["0c0ceadfe6eb051eb5164dfe508a6d7681d8b490a915cde9d0762d1b936beda7d4c9ee8bc9dc280000000002c7a31a01ff8ba31a01f69a92cabb0102ecb13a092850dd0387b40a162e6b154c677e6a4a5ab6530b9c508fac4c5b9168a30101b53cfbe508ca940be5544d91c7cb6f34d4a62af1faa16453a3406f443979ca52028000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002acc77f92091e5ebccdd904da4a03e4eb9153f13e338add4b314b436858fb7ea33bc2ea30b4982c4336311f38caed30601f72ae22a44f11ae04ccd59663e135df"]' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "error": {
    "code": -7,
    "message": "Block not accepted"
  }
}
```

## get\_last\_block\_header

Block header information for the most recent block is easily retrieved with this method. No inputs are needed.

**Alias**: _getlastblockheader_.

**Inputs**: _None_.

**Outputs**:

* _block\_header_ - A structure containing block header information.
  * _block\_size_ - unsigned int; The block size in bytes.
  * _depth_ - unsigned int; The number of blocks succeeding this block on the blockchain. A larger number means an older block.
  * _difficulty_ - unsigned int; The strength of the X-Cash network based on mining power.
  * _hash_ - string; The hash of this block.
  * _height_ - unsigned int; The number of blocks preceding this block on the blockchain.
  * _major\_version_ - unsigned int; The major version of the xcash protocol at this block height.
  * _minor\_version_ - unsigned int; The minor version of the xcash protocol at this block height.
  * _nonce_ - unsigned int; a cryptographic random one-time number used in mining a X-Cash block.
  * _num\_txes_ - unsigned int; Number of transactions in the block, not counting the coinbase tx.
  * _orphan\_status_ - boolean; Usually `false`. If `true`, this block is not part of the longest chain.
  * _prev\_hash_ - string; The hash of the block immediately preceding this block in the chain.
  * _reward_ - unsigned int; The amount of new atomic units generated in this block and rewarded to the miner. Note: 1 XCASH = 1e6 atomic units.
  * _timestamp_ - unsigned int; The unix time at which the block was recorded into the blockchain.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

**In this example, the most recent block \(1562023 at the time\) is returned:**

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_last_block_header"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 103,
      "block_weight": 103,
      "cumulative_difficulty": 106864747145143,
      "depth": 0,
      "difficulty": 271286564,
      "hash": "062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1",
      "height": 430477,
      "major_version": 12,
      "minor_version": 12,
      "nonce": 42288,
      "num_txes": 0,
      "orphan_status": false,
      "pow_hash": "",
      "prev_hash": "d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb9",
      "reward": 50350297727,
      "timestamp": 1568256049
    },
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_block\_header\_by\_hash

Block header information can be retrieved using either a block's hash or height. This method includes a block's hash as an input parameter to retrieve basic information about the block.

**Alias**: _getblockheaderbyhash_.

**Inputs**:

* _hash_ - string; The block's sha256 hash.

**Outputs**:

* _block\_header_ - A structure containing block header information. See get\_last\_block\_header.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

### In this example, block 430477 is looked up by its hash:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_header_by_hash","params":{"hash":"062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 103,
      "block_weight": 103,
      "cumulative_difficulty": 106864747145143,
      "depth": 1,
      "difficulty": 271286564,
      "hash": "062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1",
      "height": 430477,
      "major_version": 12,
      "minor_version": 12,
      "nonce": 42288,
      "num_txes": 0,
      "orphan_status": false,
      "pow_hash": "",
      "prev_hash": "d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb9",
      "reward": 50350297727,
      "timestamp": 1568256049
    },
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_block\_header\_by\_height

Similar to get\_block\_header\_by\_hash above, this method includes a block's height as an input parameter to retrieve basic information about the block.

**Alias**: _getblockheaderbyheight_.

**Inputs**:

* _height_ - unsigned int; The block's height.

**Outputs**:

* _block\_header_ - A structure containing block header information. See get\_last\_block\_header.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

### In this example, block 430477 is looked up by its height \(notice that the returned information is the same as in the previous example\):

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_header_by_height","params":{"height":430477}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 103,
      "block_weight": 103,
      "cumulative_difficulty": 106864747145143,
      "depth": 4,
      "difficulty": 271286564,
      "hash": "062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1",
      "height": 430477,
      "major_version": 12,
      "minor_version": 12,
      "nonce": 42288,
      "num_txes": 0,
      "orphan_status": false,
      "pow_hash": "",
      "prev_hash": "d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb9",
      "reward": 50350297727,
      "timestamp": 1568256049
    },
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_block\_headers\_range

Similar to get\_block\_header\_by\_height above, but for a range of blocks. This method includes a starting block height and an ending block height as parameters to retrieve basic information about the range of blocks.

**Alias**: _getblockheadersrange_.

**Inputs**:

* _start\_height_ - unsigned int; The starting block's height.
* _end\_height_ - unsigned int; The ending block's height.

**Outputs**:

* _headers_ - array of `block_header` \(a structure containing block header information. See [get\_last\_block\_header](https://web.getmonero.org/resources/developer-guides/daemon-rpc.html#get_last_block_header)\).
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_headers_range","params":{"start_height":430477,"end_height":430478}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "headers": [{
      "block_size": 103,
      "block_weight": 103,
      "cumulative_difficulty": 106864747145143,
      "depth": 5,
      "difficulty": 271286564,
      "hash": "062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1",
      "height": 430477,
      "major_version": 12,
      "minor_version": 12,
      "nonce": 42288,
      "num_txes": 0,
      "orphan_status": false,
      "pow_hash": "",
      "prev_hash": "d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb9",
      "reward": 50350297727,
      "timestamp": 1568256049
    },{
      "block_size": 4137,
      "block_weight": 4137,
      "cumulative_difficulty": 106865020737874,
      "depth": 4,
      "difficulty": 273592731,
      "hash": "0e078893024b2425652e8d5e4a8f29ae2e31de49784f4953a2293113f1683ece",
      "height": 430478,
      "major_version": 12,
      "minor_version": 12,
      "nonce": 3400194129,
      "num_txes": 1,
      "orphan_status": false,
      "pow_hash": "",
      "prev_hash": "062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1",
      "reward": 50351553081,
      "timestamp": 1568256056
    }],
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_block

Full block information can be retrieved by either block height or hash, like with the above block header calls. For full block information, both lookups use the same method, but with different input parameters.

**Alias**: _getblock_.

**Inputs** \(pick one of the following\):

* _height_ - unsigned int; The block's height.
* _hash_ - string; The block's hash.

**Outputs**:

* _blob_ - string; Hexadecimal blob of block information.
* _block\_header_ - A structure containing block header information. See [get\_last\_block\_header](https://web.getmonero.org/resources/developer-guides/daemon-rpc.html#get_last_block_header).
* _json_ - json string; JSON formatted block details:
  * _major\_version_ - Same as in block header.
  * _minor\_version_ - Same as in block header.
  * _timestamp_ - Same as in block header.
  * _prev\_id_ - Same as `prev_hash` in block header.
  * _nonce_ - Same as in block header.
  * _miner\_tx_ - Miner transaction information
    * _version_ - Transaction version number.
    * _unlock\_time_ - The block height when the coinbase transaction becomes spendable.
    * _vin_ - List of transaction inputs:
      * _gen_ - Miner txs are coinbase txs, or "gen".
        * _height_ - This block height, a.k.a. when the coinbase is generated.
    * _vout_ - List of transaction outputs. Each output contains:
      * _amount_ - The amount of the output, in [atomic units](https://web.getmonero.org/resources/moneropedia/atomic-units.html).
      * _target_ -
        * _key_ -
    * _extra_ - Usually called the "transaction ID" but can be used to include any random 32 byte/64 character hex string.
    * _signatures_ - Contain signatures of tx signers. Coinbased txs do not have signatures.
  * _tx\_hashes_ - List of hashes of non-coinbase transactions in the block. If there are no other transactions, this will be an empty list.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

### **Look up by height:**

In the following example, block 430477 is looked up by its height. Note that block 912345 does not have any non-coinbase transactions. \(See the next example for a block with extra transactions\):

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block","params":{"height":430477}}' -H 'Content-Type: application/json'
```

### Look up by hash:

In the following example, block 430477 is looked up by its hash. Note that block 993056 has 3 non-coinbase transactions:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block","params":{"hash":"0e078893024b2425652e8d5e4a8f29ae2e31de49784f4953a2293113f1683ece"}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "blob": "0c0cb1e0e6eb05d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb930a5000002c9a31a01ff8da31a01ffa4f2c8bb01025ba00bae99b2ec6c7186dcda040cd6d98d28c604f8901bdf33b9d7b2baa91ef334018a8f0ef8e4b77e57ce1a2f887fec07548817f311d07a485e05aeeb9b290d274c021100000042105c1e000000000000000000000000",
    "block_header": {
      "block_size": 103,
      "block_weight": 103,
      "cumulative_difficulty": 106864747145143,
      "depth": 5,
      "difficulty": 271286564,
      "hash": "062537808276507cb05b5d5f80dbc8dd2bb79a9213be9958dfed1517742cc6a1",
      "height": 430477,
      "major_version": 12,
      "minor_version": 12,
      "nonce": 42288,
      "num_txes": 0,
      "orphan_status": false,
      "pow_hash": "",
      "prev_hash": "d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb9",
      "reward": 50350297727,
      "timestamp": 1568256049
    },
    "json": "{\n  \"major_version\": 12, \n  \"minor_version\": 12, \n  \"timestamp\": 1568256049, \n  \"prev_id\": \"d097546bd225af6957910154f55516072474f6173737ab45cfa50f9134580fb9\", \n  \"nonce\": 42288, \n  \"miner_tx\": {\n    \"version\": 2, \n    \"unlock_time\": 430537, \n    \"vin\": [ {\n        \"gen\": {\n          \"height\": 430477\n        }\n      }\n    ], \n    \"vout\": [ {\n        \"amount\": 50350297727, \n        \"target\": {\n          \"key\": \"5ba00bae99b2ec6c7186dcda040cd6d98d28c604f8901bdf33b9d7b2baa91ef3\"\n        }\n      }\n    ], \n    \"extra\": [ 1, 138, 143, 14, 248, 228, 183, 126, 87, 206, 26, 47, 136, 127, 236, 7, 84, 136, 23, 243, 17, 208, 122, 72, 94, 5, 174, 235, 155, 41, 13, 39, 76, 2, 17, 0, 0, 0, 66, 16, 92, 30, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0\n    ], \n    \"rct_signatures\": {\n      \"type\": 0\n    }\n  }, \n  \"tx_hashes\": [ ]\n}",
    "miner_tx_hash": "2e51fbf452789bb4fd27dc85136c97452b96ec33d60b68764bca77743d0966ff",
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_connections

Retrieve information about incoming and outgoing connections to your node.

**Alias**: _None_.

**Inputs**: _None_.

**Outputs**:

* _connections_ - List of all connections and their info:

  * _address_ - string; The peer's address, actually IPv4 & port
  * _avg\_download_ - unsigned int; Average bytes of data downloaded by node.
  * _avg\_upload_ - unsigned int; Average bytes of data uploaded by node.
  * _connection\_id_ - string; The connection ID
  * _current\_download_ - unsigned int; Current bytes downloaded by node.
  * _current\_upload_ - unsigned int; Current bytes uploaded by node.
  * _height_- unsigned int; The peer height
  * _host_ - string; The peer host
  * _incoming_ - boolean; Is the node getting information from your node?
  * _ip_ - string; The node's IP address.
  * _live\_time_ - unsigned int
  * _local\_ip_ - boolean
  * _localhost_ - boolean
  * _peer\_id_ - string; The node's ID on the network.
  * _port_ - string; The port that the node is using to connect to the network.
  * _recv\_count_ - unsigned int
  * _recv\_idle\_time_ - unsigned int
  * _send\_count_ - unsigned int
  * _send\_idle\_time_ - unsigned int
  * _state_ - string
  * _support\_flags_ - unsigned int

  **Following is an example of** get\_connections **and it's return:**

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_connections"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "connections": [{
      "address": "192.168.1.51:62950",
      "avg_download": 0,
      "avg_upload": 2,
      "connection_id": "083c301a3030329a487adb12ad981d2c",
      "current_download": 0,
      "current_upload": 2,
      "height": 1562127,
      "host": "192.168.1.51",
      "incoming": true,
      "ip": "192.168.1.51",
      "live_time": 8,
      "local_ip": false,
      "localhost": false,
      "peer_id": "c959fbfbed9e44fb",
      "port": "62950",
      "recv_count": 259,
      "recv_idle_time": 8,
      "send_count": 24342,
      "send_idle_time": 8,
      "state": "state_normal",
      "support_flags": 0
    },{
    ...
    }],
    "status": "OK"
  }
}
```

## get\_info

Retrieve general information about the state of your node and the network.

**Alias**:

* _/get\_info_
* _/getinfo_

See other RPC Methods /get\_info \(not JSON\)

**Inputs**: _None_.

**Outputs**:

* _alt\_blocks\_count_ - unsigned int; Number of alternative blocks to main chain.
* _block\_size\_limit_ - unsigned int; Maximum allowed block size
* _block\_size\_median_ - unsigned int; Median block size of latest 100 blocks
* _bootstrap\_daemon\_address_ - string; bootstrap node to give immediate usability to wallets while syncing by proxying RPC to it. \(Note: the replies may be untrustworthy\).
* _cumulative\_difficulty_ - unsigned int; Cumulative difficulty of all blocks in the blockchain.
* _difficulty_ - unsigned int; Network difficulty \(analogous to the strength of the network\)
* _free\_space_ - unsigned int; Available disk space on the node.
* _grey\_peerlist\_size_ - unsigned int; Grey Peerlist Size
* _height_ - unsigned int; Current length of longest chain known to daemon.
* _height\_without\_bootstrap_ - unsigned int; Current length of the local chain of the daemon.
* _incoming\_connections\_count_ - unsigned int; Number of peers connected to and pulling from your node.
* _mainnet_ - boolean; States if the node is on the mainnet \(`true`\) or not \(`false`\).
* _offline_ - boolean; States if the node is offline \(`true`\) or online \(`false`\).
* _outgoing\_connections\_count_ - unsigned int; Number of peers that you are connected to and getting information from.
* _rpc\_connections\_count_ - unsigned int; Number of RPC client connected to the daemon \(Including this RPC request\).
* _stagenet_ - boolean; States if the node is on the stagenet \(`true`\) or not \(`false`\).
* _start\_time_ - unsigned int; Start time of the daemon, as UNIX time.
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _target_ - unsigned int; Current target for next proof of work.
* _target\_height_ - unsigned int; The height of the next block in the chain.
* _testnet_ - boolean; States if the node is on the testnet \(`true`\) or not \(`false`\).
* _top\_block\_hash_ - string; Hash of the highest block in the chain.
* _tx\_count_ - unsigned int; Total number of non-coinbase transaction in the chain.
* _tx\_pool\_size_ - unsigned int; Number of transactions that have been broadcast but not included in a block.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).
* _was\_bootstrap\_ever\_used_ - boolean; States if a bootstrap node has ever been used since the daemon started.
* _white\_peerlist\_size_ - unsigned int; White Peerlist Size

### Following is an example `get_info` call and its return:

> ```bash
> $ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_info"}' -H 'Content-Type: application/json'
> {
>   "id": "0",
>   "jsonrpc": "2.0",
>   "result": {
>     "alt_blocks_count": 70,
>     "block_size_limit": 600000,
>     "block_size_median": 2637,
>     "block_weight_limit": 600000,
>     "block_weight_median": 2637,
>     "bootstrap_daemon_address": "",
>     "cumulative_difficulty": 106868133013691,
>     "database_size": 0,
>     "difficulty": 282970761,
>     "free_space": 18446744073709551615,
>     "grey_peerlist_size": 273,
>     "height": 430490,
>     "height_without_bootstrap": 430490,
>     "incoming_connections_count": 24,
>     "mainnet": true,
>     "nettype": "mainnet",
>     "offline": false,
>     "outgoing_connections_count": 8,
>     "rpc_connections_count": 5,
>     "stagenet": false,
>     "start_time": 1566064866,
>     "status": "OK",
>     "target": 120,
>     "target_height": 430207,
>     "testnet": false,
>     "top_block_hash": "5d0a508ece6b903284260d4835494f5fa3a75495e07c8ad0ed71a5ce97aa0be9",
>     "tx_count": 1168985,
>     "tx_pool_size": 3,
>     "untrusted": false,
>     "update_available": false,
>     "was_bootstrap_ever_used": false,
>     "white_peerlist_size": 1000
>   }
> }
> ```

## hard\_fork\_info

Look up information regarding hard fork voting and readiness.

**Alias**: _None_.

**Inputs**: _None_.

**Outputs**:

* _earliest\_height_ - unsigned int; Block height at which hard fork would be enabled if voted in.
* _enabled_ - boolean; Tells if hard fork is enforced.
* _state_ - unsigned int; Current hard fork state: 0 \(There is likely a hard fork\), 1 \(An update is needed to fork properly\), or 2 \(Everything looks good\).
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _threshold_ - unsigned int; Minimum percent of votes to trigger hard fork. Default is 80.
* _version_ - unsigned int; The major block version for the fork.
* _votes_ - unsigned int; Number of votes towards hard fork.
* _voting_ - unsigned int; Hard fork voting status.
* _window_ - unsigned int; Number of blocks over which current votes are cast. Default is 10080 blocks.

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"hard_fork_info"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "earliest_height": 281000,
    "enabled": true,
    "state": 1,
    "status": "OK",
    "threshold": 0,
    "untrusted": false,
    "version": 12,
    "votes": 10080,
    "voting": 12,
    "window": 10080
  }
}
```

## set\_bans

**Alias**: _None_.

**Inputs**:

* _bans_ - A list of nodes to ban:
  * _host_ - string; Host to ban \(IP in A.B.C.D form - will support I2P address in the future\).
  * _ip_ - unsigned int; IP address to ban, in Int format.
  * _ban_ - boolean; Set `true` to ban.
  * _seconds_ - unsigned int; Number of seconds to ban node.

**Outputs**:

* _status_ - string; General RPC error code. "OK" means everything looks good.

### **Examples :**

### Banning by host

In the following example, host is banned with its IP address string-formatted as A.B.C.D:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_bans","params":{"bans":[{"host":"192.168.1.51","ban":true,"seconds":30}]}}' -H  'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

### Banning by ip

In the following example, integer-formatted IP is banned:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_bans","params":{"bans":[{"ip":838969536,"ban":true,"seconds":30}]}}' -H  'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

## get\_bans

Get list of banned IPs.

**Alias**: _None_.

**Inputs**: _None_.

**Outputs**:

* _bans_ - List of banned nodes:
  * _host_ - string; Banned host \(IP in A.B.C.D form\).
  * _ip_ - unsigned int; Banned IP address, in Int format.
  * _seconds_ - unsigned int; Local Unix time that IP is banned until.
* _status_ - string; General RPC error code. "OK" means everything looks good.

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_bans"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "bans": [{
      "host": "102.168.1.51",
      "ip": 855746662,
      "seconds": 15
    },{
      "host": "192.168.1.50",
      "ip": 838969536,
      "seconds": 28
    }],
    "status": "OK"
  }
}
```

## flush\_txpool

Flush tx ids from transaction pool

**Alias**: _None_.

**Inputs**:

* _txids_ - array of strings; Optional, list of transactions IDs to flush from pool \(all tx ids flushed if empty\).

**Outputs**:

* _status_ - string; General RPC error code. "OK" means everything looks good.

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"flush_txpool"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

## get\_output\_histogram

Get a histogram of output amounts. For all amounts \(possibly filtered by parameters\), gives the number of outputs on the chain for that amount. RingCT outputs counts as 0 amount.

**Inputs**:

* _amounts_ - list of unsigned int
* _min\_count_ - unsigned int
* _max\_count_ - unsigned int
* _unlocked_ - boolean
* _recent\_cutoff_ - unsigned int

**Outputs**:

* _histogram_ - list of histogram entries, in the following structure:
  * _amount_ - unsigned int; Output amount in atomic units
  * _total\_instances_ - unsigned int;
  * _unlocked\_instances_ - unsigned int;
  * _recent\_instances_ - unsigned int;
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_output_histogram","params":{"amounts":[100000000000]}}'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "histogram": [{
      "amount": 100000000000,
      "recent_instances": 0,
      "total_instances": 0,
      "unlocked_instances": 0
    }],
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_coinbase\_tx\_sum

Get the coinbase amount and the fees amount for n last blocks starting at particular height

**Alias**: _None_.

**Inputs**:

* _height_ - unsigned int; Block height from which getting the amounts
* _count_ - unsigned int; number of blocks to include in the sum

**Outputs**:

* _emission\_amount_ - unsigned int; amount of coinbase reward in atomic units
* _fee\_amount_ - unsigned int; amount of fees in atomic units
* _status_ - string; General RPC error code. "OK" means everything looks good.

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_coinbase_tx_sum","params":{"height":425000,"count":1}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "emission_amount": 9387854817320,
    "fee_amount": 83981380000,
    "status": "OK"
  }
}
```

## get\_version

**Alias**: _None_.

**Inputs**: _None_.

**Outputs**:

* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).
* _version_ - unsigned int;

### Example:

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_version"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "untrusted": false,
    "version": 131073
  }
}
```

## get\_fee\_estimate

Gives an estimation on fees per kB.

**Alias**: _None_.

**Inputs**:

* _grace\_blocks_ - unsigned int; Optional

**Outputs**:

* _fee_ - unsigned int; Amount of fees estimated per kB in atomic units
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _untrusted_ - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

### Example :

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_fee_estimate"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "fee": 335,
    "quantization_mask": 1,
    "status": "OK",
    "untrusted": false
  }
}
```

## get\_alternate\_chains

Display alternative chains seen by the node.

**Alias**: _None_.

**Inputs**: _None_.

**Outputs**:

* _chains_ - array of chains, the following structure:
  * _block\_hash_ - string; the block hash of the first diverging block of this alternative chain.
  * _difficulty_ - unsigned int; the cumulative difficulty of all blocks in the alternative chain.
  * _height_ - unsigned int; the block height of the first diverging block of this alternative chain.
  * _length_ - unsigned int; the length in blocks of this alternative chain, after divergence.
* _status_ - string; General RPC error code. "OK" means everything looks good.

### Example

```c
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_alternate_chains"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "chains": [{
      "block_hash": "697cf03c89a9b118f7bdf11b1b3a6a028d7b3617d2d0ed91322c5709acf75625",
      "difficulty": 14114729638300280,
      "height": 1562062,
      "length": 2
    }],
    "status": "OK"
  }
}
```

## relay\_tx

get\_alternate\_chainsRelay a list of transaction IDs.

Alias: _None_.

Inputs:

* _txids_ - array of string; list of transaction IDs to relay

Outputs:

* _status_ - string; General RPC error code. "OK" means everything looks good.

**Example :**

```bash
curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"relay_tx","params":{"txids":["9fd75c429cbe52da9a52f2ffc5fbd107fe7fd2099c0d8de274dc8a67e0c98613"]}}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

## sync\_info

Get synchronisation informations

Alias: _None_.

Inputs: _None_.

Outputs:

* _height_ - unsigned int;
* _peers_ - array of peer structure, defined as follows:
  * _info_ - structure of connection info, as defined in get\_connections
* _spans_ - array of span structure, defined as follows \(optional, absent if node is fully synced\):
  * _connection\_id_ - string; Id of connection
  * _nblocks_ - unsigned int; number of blocks in that span
  * _rate_ - unsigned int; connection rate
  * _remote\_address_ - string; peer address the node is downloading \(or has downloaded\) than span from
  * _size_ - unsigned int; total number of bytes in that span's blocks \(including txes\)
  * _speed_ - unsigned int; connection speed
  * _start\_block\_height_ - unsigned int; block height of the first block in that span
* _status_ - string; General RPC error code. "OK" means everything looks good.
* _target\_height_ - unsigned int; target height the node is syncing from \(optional, absent if node is fully synced\)

**Example:**

```bash
$ curl -X POST http://EUSEED1.x-cash.org:18281/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sync_info"}' -H 'Content-Type: application/json'
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "height": 1563543,
    "peers": [{
      "info": {
        "address": "192.168.1.51:60064",
        "avg_download": 0,
        "avg_upload": 5,
        "connection_id": "204067223b9b3415c265dd25ad29ee48",
        "current_download": 0,
        "current_upload": 1,
        "height": 1559975,
        "host": "192.168.1.51",
        "incoming": true,
        "ip": "192.168.1.51",
        "live_time": 38,
        "local_ip": false,
        "localhost": false,
        "peer_id": "96b8545dbc7a8866",
        "port": "60064",
        "recv_count": 1580,
        "recv_idle_time": 28,
        "send_count": 203603,
        "send_idle_time": 8,
        "state": "state_normal",
        "support_flags": 1
      }
    },{
      "info": {
        ...
      }
    },{
      ...
    },{
      ...
    },{
      ...
    }],
    "status": "OK",
    "target_height": 1564067
  }
}
```

