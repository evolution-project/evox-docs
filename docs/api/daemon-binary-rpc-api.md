## 35. _getblocks.bin_

`GET` http://127.0.0.1:52521/#035

* Retrieves blocks not present on requesting side. Used for wallet synchronization.

**REQUEST**

* _C++_

```
template<class t_block_complete_entry>
struct COMMAND_RPC_GET_BLOCKS_FAST_T
{
  struct request
  {
    std::list<crypto::hash> block_ids;
  };

  struct response
  {
    std::list<t_block_complete_entry> blocks;
    uint64_t    start_height;
    uint64_t    current_height;
    std::string status;
  };
};

typedef COMMAND_RPC_GET_BLOCKS_FAST_T<block_complete_entry> COMMAND_RPC_GET_BLOCKS_FAST;
```

* Outputs:

    `blocks` — list of block_complete_entry objects containing full information about each block.
    
    `start_height` — height of the first block in blocks.
    
    `current_height` — current total size of the blockchain.

!!! note

    Body Params

    _block_ids_ `array of strings` required

    - list of block IDs representing a snapshot of the local blockchain. 
    
    - The first 10 block IDs appear in sequential order, the next IDs in pow(2,n) offsets (2, 4, 8, 16, 32, 64, etc.), while the last are always the genesis block ID. See also tools::wallet2::get_short_chain_history().

## 36. _get_o_indexes.bin_

`GET` http://127.0.0.1:52521/#036

* Retrieves global output indexes by specified transaction ID.

**REQUEST**

* _C++_

```
struct COMMAND_RPC_GET_TX_GLOBAL_OUTPUTS_INDEXES
{
  struct request
  {
    crypto::hash txid;
  };

  struct response
  {
    std::vector<uint64_t> o_indexes;
    std::string status;
  };
};
```

* Outputs:
    
    `o_indexes` — vector of global indexes for each tx output.

!!! note

    Body Params

    _tx_id_ `string` required

    - transaction hash identifier

## 37. _getrandom_outs.bin_

`GET` http://127.0.0.1:52521/#037

* Retrieves random outputs from the entire blockchain for specified amounts. 
* Random outputs can be mixed with normal user's unspent outputs in a transaction to improve untraceability.

**REQUEST**

* _C++_

```
struct COMMAND_RPC_GET_RANDOM_OUTPUTS_FOR_AMOUNTS
{
  struct request
  {
    std::list<uint64_t> amounts;
    uint64_t            outs_count;
    bool                use_forced_mix_outs; 
  };

  struct out_entry
  {
    uint64_t global_amount_index;
    crypto::public_key out_key;
  };

  struct outs_for_amount
  {
    uint64_t amount;
    std::list<out_entry> outs;
  };

  struct response
  {
    std::vector<outs_for_amount> outs;
    std::string status;
  };
};
```

* Outputs:

    `outs` — vector of outs_for_amount objects.
    
    `outs_for_amount::amount` — specific amount for which random outputs were found.
    
    `outs_for_amount::outs` — list of out_entry objects.
    
    `out_entry::global_amount_index` — output index in the global outputs list.
    
    `out_entry::out_key` — output's public key.

!!! note

    Body Params

    _amounts_ `string` required

    - list of denominations for which random mix-ins are required

    _outs_count_ `int32` required

    - number of random outputs needed for each amount

## 38. _get_tx_pool.bin_

`GET` http://127.0.0.1:52521/#038

* Retrieves transaction pool.

**REQUEST**

* _C++_

```
struct COMMAND_RPC_GET_TX_POOL
{
  struct request
  {};
  struct response
  {
    std::list<blobdata> txs;
    std::string status;
 };
};
```

* Outputs:

    `txs` — list of serialized transactions from the pool.

## 39. _check_keyimages.bin_

`GET` http://127.0.0.1:52521/#039

* Checks specified key images for their spent status.

**REQUEST**

* _C++_

```
struct COMMAND_RPC_CHECK_KEYIMAGES
{
  struct request
  {
    std::list<crypto::key_image> images;
  };

  struct response
  {
    std::list<uint64_t> images_stat;  //true - unspent, false - spent
    std::string status;
 };
};
```

* Outputs:

    `images_stat` — list of integers where 0 means that corresponding key image is spent and 1 means it is not spent.

!!! note

    Body Params

    _images_ `string` required

    - list of key images to be checked for spending

## 40. _scan_pos.bin_

`GET` http://127.0.0.1:52521/#040

* Performs PoS minting iteration, i.e., scans the given PoS entries (set of unspent outputs) against the last blocks in the blockchain, and returns the output that won the minting process and corresponding PoS timestamp if successful.

**REQUEST**

* _C++_

```
struct COMMAND_RPC_SCAN_POS
{
  struct request
  {
    std::vector<pos_entry> pos_entries;
  };

  struct response
  {
    std::string status;
    uint64_t index;
    uint64_t block_timestamp;
    ...
  };
};
```

* Outputs:

    `index` — index of given PoS entry, that was selected by minting process.

    `block_timestamp` — timestamp for PoS block.

!!! note

    Body Params

    _pos_entries_ `string` required

    - vector of pos_entries objects. Unspent user outputs that participate in PoS minting.

## 41. _get_pos_details.bin_

`GET` http://127.0.0.1:52521/#041

* Retrieves PoS minting context details. Required to build a PoS block.

**REQUEST**

* _C++_

```
struct COMMAND_RPC_GET_POS_MINING_DETAILS
{    
  struct request
  {  };

  struct response
  {
    stake_modifier_type sm;
    uint64_t starter_timestamp;
    std::string pos_basic_difficulty;
    std::string status;
    crypto::hash last_block_hash;
    bool pos_mining_allowed;
  };
};
```

* Outputs:

    `sm` — prepared current stake modifier object.
    
    `starter_timestamp` — current minimum allowed timestamp for a PoS block.
    
    `pos_basic_difficulty` — current PoS difficulty.
    
    `last_block_hash` — hash identifier of the top block in the blockchain.
    
    `pos_mining_allowed` — true if PoS minting is currently allowed.
