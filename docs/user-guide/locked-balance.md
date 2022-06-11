
* On occasion, you may notice that part of your wallet balance is temporarily locked.

* The reason behind it is complex, but let us offer a simplified explanation here.

For more details, please refer to the whitepaper.

* As you may know already there’s no such thing as account balance in the blockchain.
* Instead each wallet consists of inputs and outputs.
* Due to the nature of incoming transactions, the amount in a wallet balance can get fragmented.
* When you try to send a particular amount, your transaction will be split in outgoing and change amounts. 
* Here is example.

> 1. `A` has 100 coins and sends 50 to `B`
> 2. Transaction of 100 coins creates in the blockchain
> 3. 50 coins goes to `B` and 50 coins goes to `A`

* EvoX network architecture demands 10 confirmations for this change transaction and that amount gets locked in the wallet. 
* Therefore, lock time is 10 blocks behind or approximately 10 minutes.
