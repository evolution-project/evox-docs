## 28. _getheight_

`GET` http://127.0.0.1:52521/#028

* Returns the current blockchain height.

**REQUEST**

```
curl -H "content-type:application/json;" http://127.0.0.1:52521/getheight
```

**RESPONSE**

```
{
  "height": 32317,
  "status": "OK"
}
```

* Outputs:

    `height` — unsigned int; length of the longest chain (equal to top block height plus one).

## 29. _gettransactions_

`GET` http://127.0.0.1:52521/#029

* Retrieves transactions in serialized binary form by specified tx IDs.

**REQUEST**

```
curl --data "{\"txs_hashes\":[\"0001020304050607080900111213141516171819202122232425262728293031\", \"cd7a0dfb6f69c5331fe958390c38e7770e217414a6f2341c2b56d6e38b9db54b\"]}" -H "content-type:application/json;" http://127.0.0.1:52521/gettransactions
```

***RESPONSE**

```
{
  "missed_tx": [
    "0001020304050607080900111213141516171819202122232425262728293031"
  ],
  "status": "OK",
  "txs_as_hex": [
    "010100eb85020308038df495b03458b358456f2b70fd9df3924b08f2723941c31cdfb75dda0fd91087004603a7b5d2860b31bbd95fec5bc49ad70659365d3f1fa50ab1c48d2af23fa07a669200904e03a0fafca182510f67a33b7e239a59e4e2e947f29eb1c5969933f3774bc0ea6004000516073cbb9f393d493bef6c87b5766426ef2645dd3805dbcb2f81caa171e8488805130a73696e6761706f72653215001777580ef585020000"
  ]
}
```

* Outputs:

    `missed_tx` — array of strings; hex-encoded identifiers of transactions that were not found.

    `txs_as_hex` — array of strings; found transactions serialized to binary format.

!!! note

    Body Params

    _txs_hashes_ `array of strings` required

    - hex-encoded transactions identifiers to be retrieved

## 30. _sendrawtransaction_

`POST` http://127.0.0.1:52521/#030

* Sends raw transaction (i.e., fully constructed and serialized beforehand) to the network.

**REQUEST**

```
curl --data "{\"tx_as_hex\":\"01010180897_TX_DATA_0000ff00000\"}" -H "content-type:application/json;" http://127.0.0.1:52521/sendrawtransaction
```

**RESPONSE**

* Normal Sending `application/json`

```
{
  "status": "OK"
}
```

* Transaction was NOT send for some reason `text/plain`

```
{
  "status": "Failed"
}
```

!!! note

    Body Params

    _tx_as_text_ `string` required

    - hex-encoded serialized transaction.

## 31. _force_relay_

`POST` http://127.0.0.1:52521/#031

* Broadcasts specified transactions across the network.

**REQUEST**

```
curl --data "{\"tx_as_hex\":\"01010180897_TX_DATA_0000ff00000\"}" -H "content-type:application/json;" http://127.0.0.1:52521/force_relay
```

**RESPONSE**

```
{
  "status": "OK"
}
```

!!! note

    Body Params

    _txs_as_hex_ `array of strings` required

    - hex-encoded serialized transactions to be broadcasted

## 32. _start_mining_

`POST` http://127.0.0.1:52521/#032

* Starts mining in daemon.

**REQUEST**

```
curl --data "{\"miner_address\":\"eXBvJDuQjMG9R2j4WnYUhBYNrwZPwuyXrC7FHdVmWqaESgowDvgfWtiXeNGu8Px9B24pkmjsA39fzSSiEQG1ekB225ZnrMTBp\"}" -H "content-type:application/json;" http://127.0.0.1:52521/start_mining
```

**RESPONSE**

```
{
  "status": "OK"
}
```

!!! note

    Body Params

    _miner_address_ `string` required

    - address for receiving mined coins

    _thread_count_ `int32` required

    - number of threads allocated for the miner

## 33. _stop_mining_

`POST` http://127.0.0.1:52521/#033

* Stops mining in daemon.

**REQUEST**

```
curl -H "content-type:application/json;" http://127.0.0.1:52521/stop_mining
```

**RESPONSE**

```
{
  "status": "OK"
}
```

## 34. _getinfo_

`GET` http://127.0.0.1:52521/#034

* Legacy version of JSON RPC getinfo call. Please refer to its description for more information.

**REQUEST**

```
curl -H "content-type:application/json;" http://127.0.0.1:52521/getinfo
```
