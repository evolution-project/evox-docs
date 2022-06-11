
* The Evox daemon features an internal stratum-like server that can serve miner clients via the ethProxy protocol. 
* It works like a very light and simple pool that mines to a single address.
    * To run a GPU miner with the internal Evox stratum server follow these steps :

        1 . build the daemon (evoxd executable)

        2 . run the daemon with an activated stratum server

        3 . run the GPU or CPU miner connected to the daemon

* Once all started, the miner should connect to the daemon and receive a job from it.
* Upon finding a solution, the miner should send it to the daemon and the daemon should confirm the solution.
* Both can run on remote machines.

### _Windows quick guide_

* First, install the Evox app, create a Evox wallet and wait until blockchain syncing is complete. 
* When syncing is complete close the app.
    * In order to mine, Evox must be started with the stratum server activated. 
        * Open a `cmd` console window and navigate to the `Evox` folder 

            `C:\Program Files\Evox`  by default

* Here in the folder run

```
evoxd.exe --stratum --stratum-miner-address=YourWalletAddress --log-level=0 --stratum-bind-port=52500 
```

