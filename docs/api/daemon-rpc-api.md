## 1. _getblockcount_

`GET` http://127.0.0.1:52521/json_rpc/#001

* Retrieves the current number of blocks in the main chain — the longest chain known to this node.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getblockcount\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "count": 1535,
    "status": "OK"
  }
}
```

* Outputs:

    count — unsigned integer; total number of blocks in the blockchain, including genesis block at height zero.

## 2. _on_getblockhash_

`GET` http://127.0.0.1:52521/json_rpc/#002

* Obtains block hash by given block height.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"on_getblockhash\",\"params\":[0]}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": "cc608f59f8080e2fbfe3c8c80eb6e6a953d47cf2d6aebd345bada3a1cab99852"
}
```

* Outputs:

    string; standard public address of the wallet.

!!! note

    Body Params
    
    block height `array of integers` required

    - Array of 1 unsigned integer; block height.

## 3. _getblocktemplate_

`GET` http://127.0.0.1:52521/json_rpc/#003

* Creates a template for the next block.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getblocktemplate\",\"params\":{\"extra_text\":\"miner comment\",\"wallet_address\":\"eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp\",\"stakeholder_address\":\"eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp\",\"pos_block\": false,\"pos_amount\": 0,\"pos_index\": 0}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "blocktemplate_blob": "0100000000000000004ac94cd86e558182ae7c09615f606316e6eb4d25a1a87af26394aa233c84959d00d1e09fe70500010100d4b4010180a094a58d1d0376f2f4eae8b5ca2f676bc0f5242d18603af7dbbcf60b10bc7732d4748d0496f3000516bea1995950357900a3e85dd6b82743276bfd25763abe08e550cb393fd7c6e7f8130d6d696e657220636f6d6d656e74150017431a0edeb401000000",
    "difficulty": "1563095640967",
    "height": 23124,
    "prev_hash": "4ac94cd86e558182ae7c09615f606316e6eb4d25a1a87af26394aa233c84959d",
    "seed": "0000000000000000000000000000000000000000000000000000000000000000",
    "status": "OK"
  }
}
```

* Outputs:

    `blocktemplate_blob` — string; hex-encoded serialized block template.

    `difficulty` — unsigned int, difficulty for the block template.
    
    `height` — unsigned int, height corresponding to the block template.
    
    `wallet_address` - address where transferred block reward
    
    `stakeholder_address` - used only in PoS mining, address where returned stake which was used in PoS generation (optional, normally is the same as wallet_address)

!!! note

    Body Params
    
    _wallet_address_ `string` required
    
    - miner's address for receiving newly generated coins 
    
    _extra_text_ `string`
    
    - additional text included into miner transaction. Cannot exceed 255 bytes.
    
    _pos_block_ `boolean`
    
    - specify type of block template to be created: PoS (true) or PoW (false). Default: false.

    _stakeholder_address_ `string`
    
    - specify miner's address to which the stake coins used in PoS block generation will be returned.
    
    _pos_amount_ `int32`
    
    - amount of an output used as a stake.
    
    _pos_index_ `int32`
    
    - global index of an output used as a stake.

## 4. _submitblock_

`POST` http://127.0.0.1:52521/json_rpc/#004

* Submits the given block, i.e., adds it to the local blockchain and broadcasts it to the network.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"submitblock\",\"params\":[\"0101_HEX_BLOCK_BLOB_0101\"]}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

_Block is accepted by the core_
    
```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

_Block was added as alternative_

```
{
  "error": {
    "code": -13,
    "message": "Block added as alternative"
  },
  "id": "0",
  "jsonrpc": "2.0"
}
```

_Block is not accepted by the core_

```
{
  "error": {
    "code": -7,
    "message": "Block not accepted"
  },
  "id": "0",
  "jsonrpc": "2.0"
}
```
!!! note

    Body Params
    
    _block's blob_ `array of strings` required

    - array of a single string; block's blob.

## 5. _getlastblockheader_

`GET` http://127.0.0.1:52521/json_rpc/#005

* Returns the header of the last block in the blockchain.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getlastblockheader\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "depth": 0,
      "difficulty": "1961934344476071811",
      "hash": "613a9eab47211b828aa231867aa83bc1a3573db44f47b510511f50e99e91ed21",
      "height": 23129,
      "major_version": 1,
      "minor_version": 0,
      "nonce": 0,
      "orphan_status": false,
      "prev_hash": "b6138d113a0150aeae2cb6909b803e3a22407c56f143ee2c9627b8ca5f3d9724",
      "reward": 2000000000000,
      "timestamp": 1558705095
    },
    "status": "OK"
  }
}
```

* Outputs:

    `block_header` — object; a block header object for the last block.

* block header object fields:

    `depth` — unsigned int; distance in blocks from the blockchain top. Always zero for this call.
    
    `difficulty` — unsigned int; block difficulty.
    
    `hash` — string; block identifier.
    
    `prev_hash` — string; identifier of the previous block.
    
    `height` — unsigned int; block height.

    `major_version` — unsigned int; major version of a block.
    
    `minor_version` — unsigned int; minor version of a block.
    
    `nonce` — unsigned int; block nonce.
    
    `orphan_status` — boolean; is this block orphan or not? Always false for this call.
    
    `reward` — unsigned int; how much money this block has generated.

    `timestamp` — unsigned int; block timestamp.

## 6. _getblockheaderbyhash_

`GET` http://127.0.0.1:52521/json_rpc/#006

* Returns a block header by the given hash identifier.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getblockheaderbyhash\",\"params\":{\"hash\":\"613a9eab47211b828aa231867aa83bc1a3573db44f47b510511f50e99e91ed21\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Request an existing block

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "depth": 1,
      "difficulty": "1961934344476071811",
      "hash": "613a9eab47211b828aa231867aa83bc1a3573db44f47b510511f50e99e91ed21",
      "height": 23129,
      "major_version": 1,
      "minor_version": 0,
      "nonce": 0,
      "orphan_status": false,
      "prev_hash": "b6138d113a0150aeae2cb6909b803e3a22407c56f143ee2c9627b8ca5f3d9724",
      "reward": 2000000000000,
      "timestamp": 1558705095
    },
    "status": "OK"
  }
}
```

* Request a NON-existing block

```
{
  "error": {
    "code": -5,
    "message": "Internal error: can't get block by hash. Hash = 0000000000000000000000000000000000000000000000000000000000000000."
  },
  "id": "0",
  "jsonrpc": "2.0"
}
```

* Outputs:

`block_header` — object; a block header object for the requested block. See getlastblockheader for detailed fields descriptions.

!!! note

    Body Params

    _hash_ `string` required

    - hash identifier of a block.

## 7. _getblockheaderbyheight_

`GET` http://127.0.0.1:52521/json_rpc/#007

* Returns a block header by the given block height.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getblockheaderbyheight\",\"params\":{\"height\":0}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Request an existing block.

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "depth": 23135,
      "difficulty": "1",
      "hash": "cc608f59f8080e2fbfe3c8c80eb6e6a953d47cf2d6aebd345bada3a1cab99852",
      "height": 0,
      "major_version": 1,
      "minor_version": 0,
      "nonce": 101011010205,
      "orphan_status": false,
      "prev_hash": "0000000000000000000000000000000000000000000000000000000000000000",
      "reward": 17517203000000000000,
      "timestamp": 0
    },
    "status": "OK"
  }
}
```

* Request a NON-existing block.

```
{
  "error": {
    "code": -2,
    "message": "To big height: 999999, current blockchain size = 2094"
  },
  "id": "0",
  "jsonrpc": "2.0"
}
```

* Outputs:

`block_header` — object; a block header object for the requested block. See getlastblockheader for detailed fields descriptions.

!!! note

    Body Params

    _height_ `int32` required

    - height of a block.

## 8. _get_alias_details_

`GET` http://127.0.0.1:52521/json_rpc/#008

* Returns alias details by alias name.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_alias_details\",\"params\":{\"alias\":\"crypto\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Correct request

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "alias_details": {
      "address": "eXDkdqs5U14QJBwtx2MLsxT5xHHKQ4XSwjEXVd7QVZoD4ntEVsS8MVZ9ZnKbEn5iKQ3UepcauqRU5gYU5qo2Ujxw2rseDaUdU",
      "comment": "",
      "tracking_key": ""
    },
    "status": "OK"
  }
}
```

* Request details for a NON-existing alias

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "alias_details": {
      "address": "",
      "comment": "",
      "tracking_key": ""
    },
    "status": "NOT FOUND"
  }
}
```

* Outputs:

    `alias_details` — object; an alias details block object for the requested alias.

* Alias details object fields:

    `address` — string; public address associated with requested alias.

    `comment` — string; an arbitrary comment set by the owner. Can be empty.

    `tracking_key` — string; private view key for public address. Can be empty.

!!! note 

    Body Params
    
    _alias_ `string` required

    - alias name (without "@")

## 9. _get_alias_by_address_

`GET` http://127.0.0.1:52521/json_rpc/#009

* Returns alias name by address.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_alias_by_address\", \"params\":\"eXDkdqs5U14QJBwtx2MLsxT5xHHKQ4XSwjEXVd7QVZoD4ntEVsS8MVZ9ZnKbEn5iKQ3UepcauqRU5gYU5qo2Ujxw2rseDaUdU\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Correct request

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "alias_info": {
      "address": "eXDkdqs5U14QJBwtx2MLsxT5xHHKQ4XSwjEXVd7QVZoD4ntEVsS8MVZ9ZnKbEn5iKQ3UepcauqRU5gYU5qo2Ujxw2rseDaUdU",
      "alias": "crypto",
      "comment": "",
      "tracking_key": ""
    },
    "status": "OK"
  }
}
```

* Request details for NON-existing alias

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "alias_info": {
      "address": "",
      "alias": "",
      "comment": "",
      "tracking_key": ""
    },
    "status": "NOT_FOUND"
  }
}
```

* Outputs:

    `alias` — string; alias name for an address.

!!! note

    Body Params
    
    _string_ `string` required

    - a public address associated with an alias.

## 10. _get_alias_reward_

`GET` http://127.0.0.1:52521/json_rpc/#010

* Returns current reward that must be paid to register an alias name.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_alias_reward\",\"params\":{\"alias\":\"crypto\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "reward": 100000000000,
    "status": "OK"
  }
}
```

* Outputs:

    `reward` — unsigned int; current reward (in atomic units) to be paid for an alias.

!!! note

    Body Params
    
    _alias_ `string`

    - alias name

## 11. _get_blocks_details_

`GET` http://127.0.0.1:52521/json_rpc/#011

* Return blocks details for a specified range of heights.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_blocks_details\",\"params\":{\"height_start\":10000,\"count\":1}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "blocks": [
      {
        "actual_timestamp": 1557906733,
        "already_generated_coins": "17527203000000000000",
        "base_reward": 1000000000000,
        "blob": "",
        "block_cumulative_size": 1690,
        "block_tself_size": 0,
        "cumulative_diff_adjusted": "27934854069161424",
        "cumulative_diff_precise": "16964766616549681",
        "difficulty": "3792479698469",
        "effective_fee_median": 50050000,
        "height": 10000,
        "id": "fe42b8c4742d2dbe4d5de5ae5212f75acb62b75e4b66b758ff8e252825c2d7a5",
        "is_orphan": false,
        "miner_text_info": "",
        "object_in_json": "...",
        "penalty": 0,
        "pow_seed": "",
        "prev_id": "8ee7377d6ee3632ea5b43bed2a5ea41c035abb3fe6146aec63e6c5cbbc4257f1",
        "summary_reward": 1000100000000,
        "this_block_fee_median": 100000000,
        "timestamp": 1557906733,
        "total_fee": 100000000,
        "total_txs_size": 1690,
        "transactions_details": [
          {
            "amount": 1000100000000,
            "blob": "",
            "blob_size": 136,
            "fee": 0,
            "id": "018c8bcf749063ee4a8127b84586a03f9bbd1909d9e5cfdb92007ebc463462d4",
            "keeper_block": 10000,
            "object_in_json": "",
            "pub_key": "b5800dff01803ec97edf956c7c82cc2658333de5ca3c860e68f7f8f216f3baa9",
            "timestamp": 1557906733
          },
          {
            "amount": 2999900000000,
            "blob": "",
            "blob_size": 1690,
            "fee": 100000000,
            "id": "612ca0baf8a1a8b7b86d9b56e3b9e1ef4bf6af6296ece8d75e85601fe3987b7b",
            "keeper_block": 10000,
            "object_in_json": "",
            "pub_key": "bf6e7ab5488f612e0ad8f00931633f01136d69f6e973130105431b080fac2a95",
            "timestamp": 1557906733
          }
        ],
        "type": 1
      }
    ],
    "status": "OK"
  }
}
```

* Outputs:

    blocks — array of `block_rpc_extended_info` objects.

* block_rpc_extended_info object fields:

    `actual_timestamp` — unsigned int; timestamp for the moment of block creation (for PoW blocks equal to timestamp, for PoS they differ).
    
    `already_generated_coins` — unsigned int; total number of coins generated, including this block.
    
    `base_reward` — unsigned int; base reward for the block (equal to reward if there are no transactions except the miner tx).
    
    `pow_seed` — some hex string .
    
    `block_cumulative_size` — unsigned int; total size of block's transactions, in bytes. Miner tx is included in special cases.
    
    `cumulative_diff_adjusted` — unsigned int; cumulative PoS or PoW difficulty for the block, adjusted by the sequence factor (number of same type blocks going sequentially one-by-one).
    
    `cumulative_diff_precise` — unsigned int; precise cumulative PoS or PoW difficulty for the block.
    
    `difficulty` — unsigned int; difficulty of the block.
    
    `effective_fee_median` — unsigned int; median of transaction fees within a specific window used in calculations for this block.
    
    `height` — unsigned int; block height.
    
    `id` — string; block hash identifier.
    
    `is_orphan` — boolean; orphan status for the block. False for normal blocks.
    
    `miner_text_info` — string; undefined text inserted by miner when the block was mined.
    
    `object_in_json` — string; JSON-serialized block object.
    
    `penalty` — unsigned int; difference between summary_reward and base_reward.
    
    `prev_id` — string; hash identifier of the previous block.
    
    `summary_reward` — unsigned int; amount of coins this block has generated in miner tx.
    
    `this_block_fee_median` — unsigned int; median fee among the transactions for this block.
    
    `timestamp` — unsigned int; block timestamp (serves a special purpose for PoS blocks, which is why actual_timestamp should be used as actual block timestamp).
    
    `total_fee` — unsigned int; sum of transaction fees in this block.
    
    `total_txs_size` — unsigned int; total transaction size in this block (excluding the miner tx).
    
    `transactions_details` — array of tx_rpc_extended_info objects (see below get_tx_details description).
    
    `type` — unsigned int; 0 if this is PoS block, 1 if this is PoW block.

!!! note

    Body Params

    _height_start_ `int32`

    - starting height

    _count_ `int32`
    
    - number of blocks to be requested

## 12. _get_tx_details_

`GET` http://127.0.0.1:52521/json_rpc/#012

* Returns transaction details by specified transaction hash identifier.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_tx_details\",\"params\":{\"tx_hash\":\"d4af4b8e714610865bab09375c40a625ea741ac6f2d271ee398e0ffa5c0af74c\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Correct response transaction

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "tx_info": {
      "amount": 1000000200000,
      "blob": "",
      "blob_size": 386,
      "extra": [
        {
          "datails_view": "",
          "short_view": "a6c1a6d3453a6f21c32bd03d29b30add6bb9dcc55c23961d863b0faad5643aa8",
          "type": "pub_key"
        },
        {
          "datails_view": "520f",
          "short_view": "520f",
          "type": "XOR"
        },
        {
          "datails_view": "c885",
          "short_view": "c885",
          "type": "XOR"
        }
      ],
      "fee": 100000,
      "id": "d4af4b8e714610865bab09375c40a625ea741ac6f2d271ee398e0ffa5c0af74c",
      "ins": [
        {
          "amount": 1000000000000,
          "global_indexes": [
            33772
          ],
          "kimage_or_ms_id": "bc97426f220d02badf7f836cedc0b10e15fa1abc0163000266c84a93e10d52cb",
          "multisig_count": 0
        },
        {
          "amount": 300000,
          "global_indexes": [
            459
          ],
          "kimage_or_ms_id": "2cedd318bb5e75d5ae56a1801c1804e50da5efa708854b254b1f159fe55ce787",
          "multisig_count": 0
        }
      ],
      "keeper_block": 23183,
      "object_in_json": "",
      "outs": [
        {
          "amount": 100000,
          "global_index": 3590,
          "is_spent": false,
          "minimum_sigs": 0,
          "pub_keys": [
            "242884c767abe6d9d855f31119314fee229db2f014f80c46c1ab14428be818cf"
          ]
        },
        {
          "amount": 100000,
          "global_index": 3591,
          "is_spent": false,
          "minimum_sigs": 0,
          "pub_keys": [
            "e6295a3dc3a0b139feb330ec6591cc76f29468fb660ffc8413c883221117b6c2"
          ]
        },
        {
          "amount": 1000000000000,
          "global_index": 34112,
          "is_spent": false,
          "minimum_sigs": 0,
          "pub_keys": [
            "23587fbe7731bce91b7a81b0bcb310a8430057d426a6842e205aeb223c9457c5"
          ]
        }
      ],
      "pub_key": "a6c1a6d3453a6f21c32bd03d29b30add6bb9dcc55c23961d863b0faad5643aa8",
      "timestamp": 1558706609
    }
  }
}
```

* Request a NON-existing transaction

```
{
  "error": {
    "code": -14,
    "message": "tx is not found"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Outputs:

    `tx_info` — object; a `tx_rpc_extended_info` object (see below).

* tx_rpc_extended_info object fields:

    `amount` — unsigned int; sum of transaction outputs.
    
    `attachments` — array of objects; list of transaction attachments.
    
    `blob_size` — unsigned int; size of serialized transaction in bytes.
    
    `extra` — array of objects; list of extra items.
    
    `fee` — unsigned int; transaction fee.
    
    `id` — string; hash identifier of the transaction.
    
    `ins` — array of objects; list of inputs.
    
    `keeper_block` — unsigned int; height of the block containing this transaction.
    
    `outs` — array of objects; list of outputs.
    
    `pub_key` — string; transaction public key.
    
    `timestamp` — unsigned int; actual timestamp of the block containing this transaction.

!!! note 

    Body Params
    
    _tx_hash_ `string` required

    - hash identifier of a transaction

## 13. _search_by_id_

`GET` http://127.0.0.1:52521/json_rpc/#013

* Returns type of an entity by specified hash identifier.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"search_by_id\",\"params\":{\"id\":\"d4af4b8e714610865bab09375c40a625ea741ac6f2d271ee398e0ffa5c0af74c\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Response `application/json` block

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "types_found": [
      "block"
    ]
  }
}
```

* Response `text/plain` transaction

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "types_found": [
      "block"
    ]
  }
}
```

* Outputs:

    `types_found` — array of strings; a set of the types found (usually only one). 

Possible values: block, alt_block, key_image, tx, multisig_id.

!!! note

    Body Params
    
    _id_ `string` required

    - hash identifier of a block, transaction, key image, or multisig output

## 14. _getinfo_

`GET` http://127.0.0.1:52521/json_rpc/#014

* Returns various information and stats.

List of available flags values:

|                    RPC Command                         |       Flag         |
|--------------------------------------------------------|--------------------|
| COMMAND_RPC_GET_INFO_FLAG_POS_DIFFICULTY               | 0x0000000000000001 |
| COMMAND_RPC_GET_INFO_FLAG_POW_DIFFICULTY               | 0x0000000000000002 |
| COMMAND_RPC_GET_INFO_FLAG_NET_TIME_DELTA_MEDIAN        | 0x0000000000000004 |
| COMMAND_RPC_GET_INFO_FLAG_CURRENT_NETWORK_HASHRATE_50  | 0x0000000000000008 |
| COMMAND_RPC_GET_INFO_FLAG_CURRENT_NETWORK_HASHRATE_350 | 0x0000000000000010 |
| COMMAND_RPC_GET_INFO_FLAG_SECONDS_FOR_10_BLOCKS	     | 0x0000000000000020 |
| COMMAND_RPC_GET_INFO_FLAG_SECONDS_FOR_30_BLOCKS	     | 0x0000000000000040 |
| COMMAND_RPC_GET_INFO_FLAG_TRANSACTIONS_DAILY_STAT 	 | 0x0000000000000080 |
| COMMAND_RPC_GET_INFO_FLAG_LAST_POS_TIMESTAMP	         | 0x0000000000000100 |
| COMMAND_RPC_GET_INFO_FLAG_LAST_POW_TIMESTAMP	         | 0x0000000000000200 |
| COMMAND_RPC_GET_INFO_FLAG_TOTAL_COINS	                 | 0x0000000000000400 |
| COMMAND_RPC_GET_INFO_FLAG_LAST_BLOCK_SIZE	             | 0x0000000000000800 |
| COMMAND_RPC_GET_INFO_FLAG_TX_COUNT_IN_LAST_BLOCK	     | 0x0000000000001000 |
| COMMAND_RPC_GET_INFO_FLAG_POS_SEQUENCE_FACTOR	         | 0x0000000000002000 |
| COMMAND_RPC_GET_INFO_FLAG_POW_SEQUENCE_FACTOR	         | 0x0000000000004000 |
| COMMAND_RPC_GET_INFO_FLAG_OUTS_STAT	                 | 0x0000000000008000 |
| COMMAND_RPC_GET_INFO_FLAG_PERFORMANCE	                 | 0x0000000000010000 |
| COMMAND_RPC_GET_INFO_FLAG_POS_BLOCK_TS_SHIFT_VS_ACTUAL | 0x0000000000020000 |
| COMMAND_RPC_GET_INFO_FLAG_MARKET	                     | 0x0000000000040000 |
| COMMAND_RPC_GET_INFO_FLAG_ALL_FLAGS	                 | 0xffffffffffffffff |

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getinfo\",\"params\":{\"flags\":4294967295}}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "alias_count": 89,
    "alt_blocks_count": 38,
    "block_reward": 1000000000000,
    "current_blocks_median": 125000,
    "current_max_allowed_block_size": 250000,
    "current_network_hashrate_350": 12371661017,
    "current_network_hashrate_50": 22307725414,
    "daemon_network_state": 2,
    "default_fee": 10000000000,
    "expiration_median_timestamp": 1558706697,
    "grey_peerlist_size": 52,
    "height": 23195,
    "incoming_connections_count": 17,
    "last_block_hash": "941637785d710dddd3391eb7adc451db31855c99ac31cec4f221ad856b86871f",
    "last_block_size": 0,
    "last_block_timestamp": 1558707495,
    "last_block_total_reward": 1000000000000,
    "last_pos_timestamp": 1558707495,
    "last_pow_timestamp": 1558707021,
    "max_net_seen_height": 21097,
    "mi": {
      "build_no": 26,
      "mode": 0,
      "ver_major": 1,
      "ver_minor": 0,
      "ver_revision": 0
    },
    "minimum_fee": 10000000000,
    "net_time_delta_median": 0,
    "offers_count": 0,
    "outgoing_connections_count": 8,
    "outs_stat": {
      "amount_0_001": 1636,
      "amount_0_01": 3177,
      "amount_0_1": 1829,
      "amount_1": 34129,
      "amount_10": 407,
      "amount_100": 719,
      "amount_1000": 6,
      "amount_10000": 2,
      "amount_100000": 0,
      "amount_1000000": 0
    },
    "performance_data": {
      "all_txs_insert_time_5": 106,
      "block_processing_time_0": 8000,
      "block_processing_time_1": 8337,
      "etc_stuff_6": 681,
      "insert_time_4": 16,
      "longhash_calculating_time_3": 1074,
      "map_size": 137438953472,
      "raise_block_core_event": 0,
      "target_calculating_calc": 11,
      "target_calculating_enum_blocks": 5799,
      "target_calculating_time_2": 5816,
      "tx_add_one_tx_time": 7432,
      "tx_append_is_expired": 0,
      "tx_append_rl_wait": 0,
      "tx_append_time": 253,
      "tx_check_exist": 6,
      "tx_check_inputs_attachment_check": 0,
      "tx_check_inputs_loop": 10133,
      "tx_check_inputs_loop_ch_in_val_sig": 1368,
      "tx_check_inputs_loop_kimage_check": 16,
      "tx_check_inputs_loop_scan_outputkeys_get_item_size": 10,
      "tx_check_inputs_loop_scan_outputkeys_loop": 122,
      "tx_check_inputs_loop_scan_outputkeys_loop_find_tx": 24,
      "tx_check_inputs_loop_scan_outputkeys_loop_get_subitem": 10,
      "tx_check_inputs_loop_scan_outputkeys_loop_handle_output": 0,
      "tx_check_inputs_loop_scan_outputkeys_relative_to_absolute": 0,
      "tx_check_inputs_prefix_hash": 0,
      "tx_check_inputs_time": 7175,
      "tx_count": 0,
      "tx_prapare_append": 0,
      "tx_print_log": 0,
      "tx_process_attachment": 0,
      "tx_process_extra": 0,
      "tx_process_inputs": 66,
      "tx_push_global_index": 113,
      "tx_store_db": 20,
      "writer_tx_count": 0
    },
    "pos_allowed": true,
    "pos_block_ts_shift_vs_actual": 459,
    "pos_diff_total_coins_rate": 118,
    "pos_difficulty": "2748846848713493647",
    "pos_sequence_factor": 2,
    "pow_difficulty": 1701500087805,
    "pow_sequence_factor": 0,
    "seconds_for_10_blocks": 440,
    "seconds_for_30_blocks": 889,
    "status": "OK",
    "synchronization_start_height": 21094,
    "synchronized_connections_count": 25,
    "total_coins": "17540397000000000000",
    "transactions_cnt_per_day": 559,
    "transactions_volume_per_day": 5337825461100000,
    "tx_count": 9267,
    "tx_count_in_last_block": 0,
    "tx_pool_performance_data": {
      "begin_tx_time": 7,
      "check_inputs_time": 4567,
      "check_inputs_types_supported_time": 0,
      "check_keyimages_ws_ms_time": 20,
      "db_commit_time": 13276,
      "expiration_validate_time": 1,
      "tx_processing_time": 18030,
      "update_db_time": 132,
      "validate_alias_time": 8,
      "validate_amount_time": 0
    },
    "tx_pool_size": 3,
    "white_peerlist_size": 128
  }
}
```

* Outputs:

    - `alias_count` — unsigned int; number of total aliases registered.
    
    - `alt_blocks_count` — unsigned int; number of alternative blocks known to this node.
    
    - `block_reward` — unsigned int; base block reward for the next block (excluding fees and txs size penalty).

        - Calculated only if both `COMMAND_RPC_GET_INFO_FLAG_POS_DIFFICULTY` and `COMMAND_RPC_GET_INFO_FLAG_TOTAL_COINS` flags are present.

    - `current_blocks_median` — unsigned int; median of cumulative block sizes for the recent N blocks.
    
    - `current_max_allowed_block_size` — unsigned int; maximum allowed cumulative size of a block in bytes.
    
    - `current_network_hashrate_350` — unsigned int; network hashrate calculated by difficulty within a window of the last 350 blocks. 
        
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_CURRENT_NETWORK_HASHRATE_350` flag is present.
    
    - `current_network_hashrate_50` — unsigned int; the same as above for last 50 blocks. 
        
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_CURRENT_NETWORK_HASHRATE_50` flag is present.

    - `daemon_network_state` — unsigned int; current daemon state. Possible values and their meaning:
        
        ```
        daemon_network_state_connecting = 0,
        daemon_network_state_synchronizing = 1,
        daemon_network_state_online = 2,
        daemon_network_state_loading_core = 3,
        daemon_network_state_internal_error = 4,
        daemon_network_state_unloading_core = 5
        ```

    - `default_fee` — unsigned int; current default fee.

    - `grey_peerlist_size` — unsigned int; number of peers in the gray list (these are peers received from another node and this node has not yet attempted to connect to them).
    
    - `height` — unsigned int; number of blocks in the main chain.
    
    - `incoming_connections_count` — unsigned int; number of incoming P2P connections.
    
    - `last_block_size` — unsigned int; cumulative size of the last block. 
        
        - Returned only if `COMMAND_RPC_GET_INFO_FLAG_LAST_BLOCK_SIZE` flag is present.
    
    - `last_block_total_reward` — unsigned int; actual reward for the last block. 

        - Calculated only if both `COMMAND_RPC_GET_INFO_FLAG_POS_DIFFICULTY` and `COMMAND_RPC_GET_INFO_FLAG_TOTAL_COINS` flags are present.
    
    - `last_pos_timestamp` — unsigned int; timestamp of the last PoS block in the main chain. 
    
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_LAST_POS_TIMESTAMP` flag is present.
    
    - `last_pow_timestamp` — unsigned int; timestamp of the last PoW block in the main chain. 
    
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_LAST_POW_TIMESTAMP` flag is present.
    
    - `max_net_seen_height` — unsigned int; size of the longest chain among this node’s peers.
    
    - `mi` — object; the last received maintainer info message with recommended build versions from project maintainers. 
    
        - See below detailed description of `maintainers_info_external` object.
    
    - `minimum_fee` — unsigned int; current tx fee minimum required by tx pool.
    
    - `net_time_delta_median` — signed int; median of system time differences among this node’s peers. 
    
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_NET_TIME_DELTA_MEDIAN` flag is present.
    
    - `offers_count` — unsigned int; total number of market offers known to this node. 
    
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_PERFORMANCE` flag is present and no `--disable-market` CLI option was specified.

    - `outgoing_connections_count` — unsigned int; number of outgoing P2P connections.

    - `outs_stat` — object; brief output statistics.

    - `pos_allowed` — boolean; false if PoS blocks cannot be accepted yet, otherwise — true.

    - `pos_block_ts_shift_vs_actual` — signed int; the difference between the block timestamp and actual block timestamp for the last PoS block in the main chain.     
    
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_POS_BLOCK_TS_SHIFT_VS_ACTUAL` flag is present.

    - `pos_diff_total_coins_rate` — unsigned int; current ratio of PoS difficulty to total coins mined. 

        - Calculated only if both `COMMAND_RPC_GET_INFO_FLAG_POS_DIFFICULTY` and `COMMAND_RPC_GET_INFO_FLAG_TOTAL_COINS` flags are present.
    
    - `pos_difficulty` — unsigned int; difficulty for the next PoS block.
    
    - `pos_sequence_factor` — unsigned int; size of a continuous sequence of PoS blocks starting from the top block. 
    
        - Calculated only if `COMMAND_RPC_GET_INFO_FLAG_POS_SEQUENCE_FACTOR` flag is present.

    - `pow_difficulty` — unsigned int; difficulty for the next PoW block.
    
    - `pow_sequence_factor` — unsigned int; size of a continuous sequence of PoW blocks starting from the top block. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_POW_SEQUENcE_FACTOR`)
    
    - `seconds_for_10_blocks` — unsigned int; timestamp difference between the top block and the 10th from the top. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_SECONDS_FOR_10_BLOCKS`)
    
    - `seconds_for_30_blocks` — unsigned int; timestamp difference between the top block and the 30th from the top. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_SECONDS_FOR_30_BLOCKS`)
    
    - `synchronization_start_height` — unsigned int; size of the local blockchain when the synchronization process started for the first time after daemon start.
    
    - `synchronized_connections_count` — unsigned int; number of synchronized peers.
    
    - `total_coins` — unsigned int; number of emitted coins. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_TOTAL_COINS`)
    
    - `transactions_cnt_per_day` — unsigned int; number of non-coinbase transactions for the last 24 hours. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_TRANSACTIONS_DAILY_STAT`)
    
    - `transactions_volume_per_day` — unsigned int; total amount of non-miner transactions for the last 24 hours. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_TRANSACTIONS_DAILY_STAT`)
    
    - `tx_count` — unsigned int; total number of all non-coinbase transactions.
    
    - `tx_count_in_last_block` — unsigned int; number of non-coinbase transactions in the last block. 
    
        (Required flag: `COMMAND_RPC_GET_INFO_FLAG_TX_COUNT_IN_LAST_BLOCK`)
    
    - `tx_pool_size` — unsigned int; number of transactions in the tx pool.
    
    - `white_peerlist_size` — unsigned int; number of peers in the white list (total number of peers to which this node has ever been connected).

* Fields of maintainers_info_external object:

    `ver_major` — unsigned int; major build version from project maintainers.
    
    `ver_minor` — unsigned int; minor build version from project maintainers.
    
    `ver_revision` — unsigned int; revision build version from project maintainers.
    
    `mode` — unsigned int; maintainers info message type:

    ```
    #define ALERT_TYPE_CALM 1
    #define ALERT_TYPE_URGENT 2
    #define ALERT_TYPE_CRITICAL 3
    ```

!!! note

    Body Params

    _flags_ `int32` required

    - binary mask of flags combined together with bitwise-OR operation. Default: 0 (no flags)

## 15. _get_out_info_

`GET` http://127.0.0.1:11211/json_rpc/#015

* Looks up an output in the global outputs table by specified amount and output global index.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_out_info\",\"params\":{\"amount\":1000000,\"i\":1}}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Normal request `application/json`

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "out_no": 0,
    "status": "OK",
    "tx_id": "09237f99387faa47ca971ce6805534c012f29a513b1e95dd8279ba4e1d81a837"
  }
}
```

* Request a NON-existing output `text/plain`

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "out_no": 0,
    "status": "NOT FOUND",
    "tx_id": "0000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

* Outputs:

    `status` — string; "OK" if the output was found, "NOT FOUND" if the requested output was not found.

    `tx_id` — string; hash identifier of output's source transaction.

    `out_no` — unsigned int; output local index in its source transaction.

!!! note

    Body Params

    _amount_ `int32` required

    - output amount

    _i_ `int32` required

    - output global index

## 16. _get_multisig_info_

`GET` http://127.0.0.1:52521/json_rpc/#016

* Looks up multisig output by specified identifier.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_multisig_info\",\"params\":{\"ms_id\":\"8937e6b5d11b18e6bf55c213723aeb2203e0b8a8906d1ed662236e4bc070e604\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Normal request  `application/json`

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "out_no": 3,
    "status": "OK",
    "tx_id": "2df88a09b2d8b73a45824526c26e7f21836bbe0b111e1e8a6896c1a7fc8e03eb"
  }
}
```

* Request NON-existing output `text/plain`

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "out_no": 0,
    "status": "NOT FOUND",
    "tx_id": "0000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

* Outputs:
    
    `tx_id` — string; hash identifier of transaction, containing the given multisig output.

    `out_no` — unsigned int; local multisig output index in the transaction, starting from zero.

!!! note

    Body Params

    _ms_id_ `string` required

    - hash identifier of a multisig output

## 17. _get_all_alias_details_

`GET` http://127.0.0.1:52521/json_rpc/#017

* Returns all registered aliases.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_all_alias_details\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "aliases": [
      {
        "address": "ZxBvxQuPUah1BxopoeysUze4Z1G7Ts86uQNMTRrcMLrwRcpfqFtPinuXf7sMy7gLfYWXrRJnA692PC4nM7JJS5Tn1rQSmtgcG",
        "alias": "abcevox",
        "comment": "",
        "tracking_key": ""
      },
      {
        "address": "ZxCgpbN4yts1CAhXh9qxKbERsYoaZY972EBJKoiukhdp9tQSSbSjpE9a2g4KNSssJ8HiL4p2mjz77gTTDuvRFzhe26yyaPhgW",
        "alias": "alexandre",
        "comment": "",
        "tracking_key": ""
      },
      {
        "address": "ZxCVg1QXPr1UQNaBN3zkaVZdhUB4tbvMhRaXeWToyfm2Rpg76GWbeJZ7e1vL535JYXg7K5E7wj2w9YJwW38pvPwF34u7TX7cm",
        "alias": "alexbo",
        "comment": "",
        "tracking_key": ""
      },
      {
        "address": "ZxC7eRhz54PLZnaECze66aKuvSTSZa74DNjFfppy5SNV7aK1uzeNo1wKDAKyiCmU44K9vZ8TVLw3A86ALoj6k1Ra31trAhqNR",
        "alias": "zoinker",
        "comment": "",
        "tracking_key": ""
      }
    ],
    "status": "OK"
  }
}
```

* Outputs:

    `aliases` — array of `alias_rpc_details` objects.

    * alias_rpc_details object's fields description:

        `alias` — string; alias name.

        `address` — string; address of a corresponding wallet.

        `tracking_key` — string; hex-encoded secret view key (optional) of the wallet.

        `comment` — string; user-defined comment, made by alias owner (optional).

## 18. _get_aliases_

`GET` http://127.0.0.1:52521/json_rpc/#018

* Retrieves a specified range of aliases from the global list.

!!! warning

    Obsolete method!

    Not recommended for use.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_aliases\",\"params\":{\"offset\":0,\"count\":1}}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "aliases": [
      {
        "address": "eXBvxQuPUah1BxopoeysUze4Z1G7Ts86uQNMTRrcMLrwRcpfqFtPinuXf7sMy7gLfYWXrRJnA692PC4nM7JJS5Tn1rQSmtgcG",
        "alias": "abcevox",
        "comment": "",
        "tracking_key": ""
      }
    ],
    "status": "OK"
  }
}
```

* Outputs:

    `aliases` — array of alias_rpc_details objects; see get_all_alias_details method description for details.

!!! note

    Body Params
    
    _offset_ `int32` required

    - starting offset in global alias list

    _count_ `int32` required

    - how many elements to retrieve

## 19. _get_pool_txs_details_

`GET` http://127.0.0.1:52521/json_rpc/#019

* Returns transactions that are currently in the pool.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":\"0\",\"method\":\"get_pool_txs_details\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Requestiong all transactions `application/json`

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "txs": [
      {
        "amount": 500000000000000,
        "blob": "",
        "blob_size": 57913,
        "fee": 10000000000,
        "id": "5864f3e061c70b47d4ea6a47e593aad66bb2462fa4036be2cd3dfede9faa7583",
        "keeper_block": 0,
        "object_in_json": "",
        "pub_key": "24a523aea7d0b1e69e5a393a7e5531455c4317e527ad83be7a871fea716dc45c",
        "timestamp": 1558557359
      }
    ]
  }
}
```

* Requesting specific transactions

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "txs": [{
      <FULL DETAILED TX INFO for tx 01c5cf5128e4941e78c522d829b6e93277248567159498ad2576a677919c89e9>
    }]
  }
}
```

* Outputs:

    `txs` — array of tx_rpc_extended_info objects; see get_tx_details method description for details.

    Note: Output is less detailed if ids parameter is empty or omitted.

!!! note

    Body Params
    
    _ids_ `array of strings`

    - list of transaction hash identifiers for which information is requested. 
    
    - All transactions from the pool will be returned if ids is empty or if this parameter is omitted

## 20. _get_pool_txs_brief_details_

`GET` http://127.0.0.1:52521/json_rpc/#020

* Returns brief information for transactions currently in the pool.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_pool_txs_brief_details\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Requestiong all transactions `application/json`

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "txs": [
      {
        "fee": 10000000000,
        "id": "5864f3e061c70b47d4ea6a47e593aad66bb2462fa4036be2cd3dfede9faa7583",
        "sz": 57913,
        "total_amount": 500000000000000
      }
    ]
  }
}
```

* Requestion specific transactions `text/plain`

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "txs": [
      {
        "fee": 100000,
        "id": "5e7ce042556717c4b31b0c3dc744042289bfc058cf669f75ce76a48ca3f75bfd",
        "sz": 310,
        "total_amount": 49900000
      }
    ]
  }
}
```

* Outputs:

    `txs` — array of tx_rpc_brief_info objects.

    - tx_rpc_brief_info object's fields:

        `fee` — unsigned int; transaction fee.
        `id` — string; hash identifier.
        `sz` — unsigned int; size of serialized transaction in bytes (the same as blob_size in tx_rpc_extended_info).
        `total_amount` — unsigned int; sum of all transaction outputs.

!!! note

    Body Params

    _ids_ `array of strings`

    - list of transaction hash identifiers for which information is requested. 
    
    - All transactions from the pool will be returned if ids is empty or if this parameter is omitted.

## 21. _get_all_pool_tx_list_

`GET` http://127.0.0.1:52521/json_rpc/#021

* Returns IDs for all txs in the pool.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_all_pool_tx_list\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "ids": [
      "07af9af51abace52c6c9f5e96eac1f4123e56d8d2b2e1ac2ba5c6d68be94680f",
      "968d44f9443b067debc4a467174ad5b640690e165a2f8d45b2904d082bc1312e"
    ],
    "status": "OK"
  }
}
```

* Outputs:

    `ids` — array of strings; list of hash identifiers for all transactions that are currently in the pool.

## 22. _get_main_block_details_

`GET` http://127.0.0.1:52521/json_rpc/#022

* Returns block details for a specified identifier. Only for main chain blocks.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_main_block_details\",\"params\":{\"id\":\"0036D628535E31969BCABCA15BFC9ED7B7FB5B0A0CE88E7F84A299EC22EF1539\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

* Normal request `application/json`

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "block_details": {
      "actual_timestamp": 1558707712,
      "already_generated_coins": "17540409000000000000",
      "base_reward": 1000000000000,
      "blob": "",
      "block_cumulative_size": 0,
      "block_tself_size": 0,
      "cumulative_diff_adjusted": "47789840156373229",
      "cumulative_diff_precise": "29013538618324661",
      "difficulty": "1852005064583",
      "effective_fee_median": 100000000,
      "height": 23206,
      "id": "0036d628535e31969bcabca15bfc9ed7b7fb5b0a0ce88e7f84a299ec22ef1539",
      "is_orphan": false,
      "miner_text_info": "",
      "object_in_json": "...",
      "penalty": 0,
      "pow_seed": "",
      "prev_id": "83474d142764e00b033ae10c1c3697e526b1e7331743012122f3dfb365dbe792",
      "summary_reward": 1000000000000,
      "this_block_fee_median": 0,
      "timestamp": 1558707712,
      "total_fee": 0,
      "total_txs_size": 0,
      "transactions_details": [
        {
          "amount": 1000000000000,
          "blob": "",
          "blob_size": 100,
          "fee": 0,
          "id": "1f05b64404c05f028df9ae780abb5c0119c7f4df96d81002922bb67b1ebaa79d",
          "keeper_block": 23206,
          "object_in_json": "",
          "pub_key": "61c2122faf0854a2b6988859f693c1d6435055114e73f2a80ec1196035a044b9",
          "timestamp": 1558707712
        }
      ],
      "type": 1
    },
    "status": "OK"
  }
}
```

* Request a NON-existing block `text/plain`

```
{
  "error": {
    "code": -14,
    "message": "the requested block has not been found"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Outputs:

    `block_details` — block_rpc_extended_info object; see get_blocks_details method for more details.

!!! note

    Body Params
    
    _id_ `string` required

    - hash identifier for a block.

## 23. _get_alt_block_details_

`GET` http://127.0.0.1:52521/json_rpc/#023

* Returns block details for a specified identifier. Only for blocks in alternative chains.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_alt_block_details\",\"params\":{\"id\":\"5391963EB274AF8391FA89BC711122B5DB9B6C3703CB8865D45505F919F9842B\"}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "block_details": {
      "actual_timestamp": 1558705296,
      "already_generated_coins": "0",
      "base_reward": 1000000000000,
      "blob": "",
      "block_cumulative_size": 0,
      "block_tself_size": 0,
      "cumulative_diff_adjusted": "47701129767973676",
      "cumulative_diff_precise": "12602457701330728735458",
      "difficulty": "2053263042953598309",
      "effective_fee_median": 0,
      "height": 23138,
      "id": "5391963eb274af8391fa89bc711122b5db9b6c3703cb8865d45505f919f9842b",
      "is_orphan": true,
      "miner_text_info": "1.0.31[29c0487]",
      "object_in_json": "...",
      "penalty": 0,
      "pow_seed": "",
      "prev_id": "62c3d7a0a2f7d253b78a8e48dc0407e7d8f0d26b4fdb8b52687b07a7c4020ba9",
      "summary_reward": 1000000000000,
      "this_block_fee_median": 0,
      "timestamp": 1558705800,
      "total_fee": 0,
      "total_txs_size": 0,
      "transactions_details": [
        {
          "amount": 2000000000000,
          "blob": "",
          "blob_size": 203,
          "extra": [
            {
              "datails_view": "",
              "short_view": "7eaeb2490d85b17773b30ffe9f32fce50a1e89ee5ed258531e6b66ae13b00327",
              "type": "pub_key"
            },
            {
              "datails_view": "312e302e33315b323963303438375d",
              "short_view": "15 bytes",
              "type": "user_data"
            },
            {
              "datails_view": "",
              "short_view": "0 bytes",
              "type": "extra_padding"
            },
            {
              "datails_view": "cefd",
              "short_view": "cefd",
              "type": "XOR"
            },
            {
              "datails_view": "",
              "short_view": "height: 23148",
              "type": "unlock_time"
            },
            {
              "datails_view": "",
              "short_view": "timestamp: 1558705296 Fri, 24 May 2019 13:41:36 GMT",
              "type": "pos_time"
            }
          ],
          "fee": 0,
          "id": "a88f4ae2a89d5cbf9a76946785b6b45e898177cc0acd0b9c6ec5e35ccd73d9e8",
          "ins": [
            {
              "amount": 0,
              "kimage_or_ms_id": "",
              "multisig_count": 0
            },
            {
              "amount": 1000000000000,
              "global_indexes": [
                32159
              ],
              "kimage_or_ms_id": "5125de7598e723efba04d83258f31a3b30b21ed036f8e52b0669b12b93264267",
              "multisig_count": 0
            }
          ],
          "keeper_block": 0,
          "object_in_json": "",
          "outs": [
            {
              "amount": 1000000000000,
              "global_index": 0,
              "is_spent": false,
              "minimum_sigs": 0,
              "pub_keys": [
                "6acb06f7e4916d38ecafb537065e4dedaf0949fd49787fad55256770eaa8b029"
              ]
            },
            {
              "amount": 1000000000000,
              "global_index": 0,
              "is_spent": false,
              "minimum_sigs": 0,
              "pub_keys": [
                "c5179786faf5fe9f0d2adfcf6c9b069aae0e1393b771efddef8efade4bb4ce73"
              ]
            }
          ],
          "pub_key": "7eaeb2490d85b17773b30ffe9f32fce50a1e89ee5ed258531e6b66ae13b00327",
          "timestamp": 1558705296
        }
      ],
      "type": 0
    },
    "status": "OK"
  }
}
```

* Outputs:

    `block_details` — block_rpc_extended_info object; see get_blocks_details method for more details.

!!! note

    Body Params

    _id_ `string` required

    - hash identifier for a block.

## 24. _get_alt_blocks_details_

`GET` http://127.0.0.1:52521/json_rpc/#024

* Returns alternative blocks details for a specified range.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_alt_blocks_details\",\"params\":{\"offset\":0,\"count\":2}}}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "blocks": [{
      "actual_timestamp": 1537462404,
      "already_generated_coins": 0,
      "base_reward": 0,
      "blob": "",
      "block_cumulative_size": 0,
      ....
    },{
      "actual_timestamp": 1537462619,
      "already_generated_coins": 0,
      "base_reward": 0,
      "blob": "",
      "block_cumulative_size": 0,
      ....
    }],
    "status": "OK"
  }
}
```

* Outputs:

    `blocks` — array of block_rpc_extended_info objects; see get_blocks_details method for more details.

!!! note

    Body Params

    _offset_ `int32` required

    - starting offset in the global list of alternative blocks

    _count_ `int32` required

    - number of blocks to be requested

## 25. _reset_transaction_pool_

`POST` http://127.0.0.1:52521/json_rpc/#025

* Clears the transaction pool.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"reset_transaction_pool\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

## 26. _get_current_core_tx_expiration_median_

`GET` http://127.0.0.1:52521/json_rpc/#026

* Returns the median for timestamps of the last 20 blocks. Displayed as returned median value plus 600 seconds, this is used to check the expiration time of parameters.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_current_core_tx_expiration_median\"}" -H "content-type:application/json;" http://127.0.0.1:52521/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "expiration_median": 1537465582,
    "status": "OK"
  }
}
```

* Outputs:

    `expiration_median` — unsigned int; median value.

## 27. _marketplace_global_get_offers_ex_

`GET` http://127.0.0.1:52521/json_rpc/#027

* This main marketplace API, which lets to read "offers" from EvoX blockchain. It has diverse filters, which let specify particular parameters of the request and help organize effective communication on production.

!!! note

    Activate "Offers service" in daemon to use this API

    - To make daemon work with this API make sure you started daemon with `--enable_offers_service` command line parameter

* This main marketplace API method, which lets to read "offers" created and managed by given wallet. It has diverse filters, which let specify particular parameters of the request and help organize effective communication on production. 
* More detailed specification of the filter fields provided in [ " Filter " ](/api/marketplace-filter-structure-and-description/) structure defintion.
* Result returned as an array of `Offer` objects, which is described in [ " Offer " ](/api/marketplace-offer-structure-and-description/) structure.

**REQUEST**

```
curl --request GET \
     --url 'http://127.0.0.1:52521/json_rpc/#027' \
     --header 'Accept: application/json'
```

**RESPONSE**

* Response OK `application/json`

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "offers": [
      {
        "ap": "20",
        "at": "1",
        "b": "",
        "cat": "CLS:MAN:TSH",
        "cnt": "Skype: some_skype, discord: some_user#01012",
        "com": "Some nice comments about tshirt",
        "do": "Additional conditions",
        "et": 10,
        "fee": 10000000000,
        "index_in_tx": 0,
        "lci": "",
        "lco": "World Wide",
        "ot": 1,
        "p": "USD",
        "pt": "Credit cards, BTC, EVOX, ETH",
        "security": "0000000000000000000000000000000000000000000000000000000000000000",
        "t": "T-shirt with Evox logo, made by Crypjunkie",
        "timestamp": 1570219600,
        "tx_hash": "6ba12c5d2c66d31f770bfdc88ae9dc90d007b9b33f946fc7c1d9750f8655331c",
        "tx_original_hash": "0000000000000000000000000000000000000000000000000000000000000000"
      },
      {
        "ap": "20",
        "at": "1",
        "b": "",
        "cat": "CLS:MAN:TSH",
        "cnt": "Skype: some_skype, discord: some_user#01012",
        "com": "Some nice comments about tshirt",
        "do": "Additional conditions",
        "et": 10,
        "fee": 10000000000,
        "index_in_tx": 0,
        "lci": "",
        "lco": "World Wide",
        "ot": 1,
        "p": "USD",
        "pt": "Credit cards, BTC, EVOX, ETH",
        "security": "0000000000000000000000000000000000000000000000000000000000000000",
        "t": "T-shirt with Evox logo, made by Crypjunkie",
        "timestamp": 1570219840,
        "tx_hash": "2987b671cc337203628a3a1bb7ac811e41f110864d6162d3c2276d2c79f694d6",
        "tx_original_hash": "0000000000000000000000000000000000000000000000000000000000000000"
      }
    ],
    "status": "",
    "total_offers": 0
  }
}
```

* Response BAD `application/json`

```
{}
```
