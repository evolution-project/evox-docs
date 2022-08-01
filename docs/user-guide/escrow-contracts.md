
* EvoX network provides the framework for a secure and private transaction without the need for a trusted third party.
* Our Escrow system, as proposed, will require participants to make additional deposits, which they will forfeit if there is any attempt to act maliciously, or in a way that is contemptuous toward their counter party. 



### **Proposal**

* Each escrow contract starts with the buyer proposal. 

* Once it's sent the deposit amount will be locked for a `Time until response` period. 

* If during the period seller accepts the terms, Escrow contract will be activated. 

* To initiate the process navigate to wallet `Contracts` tab and choose `New Purchase`. 

* Proposal details are the following.

    1 . `Description` -- title or description for contract subject

    2 . `Seller` -- wallet address of merchant or seller
  
    3 . `Amount` -- payment amount for goods or services
  
    4 . `Your deposit` -- sum of collateral and payment amount
  
    5 . `Seller deposit` -- collateral from seller required by buyer
  
    6 . `Comment` -- additional information like order ID, delivery address, etc.
  
    7 . `Fee` -- transaction fee amount
  
    8 . `Time until response` -- proposal expiration time
  
    9 . `Payment ID` -- transaction payment identifier provided by seller

### **Confirmation**

* When the seller accepts the proposal a special multi signature transaction will be sent to the blockchain.

* Then after `10 confirmations` a new contract will be started.

* The seller can now fulfil contract terms like shipping the item to the buyer.

* The buyers contract window will get three options to continue with: 

    1. `Cancel` and return deposits.
    
    2. `Terminate` and burn deposits. 
    
    3. `Complete` and release deposits.

### **Cancel and return deposits**

* The buyer can send a cancellation offer to return both deposits and close the contract.
* The seller can accept or ignore this offer within a given response time.
* This option is useful when deal is mutually canceled.

### **Terminate and burn deposits**

* When parties cannot find mutual agreement on any occasions one can decide to burn the deposits completely and close the contract.
* In that case deposits will not be returned ever.

### **Complete and release deposits**

* If buyer is satisfied with the delivery or a provided service the contract can be closed. 
* Releasing deposits will return both parties collaterals.

<BR>

## **More Detail ESCROW CONTRACT**

* Using escrow system, the both parties are asked to offer an agreed upon number of coins as collateral.
* This amount can be custom for each person, it depends on how much the users think is the risk for each side.
* Collateral coins will be returned to each user back after the deal is done and confirmed by both sides. 
* If a users don't agree and try to scam in escrow, the coins are burned.
* Using the Contract system build in EvoX app any user will keep his anonimity.

![Image title](/images/p2p-escrow.png) 

* Alice wants to buy a item from Bob for 100 EVOX
* Alice make a Contract in total of 300 EVOX that transfers from both Alice and Bob. First key is for Alice and second for Bob.
* All Alice Contract details will be encrypted and visible only for Bob.
* This data type was described above as transaction attachments and while the hash of the attachment will be verifiable indefinitely on the blockchain, 
the actual attachment data will fall off after passing checkpoints to manage blockchain bloat as this information at some point in the future loses its relevance. 
For example, the shipping details including name, address etc. on a package you received a year ago is no longer relevant.
* Alice adds 200 EVOX and sends the Contract proposal transaction to Bob for him to review.
* Bob receive the Contract notification, read it and if is agree he put 100 EVOX, after this Contract start and all coins from both patry are locked.
* Now Bob can sends Alice items and wait for response.
* Alice satisfied with the order, finish the Contract witch unlock 100 EVOX and send back to Alice and 200 EVOX send to Bob. Contract Complete.
* Other version is:
    1. Alice send the items back to Bob, refuse the Contract both get their money back.
    2. Alice keeps the broken items ask for compensation and both share the money 50/50
* In case of scam and anyone breaks the escrow Contract:
	1. Bob don't send the items.
	2. Alice receive the items but refuse to complete Contract and don't unlock the money.
* Both party will lose more then it should receive:
	1. Bob lose 100 EVOX.
	2. Alice pays 2 times the money for items.
* `CONCLUSION :` No party will be intrested to scam, and both will try to successfuly complete Contract