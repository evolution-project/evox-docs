
### Building distributed markets based on Evox

* The EvoX blockchain will act as a platform for building distributed services. 
* One of those services is our Marketplace, offering out of the box blockchain solutions.
* With the Evox Marketplace you create, update, or deactivate offers.
* Offers contain information about a user who is selling or buying something.
* As soon as an offer is published in the blockchain it is visible to everyone.
* This feature allows developers to build a decentralized online stores, based on offers and escrow contracts.
* The Evox daemon has built-in service, that can show all active offers from the blockchain as multi indexed set with diverse filtering options. 
* These offers are active for 2 weeks, after that, an offer needs to be re-posted.

### _Posting and updating offers_

* To be able to `post/update` offers you have to work with wallet `API`, since every operation over the offers has it's fee. 

    * Posting offer.
    
        * To post an offer should be used `marketplace_push_offer` API method.

        * This method takes as a parameter `Offer structure` and sends it with the carrier transaction to the blockchain. 
    
        * As soon as transaction got 10 confirmations it will appear in `search API` of the daemon.
    
    * Updating offer.

        * It's possible to update an offer if needed to make changes in any of the offer's fields. 
        * When wallet create a carrier transaction with updating offer, it includes proof that this update created by the owner of the original offer transaction.
        * To perform update should be used `marketplace_push_update_offer` wallet API method. 
        * In parameters should be specified original offer post transaction ID as a reference, and full details of the new updated version of the offer.
        * Update procedure may be repeated many times, every time reference transaction id should be used from last update operation or from original posting transaction if there were no updates before.
    
    * Cancelling offer.

        * It's also possible to mark an offer as inactive by calling `marketplace_cancel_offer`. 
        * This API call confirm the authority of the owner same was as `marketplace_push_update_offer`, so the caller must provide original offer's transaction id as a reference. 
        * After carrier transaction with this command is confirmed offer is no longer returned by `search API` of the daemon.

    * Enumerating my offer.

        * To make easier management of the offers that belong to a particular wallet, we introduced `marketplace_get_offers_ex` API.
        * This API return list of the active offers that had been posted from the current wallet. 
        * Diverse set of filtering and ordering options explained in documentation to this method.

### _Reading offers from blockchain_

* First of all, to activate `Marketplace` service in the daemon. 
    
    * By default, this service is deactivated to avoid performance waste.
    * To activate this service you have to run daemon with :
        <br> `--enable-offers-service` command line option.
    * To fetch active offers from blockchain should be used `marketplace_global_get_offers_ex` method. 
    * This method based on the filter structure, which provide diverse set of options for filtering and ordering offers.
    * If you want basically to enumerate all offers in the blockchain you may want to use offset and limit options from filter. 
    * By adding limit amount to offset in every next method call, you can subsequently fetch all offers from blockchain, even if there are millions of it.
