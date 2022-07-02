
### **What is an auditable wallet ?**

* Auditable is the type of wallet that allows 3rd party to see the balance and transaction history without permission to spend it or interact in any other way.

### **How can I tell if wallet is auditable ?**

* It's a wallet with an address in a special format that starts with `"aEX"`. 
* For instance: 

> `aEX`awNXAuekCXcnzutthLaPZQxAyaofb59FpzNBSCQb7iT7D1nsaxdTCvK4Xhn6nfuRpqDiN

### **What is the purpose of auditable wallets ?**

* Having an auditable wallet, you can allow someone to watch your balance and transaction history without giving him/her the right to spend your funds.

### **Can I get an auditable address for my existing normal wallet ?**

* NO. You need to create a new auditable wallet and transfer your coins into it.

### **How can I create an auditable wallet ?**

* Using simplewallet CLI application :

```    
>simplewallet.exe --generate-new-auditable-wallet auditable_wallet_x
    
EvoX wallet v1.1.7[3e463b0]

password: ***
Generated new AUDITABLE wallet: aEXb9v1DFtaK6Z4bW7UUuaZcmq7MZBzz875eZ5N3vSRa2vWz9wBVE3vVKFGNH8414TTjhiwPz7PTV5ttuZP7GsdDQeWbewpmMaX
view key: 3dd8fd870c694818194c1e7a095a51e2e65486e212baca77fce4157f39287f05    
tracking seed:
aEXb9v1DFtaK6Z4bW7UUuaZcmq7MZBzz875eZ5N3vSRa2vWz9wBVE3vVKFGNH8414TTjhiwPz7PTV5ttuZP7GsdDQeWbewpmMaX:3dd8fd870c694818194c1e7a095a51e2e65486e212baca77fce4157f39287f05:1595429852 
    
**********************************************************************
Your wallet has been generated.
**********************************************************************
```

### **Using an auditable wallet, how can I give someone the ability to track my balance and transaction history ?**

* You should obtain a tracking seed for your auditable wallet and give it to someone you'd like to be able to track your wallet. 
* At the moment, it can only be done by using `tracking_seed` command in `simplewallet` :

```
>simplewallet.exe --wallet-file auditable_wallet_x

EvoX wallet v1.1.7.96[3e463b0]

password: ***
Opened auditable wallet: aEXb9v1DFtaK6Z4bW7UUuaZcmq7MZBzz875eZ5N3vSRa2vWz9wBVE3vVKFGNH8414TTjhiwPz7PTV5ttuZP7GsdDQeWbewpmMaX
Starting refresh...
Refresh done, blocks received: 0
balance: 0.000000000000, unlocked balance: 0.000000000000
**********************************************************************
Use "help" command to see the list of available commands.
**********************************************************************

[EvoX wallet aEXb9v]: tracking_seed

Auditable watch-only tracking seed for this wallet is:
aEXb9v1DFtaK6Z4bW7UUuaZcmq7MZBzz875eZ5N3vSRa2vWz9wBVE3vVKFGNH8414TTjhiwPz7PTV5ttuZP7GsdDQeWbewpmMaX:3dd8fd870c694818194c1e7a095a51e2e65486e212baca77fce4157f39287f05:1595429852 
Anyone having this tracking seed is able to watch your balance and transaction history, but unable to spend coins.
```

* Tracking seed technically is an `auditable address` , `secret view key` and a creation `timestamp` combined with a colon:

```
aZxawNXAuekCXcnzutthLaPZQxAyaofb59FpzNBSCQb7iT7D1nsaxdTCvK4Xhn6nfuRpqDiNjeUNx2J9KWfTXHmDWNQ85v2bpbi:1be5866b6fda704c0c4a08f9c79c774911fda72dcd8cc8c7ca31bc1a6fda4d0b:1593998615 
```

### **I got a tracking seed. How can I track the wallet that it is bound to ?**

* You need to restore a wallet using the tracking key the same way you restore a regular wallet using a seed. 

1 . It could be done in simplewallet:

```
>simplewallet.exe --restore-wallet=wallet-name

EvoX wallet v1.1.7[3e463b0]

password: ***
please, enter wallet seed phrase or an auditable wallet's tracking seed: ***
Tracking wallet restored: aEXb9v1DFtaK6Z4bW7UUuaZcmq7MZBzz875eZ5N3vSRa2vWz9wBVE3vVKFGNH8414TTjhiwPz7PTV5ttuZP7GsdDQeWbewpmMaX 
**********************************************************************
Your wallet has been restored.
To start synchronizing with the daemon use "refresh" command.
Use "help" command to see the list of available commands.
Always use "exit" command when closing simplewallet to save
current session's state. Otherwise, you will possibly need to synchronize
your wallet again. Your wallet keys is NOT under risk anyway.
**********************************************************************
```

2 . In GUI wallet.

### **Are there any restrictions of using auditable wallets ?**

* `Only one` : you cannot use mixins when you send coins from an auditable wallet.

??? note 

    Mention

    The following limitations were effective until `hardfork 2`:

        * When sending coins from an auditable address the sender address is always hidden.
        * When sending coins to an auditable address the receiver address is always hidden.
        Once the blockchain passed hardfork 2 these limitations were removed.

### **Can I use integrated addresses with the auditable feature ?**

* Yes. An integrated address for an auditable wallet can be generated as usual. Addresses will have `aiEX` prefix.

### **Can I mine PoS with my auditable wallet ?**

* Yes, you can. 

* Also, you can use a corresponding watch-only wallet to monitor your balance without the risk of leaking your spend key. 

* The only tradeoff is you can not use mixins but only directly spend coins from your auditable wallet.