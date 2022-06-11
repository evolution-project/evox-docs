## Starting the daemon and the wallet application as RPC server

* Starting the daemon and the wallet application as RPC server

* EvoX command-line wallet application (simplewallet) can be run in RPC server mode. 

* In this mode it can be controlled by `RPC` calls via `HTTP` and be used as a back-end for an arbitrary service.

### **Starting the wallet in RPC server mode**

1 . Run `evoxd` (the daemon application).

2 . Run `simplewallet` with the following options :

```
simplewallet --wallet-file PATH_TO_WALLET_FILE --password PASSWORD --rpc-bind-ip RPC_IP --rpc-bind-port RPC_PORT --daemon-address DEAMON_ADDR:DAEMON_PORT --log-file LOG_FILE_NAME
```
<br>

* `PATH_TO_WALLET_FILE` — path to an existing wallet file (should be created beforehand using --generate-new-wallet command);

* `PASSWORD` — wallet password;

* `RPC_IP` — IP address to bind RPC server to (127.0.0.1 will be used if not specified);

* `RPC_PORT` — TCP port for RPC server;

* `DEAMON_ADDR:DAEMON_PORT` — daemon address and port (may be omitted if the daemon is running on the same machine with the default settings);

* `LOG_FILE_NAME`  — path and filename of simplewallet log file.

<br><br>

!!! note "Note !!! <br><br> Examples in List of Wallet RPCs are given with assumption that the wallet application is running in RPC server mode and listening at `127.0.0.1:12233` <br><br> All amounts and balances are represented as unsigned integers and measured in atomic units — the smallest fraction of a coin. One coin equals `10^12` atomic units."
