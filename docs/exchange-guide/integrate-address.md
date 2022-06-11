## Integrated addresses for exchanges

### **Starting the daemon and the wallet application as RPC server**

* Unlike Bitcoin, CryptoNote family coins have different, more effective approach on how to handle user deposits.

* An exchange generates only one address for receiving coins and all users send coins to that address. 

* To distinguish different deposits from different users the exchange generates random identifier (called payment ID) for each one and a user attaches this payment ID to his transaction while sending. 

* Upon receiving, the exchange can extract payment ID and thus identify the user.

* In original CryptoNote there were two separate things: exchange deposit address (the same for all users) and payment ID (unique for all users). 

* Later, for user convenience and to avoid missing payment ID we combined them together into one thing, called integrated address. 

* So nowadays modern exchanges usually give to a user an integrated address for depositing instead of pair of standard deposit address and a payment ID.

<br><br>

!!! note "Note !!! <br><br> For more information on how to handle integrated addresses, please refer to RPCs `make_integrated_address` and `split_integrated_address` below"