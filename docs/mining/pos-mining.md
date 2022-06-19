### Proof Of Stake (POS)

* Proof-of-stake mining or POS for short is typically implemented such that a random coin owner obtains the right to sign a new block. 
* EvoX `POS` implementation keep miners in full anonymity and simple with a push of a button.
* Open EvoX app, make sure the blockchain is synchronised and turn ON `Staking` switch.
* You can observe your progress in the `Staking` tab of your staking wallet. 
* The amount of earnings depends on the wallet balance.
* Amount of earnings depends also of how many user are running PoS.
* When you turn `Staking` OFF, balance will get unlocked right away. 
* You can switch `Staking` ON and OFF without any limitations.

### _Server mode POS mining_

* In some cases it would be convenient to do POS mining without application running. 

* Here are steps to achieve it.

    1 . Build `EvoX` daemon (evoxd executable) following instructions
    
    2 . Navigate to `Evox` folder
    
    3 . Start `evoxd` daemon (service)
    
    4 . Start wallet daemon in `RPC` mode


        simplewallet --wallet-file WalletFile --password WalletPassword --rpc-bind-port=12233 --do-pos-mining  


    * `WalletFile` -- the wallet file name
    * `WalletPassword` -- the password of the wallet that is in use
    * `12233` -- port of your choice, e.g. 12233

!!! note
        Now this wallet now participates in proof-of-stake mining.