## Offer's JSON structure and fields description

* Here is a typical offer, that can be pushed to the EvoX blockchain-based marketplace.

* The structure of the offer is quite flexible, it's defined in `JSON` text format with this very shorten names of the fields, and kept gzipped inside transaction attachment, to safe space. 

* Offers not stay forever in blockchain, since it would be a waste of space, after transactions, which carry offers, will pass the checkpoints, it will be pruned, same way as signatures get pruned from tx.

### JSON

```
od: {
      "ap": "20",
      "at": "1",
      "cat": "CLS:MAN:TSH",
      "cnt": "Skype: some_skype, discord: some_user#01012",
      "com": "A comment about tshirt",
      "do": "Additional conditions",
      "et": 10,
      "fee": 10000000000,
      "lci": "",
      "lco": "World Wide",
      "ot": 1,
      "pt": "Credit cards, BTC, EVOX, ETH",
      "t": "T-shirt with Evox logo"
        }
```

### Fields description:

`ot` - Offer type 
    
```    
    0 - buy currency for EvoX, 
    1 - buy EvoX for currency, 
    2 - buy goods for EvoX, 
    3 - sell goods for EvoX
```

- `ap` - the amount of the currency specified for use in this offer

- `at` - the amount of the items to be sold/bought

- `cat` - Category of the goods, could be specified with subcategories by separation CLS:MAN:TSH, which could mean Clothes->Man->Tshirts

- `cnt` - Contacts, like skype, discord, telegram, whatever

- `com` - Comments regarding this offer

- `do` - Additional conditions, if need to specify

- `et` - Expiration time, set in days, eg 5 - expire in 5 days after creation

- `fee` - Fee paid for this transaction with the offer, it can be default offer, but the higher fee may bring offers to be higher in search results

- `lco` - Location country, if this makes sense for an offer

- `lci` - Location city, if the also make sense for an offer, could be google geo-autocomplete id, like ChIJD7fiBh9u5kcRYJSMaMOCCwQ

- `pt` - Payment type, Credit cards, Crypto, Paypal, Flexa

- `t` - Description for the goods/service which is selling/seeking
