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

`GET` http://127.0.0.1:12233/json_rpc/#044

* Creates a transaction and broadcasts it to the network.