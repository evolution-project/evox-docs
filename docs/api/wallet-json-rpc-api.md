## 42. _getbalance_

`GET` http://127.0.0.1:12233/json_rpc/#042

* Retrieves current wallet balance: total and unlocked.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getbalance\"}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "balance": 4631800000,
    "unlocked_balance": 4431900000
  }
}
```

* Outputs:

    `balance` — unsigned integer; total fund, that the wallet has (unlocked and locked coins).
    
    `unlocked_balance` — unsigned integer; unlocked funds. 
    
    - i.e. coins that are stored deep enough in the blockchain to be considered relatively safe to spend. 
    
    - Only this many coins are immediately spendable. Unlocked_balance is always less or equal to balance.

## 43. _getaddress_

`GET` http://127.0.0.1:12233/json_rpc/#043

* Obtains wallet’s public address.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"getaddress\"}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "address": "eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp"
  }
}
```

* Outputs:

    `address` — string; standard public address of the wallet.

## 44. _transfer_

`POST` http://127.0.0.1:12233/json_rpc/#044

* Creates a transaction and broadcasts it to the network.

!!! note

    `transfer_destination` object fields:

    `address` — string; standard or integrated address of a recipient.

    `amount` — unsigned int; amount of coins to be sent;

!!! warning

    Integrated address usage

    - If you use multiple addresses in destinations field, make sure there are maximum 1 integrated address involved, or, if "payment id" parameter was specified, then integrated addresses are not allowed.

**REQUEST**

```
curl --data-binary '"{"jsonrpc":"2.0","id":0,"method":"transfer","params":{"destinations":[ {"address":"eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp", "amount":10000000000}, {"address":"eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBq", "amount":20000000000}], "fee":10000000000, "mixin":0}}' -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

* Correct response 200 `application/json`

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "tx_hash": "b329cce92a23fdaf89a5ad907ca9c4c1fbd052b79ec8414438533c83b39afc2b",
    "tx_unsigned_hex": ""
  }
}
```

* Response 400 "Not Enough Money" `text/plain`

```
{
  "error": {
    "code": -4,
    "message": "NOT_ENOUGH_MONEY"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Response 400 "Too Small Fee" `text/plain`

```
{
  "error": {
    "code": -4,
    "message": "transaction was rejected by daemon"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Outputs:

  `tx_hash` — string; hash identifier of the transaction that was successfully sent.

  `tx_unsigned_hex` — string; hex-encoded unsigned transaction (for watch-only wallets; to be used in cold-signing process).

!!! note

    Body Params

    _destinations_ `string` required

    - list of `transfer_destination` objects (see below); list of recipients with corresponding amount of coins for each.

    _fee_ `int32` required

    - transaction fee in atomic units. Minimum: 10^10 atomic units.

    _mixin_ `int32` required

    - number of foreign outputs to be mixed in with each input. Increases untraceability. Specify zero for direct and traceable transfers.

    _payment_id_ `string`

    - hex-encoded payment id. Can be empty if payment ID is not required for this transfer.

    _comment_ `string`

    - text commentary which follow the transaction in encrypted form and is visible only to the sender and the receiver

## 45. _store_

`POST` http://127.0.0.1:12233/json_rpc/#045

* Saves wallet update progress into a wallet file. 

* Although progress is always saved upon graceful wallet application termination, with this call a user can manually trigger saving process. 

* Otherwise, in a case of abnormal wallet application termination the progress won’t be saved and it will take some time to synchronize on the next launch.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"store\"}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {}
}
```

## 46. _get_payments_

`GET` http://127.0.0.1:12233/json_rpc/#046

* Gets list of incoming transfers by a given payment ID.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_payments\",\"params\":{\"payment_id\":\"PAYMENT_ID\"}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "payments": [
      {
        "amount": 100000000,
        "block_height": 202556,
        "tx_hash": "01220e8304d46b940a86e383d55ca5887b34f158a7365bbcdd17c5a305814a93",
        "unlock_time": 0
      }
    ]
  }
}
```

* Outputs:

  `result` — list of payments object.

  `payments` object fields:

  `amount` — unsigned int; amount of coins in atomic units.

  `block_height` — unsigned int; height of the block containing corresponding transaction.
  
  `tx_hash` — string; transaction’s hash.
  
  `unlock_time` — unsigned int; if nonzero — unix timestamp since then this transfer’s coins can be spent. 
  
  - If it is less than 500000000, the value is treated as a minimum block height at which this transfer’s coin can be spent.

!!! note

    Body Params

    _payment_id_ `string` required

    - hex-encoded payment ID

    _allow_locked_transactions_ `boolean`

    - if set to `false` (default value) transactions with `unlock_time` value higher than current block height + 10 will not be included. 
    
    - If set to `true` parameter `unlock_time` will be ignored during validation. 
    
    - Using it can be potentially `dangerous`, since payments can be accepted but cannot be spent due to locked status.

## 47. _get_bulk_payments_

`GET` http://127.0.0.1:12233/json_rpc/#047

* Gets list of incoming transfers by given payment IDs.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"get_bulk_payments\",\"params\":{\"payment_ids\":[\"PAYMENT_ID_1\",\"PAYMENT_ID_2\"]}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "payments": [
      {
        "amount": 100000000000000,
        "block_height": 131944,
        "payment_id": "PAYMENT_ID_1",
        "tx_hash": "176416cb542884e10f826627f87df6cf45a16039f913deb2e41f5f2d0647a96d",
        "unlock_time": 0
      }
    ]
  }
}
```

* Outputs:

  `payments` — list of payment_details object (see [get_payments](/api/wallet-json-rpc-api/#46-get_payments) for details).

!!! note

    Body Params

    _payment_ids_ `array of strings` required

    - hex-encoded payment IDs.

    _min_block_height_ `int32` required

    - minimum block height.

    _allow_locked_transactions_ `boolean`

    - if set to `false` (default value) transactions with `unlock_time` value higher than current block height + 10 will not be included. 
  
    - If set to `true`  parameter `unlock_time` will be ignored during validation. 

    - Using it can be potentially `dangerous`, since payments can be accepted but cannot be spent due to locked status.

## _search_for_transactions_

`POST` http://127.0.0.1:12233/json_rpc/

* Gets list of incoming transfers by a given payment ID.

**REQUEST**

```
 curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0", "id":"0", "method":"search_for_transactions", "params":{  "in":true, "out":true, "tx_id":"e46a101fede26cdf97e03003b35448a470f18fdf7325dc4f0f97a97441c50572"   }}'
```

**RESPONSE**

```
{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "in": [
      {
        "amount": 100000000000000,
        "comment": "",
        "fee": 10000000000,
        "height": 301864,
        "is_income": true,
        "is_mining": false,
        "is_mixing": false,
        "is_service": false,
        "payment_id": "",
        "show_sender": false,
        "td": {
          "rcv": [
            100000000000000
          ]
        },
        "timestamp": 1586263122,
        "tx_blob_size": 314,
        "tx_hash": "e46a101fede26cdf97e03003b35448a470f18fdf7325dc4f0f97a97441c50572",
        "tx_type": 0,
        "unlock_time": 0
      }
    ]
  }
}
```

* Outputs:

  `in` — list of _wallet_transfer_info_ objects for incoming transactions;

  `out` — list of _wallet_transfer_info_ objects for outgoing transactions;

  `pool` — list of _wallet_transfer_info_ objects for unconfirmed transactions from tx pool;

  - _wallet_transfer_info_ object fields:

  `amount` — integer; amount of coins in atomic units;
    
  `comment` — string; an optional comment set by the sender;
    
  `fee` — integer; transaction fee in atomic units;
  
  `height` — integer; height of the block containing corresponding transaction;
  
  `is_income` — Boolean; _true_ if this is incoming transfer;
  
  `is_mining` — Boolean; _true_ if this is a miner (i.e. coinbase) tx;
    
  `is_mixing` — Boolean; _true_ if this tx is using mixins;
  
  `is_service` — Boolean; _true_ if this is a special service tx, not a normal one;
  
  `payment_id` — string; (optional) hex-encoded payment identifier;
  
  `show_sender` — Boolean; _true_ if sender address info is present;
  
  `remote_addresses` — list of strings; (optional) sender address(es);
  
  `recipients_aliases` — list of strings; (optional) used aliases of the recipient;
  
  `td` — a _wallet_transfer_info_details_ object;
  
  `timestamp` — integer; Unix timestamp when the tx was received;
  
  `tx_hash` — string; transaction’s hash.
  
  `unlock_time` — unsigned int; if nonzero — unix timestamp since then this transfer’s coins can be spent. 
  
  - If it is less than 500000000, the value is treated as a minimum block height at which this transfer’s coin can be spent.

  _wallet_transfer_info_details_ object fields:

  `rcv` — list of integers; received amounts in atomic units;
  
  `spn` — list of integers; spent amounts in atomic units;

!!! note

    Body Params

    _tx_id_ `string`

    - hash of a transaction, if specified then only that tx will be returned (if it passes the filters)

    _in_ `boolean`

    - if true then incoming transactions will be taken into account

    _out_ `boolean`

    - if true then outgoing transactions will be taken into account

    _pool_ `boolean`

    - if true, unconfirmed transactions from the pool will be taken into account as well

    _filter_by_height_ `boolean`

    - if true, transactions will be filtered by block height using min_height and max_height

    _in_height_ `int32`

    - minimum block height (including)

    _max_height_ `int32`

    - maximum block height (including)

## 48. _make_integrated_address_

`POST` http://127.0.0.1:12233/json_rpc/#048

* Creates an integrated address for the wallet by embedding the given payment ID together with the wallet's public address.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"make_integrated_address\",\"params\":{\"payment_id\":\"00000000FF00ff00\"}}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

* Correct Payment ID response 200 `application/json`

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "integrated_address": "eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp",
    "payment_id": "00000000ff00ff00"
  }
}
```

* Empty Payment ID response 200

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "integrated_address": "eXC4TEzP31KXoBgiyzprRQH1ZkBdshu61GeK7x38MAbbgp9PvXYw6Uphd5yK1XEtbzZaZMsjzTVEwRVXJjH6o6hm22U8CbESq",
    "payment_id": "c2c4aaeac1485777"
  }
}
```

* Invalid Payment ID response 400

```
{
  "error": {
    "code": -5,
    "message": "invalid payment id given: ' !@&#*', hex-encoded string was expected"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Outputs:

  `integrated_address` — string; the result.

  `payment_id` — string; hex-encoded payment ID, that was used (useful if an empty `payment_id` was given as an input).

!!! note

    Body Params

    _payment_id_ `string`

    - hex-encoded payment identifier. 
    
    - If empty, random 8-byte payment ID will be generated and used.

## 49. _split_integrated_address_

`POST` http://127.0.0.1:12233/json_rpc/#049

* Creates an integrated address for the wallet by embedding the given payment ID together with the wallet's public address.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"split_integrated_address\",\"params\":{\"integrated_address\":\"eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp\"}}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

* Valid Standard Address response 200

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "payment_id": "",
    "standard_address": "eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp"
  }
}
```

* Valid Integrated Address response 200

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "payment_id": "00000000ff00ff00",
    "standard_address": "eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp"
  }
}
```

* Invalid Integrated Address response 400

```
{
  "error": {
    "code": -2,
    "message": "invalid integrated address given: '!k9s02j23n'"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Outputs:

  `standard_address` — string; standard address with no payment ID attached

  `payment_id` — string; hex-encoded payment ID, extracted from the given integrated address. 
  
  - Can be empty. Will be empty when a standard address is given as an input.

!!! note

    Body Params

    _integrated_address_ `string` required

    - integrated or standard address

## 50. _sign_transfer_

`POST` http://127.0.0.1:12233/json_rpc/#050

* Signs a transaction prepared by watch-only wallet (for cold-signing process).

!!! note

    This method requires spending private key and can't be served by watch-only wallets.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"sign_transfer\",\"params\":{\"tx_unsigned_hex\":\"00_LONG_HEX_00\"}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "tx_hash": "855ae466c59b24295152740e84d7f823eaf3c91adfb1ba7b4ff1dc6085b79e63",
    "tx_signed_hex": "00_LONG_HEX_00"
  }
}
```

* Outputs:

  `tx_hash` — string; hash identifier of signed transaction.
  
  `tx_signed_hex` — string; hex-encoded signed transaction.

!!! note

    Body Params

    _tx_unsigned_hex_ `string` required

    - hex-encoded unsigned transaction as returned from transfer call.

## 51. _submit_transfer_

`POST` http://127.0.0.1:12233/json_rpc/#051

* Broadcasts transaction that was previously signed using sign_transfer call.

!!! note

    This method is designed for using with watch-only wallets that are unable to sign transactions by themselves.

**REQUEST**

```
curl --data "{\"jsonrpc\":\"2.0\",\"id\":0,\"method\":\"submit_transfer\",\"params\":{\"tx_signed_hex\":\"00_LONG_HEX_00\"}" -H "content-type:application/json;" http://127.0.0.1:12233/json_rpc
```

**RESPONSE**

* Transaction Successfully Sent response 200

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "tx_hash": "0554849abdb62f7d1902ddd14ce005722a340fc14fab4a375adc8749abf4e10b"
  }
}
```

* Transaction Was Rejected for some reason, response 400

```
{
  "error": {
    "code": -4,
    "message": "transaction was rejected by daemon"
  },
  "id": 0,
  "jsonrpc": "2.0"
}
```

* Outputs:

  `tx_hash` — string; transaction hash identifier.

!!! note

    Body Params

    _tx_signed_hex_ `string` required

    - hex-encoded signed transaction as returned from sign_transfer call.

## 52. _marketplace_push_offer_

`GET` http://127.0.0.1:12233/json_rpc/#052

* Broadcasts transaction that was previously signed using sign_transfer call.

* This method creates a new offer on marketplace service. More details in [Offer](/api/marketplace-offer-structure-and-description/)  structure.

**REQUEST**

```
curl --request GET \
     --url 'http://127.0.0.1:12233/json_rpc/#052' \
     --header 'Accept: application/json'
```

**RESPONSE**

* Transaction Send Successfully response 200

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "tx_blob_size": 549,
    "tx_hash": "2987b671cc337203628a3a1bb7ac811e41f110864d6162d3c2276d2c79f694d6"
  }
}
```

## 53. _marketplace_push_update_offer_

`GET` http://127.0.0.1:12233/json_rpc/#053

* This method update marketplace offer details

**REQUEST**

```
curl --request GET \
     --url 'http://127.0.0.1:12233/json_rpc/#053' \
     --header 'Accept: application/json'
```

**RESPONSE**

* Transaction Send Successfully response 200
```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "tx_blob_size": 725,
    "tx_hash": "06da9bac0f15fd7ab41983f9437f95835b1baef6810fe15b2ea831f60b058b4b"
  }
}
```

* This method updates an active offer, by providing proper proof that new details specified by the owner of the original offer posting.

* It has basically three parameters inside `JSON` body:

    `tx_id`- id of the transaction with original offer posting (returned in `marketplace_push_offer`). 
    
    - Basically offers identified by carier transactions id. 
    
    - Theoretically, one transaction can carry more than one offer, so the there is a second parameter which specifies an index of the offer inside carrier transaction, but since we didn't want to make the user interface and whole system way too complicated for using, by default API place only one offer per transaction.

    `no` - this parameter is 0 by default, must be used if transaction carry more then on offer.
    
    `od` - this is new offer details, specified as [Offer](/api/marketplace-offer-structure-and-description/) structure.

## 54. _marketplace_cancel_offer_

`GET` http://127.0.0.1:12233/json_rpc/#054

* Mark offer as NOT active.

**REQUEST**

```
curl --request GET \
     --url 'http://127.0.0.1:12233/json_rpc/#054' \
     --header 'Accept: application/json'
```

**RESPONSE**

* Transaction Successfully Send response 200

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "tx_blob_size": 368,
    "tx_hash": "d52014dae0b65168e0551acef9e95972041f3f38d92455d18c8b886baece3d90"
  }
}
```

* This method mark offer as innactive, by providing proper proof that action performed by the owner of the original offer posting.

* It has basically two parameters inside JSON body:

    `tx_id` - id of the transaction with original offer posting ( returned in `marketplace_push_offer` ). 
    
    - Basically offers identified by carrier transactions id. 
    
    - Theoretically, one transaction can carry more than one offer, so then there is a second parameter which specifies an index of the offer inside carrier transaction, but since we didn't want to make the user interface and whole system way too complicated for using, by default API place only one offer per transaction.

    `no` - this parameter is 0 by default, must be used if transaction carries more then on offer.

## 55. _marketplace_get_offers_ex_

`GET` http://127.0.0.1:12233/json_rpc/#055

* General marketplace API which lets read offers related to given wallet

**REQUEST**

```
curl --request GET \
     --url 'http://127.0.0.1:12233/json_rpc/#055' \
     --header 'Accept: application/json'
```

**RESPONSE**

* Transaction Successfully Send response 200

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
        "t": "T-shirt with EvoX logo, made by Crypjunkie",
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
        "t": "T-shirt with EvoX logo, made by Crypjunkie",
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

* This main marketplace API method, which lets to read "offers" created and managed by given wallet. 

* It has diverse filters, which let specify particular parameters of the request and help organize effective communication on production. 

* More detailed specification of the filter fields provided in Filter structure defintion.

* Result returned as an array of `Offer` objects, which described in [Offer](/api/marketplace-offer-structure-and-description/) structure.

## 72. _get_recent_txs_and_info_

`GET` http://127.0.0.1:12233/json_rpc/#072

* Api for fetching recent transactions history. 

* To keep history reading consistent, better to set `offset` parameter from last processed tx's `transfer_internal_index`.

* Below is typical request body:

```
{
  "jsonrpc": "2.0",
  "id": 0,
  "method": "get_recent_txs_and_info",
  "params": {
    "offset": 0,
    "update_provision_info": true,
    "exclude_mining_txs": true,
    "count": 100,
    "order": "FROM_BEGIN_TO_END",
    "exclude_unconfirmed": true
  }
}
```

* Request params description:

  `offset`: - internal wallet's index of transfer (every transfer has `transfer_internal_index` field, which simply index of transfer).

  `update_provision_info`: - true if need to update balance (could be disable for performance matters)

  `exclude_mining_txs`: - filter mining transactions

  `count`: - number transactions to fetch

  `order`: - Enumeration direction, could be `FROM_BEGIN_TO_END`, `FROM_END_TO_BEGIN`

  `exclude_unconfirmed`: - true if unconfirmed transactions not needed

**REQUEST**

```
curl --request GET \
     --url 'http://127.0.0.1:12233/json_rpc/#072' \
     --header 'Accept: application/json'
```

**RESPONSE**

```
{
  "id": 0,
  "jsonrpc": "2.0",
  "result": {
    "last_item_index": 72,
    "pi": {
      "balance": 2260000000000,
      "curent_height": 1623835,
      "transfer_entries_count": 96,
      "transfers_count": 3,
      "unlocked_balance": 2260000000000
    },
    "total_transfers": 3,
    "transfers": [
      {
        "amount": 1000000000000,
        "comment": "",
        "fee": 10000000000,
        "height": 1131972,
        "is_income": true,
        "is_mining": false,
        "is_mixing": false,
        "is_service": false,
        "payment_id": "",
        "remote_addresses": [
          "eXCD4JQoUw6MD343aKyJx2Zx44fdkc2r22rwULfcBDrAKyfcqYPNjiFKfnXVyRcHgMLdJLrhmmvN4ViRBDfanhLZ1EdqY8vbk"
        ],
        "show_sender": false,
        "td": {
          "rcv": [
            1000000000000
          ]
        },
        "timestamp": 1625569494,
        "transfer_internal_index": 0,
        "tx_blob_size": 1225,
        "tx_hash": "b4f6335a3d476629448aad0cbb5a56cbd36ea60d00dcfdb79b501d3f2d4abede",
        "tx_type": 0,
        "unlock_time": 0
      },
      {
        "amount": 1000000000000,
        "comment": "",
        "fee": 10000000000,
        "height": 1131972,
        "is_income": true,
        "is_mining": false,
        "is_mixing": false,
        "is_service": false,
        "payment_id": "",
        "remote_addresses": [
          "eXCD4JQoUw6MD343aKyJx2Zx44fdkc2r22rwULfcBDrAKyfcqYPNjiFKfnXVyRcHgMLdJLrhmmvN4ViRBDfanhLZ1EdqY8vbk"
        ],
        "show_sender": false,
        "td": {
          "rcv": [
            1000000000000
          ]
        },
        "timestamp": 1625569494,
        "transfer_internal_index": 1,
        "tx_blob_size": 1226,
        "tx_hash": "0a7551887a82f893aedfe72aa32189a84743d0044d47b05a5000a2a08ce791a3",
        "tx_type": 0,
        "unlock_time": 0
      },
      {
        "amount": 0,
        "comment": "",
        "fee": 10000000000,
        "height": 1555055,
        "is_income": false,
        "is_mining": false,
        "is_mixing": false,
        "is_service": true,
        "payment_id": "",
        "recipients_aliases": [
          "testtest"
        ],
        "remote_addresses": [
          "eXDEMMwyGBE1JE1b5pYH4vExnTeFfN3gMeiC1wb7n5dVC43oLHAJXkq5pmiZqRMegTi4LzepmddWWAiUZBc44HJL2iStSuRPV"
        ],
        "show_sender": false,
        "td": {
          "spn": [
            10000000000
          ]
        },
        "timestamp": 1651003337,
        "transfer_internal_index": 2,
        "tx_blob_size": 324,
        "tx_hash": "78695ec5cd55bc507955c53dcca11a08d13d91498d5edfd2b61415783f23c133",
        "tx_type": 5,
        "unlock_time": 0
      }
    ]
  }
}
```

* It is recommended to validate each transfer height against `curent_height` in response, to make sure that transfer got needed number of confirmations.

* Each next call of the `get_recent_txs_and_info` should be done with `offset`, taken from last transfer returned from previous call of `get_recent_txs_and_info`, with such call first returned transaction should be the same as it was in previous call, and to make sure that there were no split or chain swithch, the best practice would be to double check that id of the first returned `TX` from latest call match with `ID` of the last transactions from previous call.

* `Pseudocode` for work with this API might look like this:

* _C++_

```
#define NATIVE_CONFIRMATIONS_NEEDED 40
int index_in_wallet = 0;
last_tx_hash = nullhash;
while(true)
{
    
    req = {};
    req.offset = index_in_wallet;
    req.update_provision_info = true;
    req.exclude_mining_txs = true;
    req.count = BUNCH_OF_TRANSACTIONS_TO_FETCH;
    req.order = ORDER_FROM_BEGIN_TO_END;
    req.exclude_unconfirmed = true;

    get_transactions_history(req, resp);
 
    //check that last tx match
    if (resp.transfers.size() && last_tx_hash != nullhash  && resp.transfers[0].tx_hash != last_tx_hash)
    {
      //log problems 
      return false;
    }

    //regular synchronization
    for (int i = 0; i < resp.transfers.size(); i++)
    {
      if (resp.pi.curent_height - resp.transfers[i].height < NATIVE_CONFIRMATIONS_NEEDED)
      {
        //don't even read blocks with smaller  confirmation ration than expected
        break;
      }

      if (resp.transfers[i].is_income)
      {
        if(resp.transfers[i].payment_id)
        {
            db.increase_user_balance(payment_id, resp.transfers[i].amount);
        }     
      }
      last_tx_hash = resp.transfers[0].tx_hash;
      index_in_wallet = resp.transfers[i].transfer_internal_index;
    }
    sleep(10000); //sleep for 10 seconds
}
```

* BAD Response 400

```
{}
```