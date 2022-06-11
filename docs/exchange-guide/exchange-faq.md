## Exchange FAQ

Frequently asked questions for exchanges and services

<br>

### **Why don't you compile your code and just send binaries to us?**

* Our project is open source and we believe this would be insecurity in its extreme form.

<br>

### **Which OS is recommended?**

* Ubuntu 18.04 LTS or Ubuntu 20.04 LTS

<br>

### **We got "internal compiler error: Killed"**

* You ran out of RAM. Try to limit make job slots by specifying `make -j 1`

<br>

### **We got compiler/linker error mentioning Boost**

* Make sure you have built the recommended version of Boost manually (via `./bootstrap.sh`, `./b2`).

<br>

### **Wallet RPC is not working. We got "Core is busy" in logs/responses**

* Make sure the daemon is synchronized with the network. It may take up to few hours when running for the fist time. When it synchronized you'll stop seeing yellow ` "sync progress" ` messages in daemon logs.

<br>

### **How to validate an address?**

* To validate an address you can use `split_integrated_address`. It also works with standard addresses (non integrated one)

<br>

### **How to get all integrated addresses on a wallet?**

* A wallet does not store all integrated addresses, thus it is impossible. 

* Integrated address is just your wallet address PLUS encoded hex payment id you provided packed together.

* As you can provide ANY payment id you could imagine, you can generate unlimited number of integrated addresses for a wallet.

<br>

### **Can we use random payment id when generating integrated address for a user?**

* Yes, It is highly recommended to use randomly generated payment id's to identify each of your users.

<br>

### **What transaction fee should we specify in RPCs?**

* Minimum transaction fee is `0.01` EvoX.

<br>

### **What are "pub_keys" that we see in transaction output via explorer?**

* Each output in CryptoNote-like currency has it's own public key (i.e. one-time destination key) that cannot be linked with the address or other outputs. 

* The owner of the output calculates the private key when he spends it.

<br>

### **We got "Invalid params" RPC response**

* Make sure you pass amounts as integers not strings.

<br>

### **We can't see our own transfer when filtering by payment id with get_bulk_payments or get_payments RPC calls**

* Make sure you're not sending coins to yourself (from an address to the very same address). 

* Coins which were sent that way will safely reach their destination (and the balances will be correct) but such a transfer won't be seen when you filter transfers by payment id via `get_bulk_payments` or `get_payments`.