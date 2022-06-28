## Offer's JSON structure and fields description

* Blockchain can keep hundreds of thousands of offers and fetching a particular set of offers need to have a filtering mechanism. 
* EvoX daemon has a built-in effective search engine, which can represents marketplace offers in a few different orders, with wide filtering options provided in this structure.

<br>

### JSON

```
od: {
      "offset": 0,
      "limit": 100,
      "amount_low_limit": 0,
      "amount_up_limit": 0,
      "category": "",
      "keyword": "",
      "location_city": "",
      "location_country": "",
      "offer_type_mask": 0,
      "order_by": 0,
      "primary": "",
      "rate_low_limit": "0.000000",
      "rate_up_limit": "0.000000",
      "reverse": false,
      "target": "",
      "timestamp_start": 0,
      "timestamp_stop": 0
    }
```

###  Fields description:

`order_by` - chose in how to order offers in selection. 

* At this moment supported following ordering:

| **_order_by_ value** |         **Order Type**                                                        |
|----------------------|-------------------------------------------------------------------------------|
|   0                  | Order by timestamp (most usable)                                              |
|   1                  | Order by an amount of EvoX                                                    |
|   2                  | Order by the amount of specified currency                                     |
|   3                  | Order by rate, which calculated as the amount currency divided to amount EvoX |
|   4                  | Order by payment type(as string)                                              |
|   5                  | Order by contact field(as string)                                             |
|   6                  | Order by location: country string concatenated with city string               |
|   7                  | Order by target string, basically title string                                |

- `reverse` - Reverse order

- `offset` - Offset regarding first item which fit specified filter, count include only items which fit the filter. 

    Userful for enumeration big amount or records, up to whole offers database enumeration.

- `limit` - Maximum records to return.

- `amount_low_limit` - filter offers selection by field amount of specified currency at lower boundary.

- `amount_up_limit` - filter offers selection by field amoun of specified currency t at higher boundary.

- `category` - fiter by category, work's as substring matching, 

    i.e. if categories set to `"CLS:MAN:TSH"` and filters category fileds set to `"MAN"` then it fits category condition.

- `keyword` - This use search by keyword throught the all fields.

- `location_city` - Used to filter by city name or geo-tag

- `location_country` - Filters by country code.

- `offer_type_mask` - Specify type of the offer:

|   Number   |        Offer Type                    |
|------------|--------------------------------------|
| 0x00000001 | Offer type 0 (buy currency for EvoX) |
| 0x00000002 | Offer type 1 (buy EvoX for currency) |
| 0x00000004 | Offer type 2 (buy goods for EvoX)    |
| 0x00000008 | Offer type 3 (sell goods for EvoX)   |

- `rate_low_limit` - Filter by low limit of the rate between EvoX and currency amount currency divided to amount EvoX)

- `rate_up_limit` - Filter by up limit of the rate between EvoX and currency amount currency divided to amount EvoX)

- `target` - Basically a title for subject of the Offer - could be the name of the goods or currency which supposed to be traded.

- `timestamp_start` - Setup a lower timestamp boundary. Useful if the offers are selecting for given time range.

- `timestamp_stop` - Setup a higher timestamp boundary. Useful if the offers are selecting for given time range.
