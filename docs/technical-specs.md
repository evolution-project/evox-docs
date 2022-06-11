---
title: Evolution Technical Specification
---
# Evolution Technical Specs

## Live

* Evolution blockchain is live since:
Timestamp [UCT] (epoch)
2020-03-02 21:31:47 (1583184707) 

## Premine

* Evolution had premine of 100.000 EVOX
* They where spend on airdrop and exchanges
* Evolution had no presale of any kind

## Proof of Work

* RandomARQ
    * v0 since block height 1 (forked from ARQMA)

## Difficulty retarget

* every block

## Block time

* 120 seconds (2 minutes)
* may varry

## Block reward

* smoothly decreasing and subject to penalties for blocks greater then median size of the last 100 blocks (M100)
* ~21.543 EvoX as of Sept 2021; for the current reward check the Evolution [Blockchain](https://explorer.evolution-network.org/)

## Block size

* dynamic
* maximum of two times the median size of the last 100 blocks (2 * M100)

## Emission curve

* Planed for 20 years

### Main emission

* The main emission is about to produce ~55 million coins by the end of .....

## Max supply

* ~55 million EVOX 

## Divisibility

* Evolution is divisible up to 9 digits
* The smallest unit equals 1e-9 EVOX, or 0.00000001 EVOX

## Sender privacy

* ring signatures
    * the ring size is 11 (10 decoys)
* assurance: probabilistic / plausible deniability

## Recipient privacy

* stealth addresses
* assurance: strong

## Amount privacy

* ring confidential transactions
* assurance: strong

## IP address privacy

For the full node (`evolutiond`):

* dandelion++
* assurance: won't protect against ISP/VPN provider, won't protect against the very first remote node in Dandellion++ protocol
* for the full protection user must manually wrap `evolutiond` with Tor

For the wallet (`evolution-electron-gui` or `evolution-wallet-cli`):

* typically wallet runs on the same machine as full node so there is no risk
* if wallet connects to remote full node, there is no IP protection by default
    * user must protect himself
