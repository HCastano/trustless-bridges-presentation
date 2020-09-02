# Trustless Bridges

### By: Hernando Castano
### hernando@parity.io

---

## Presentation Outline

- What is a bridge?
- Types of Bridges
- Rialto Bridge
    - Brief Interlude: Finality
    - Architecture
    - Applications
- Live Demo
- Future Plans
    - Substrate <-> Substrate
    - Becoming a Parachain

---

## What is a Bridge?

tl;dr: A way to connect two unrelated chains

### Types of Bridges
- Custodial
- Collateral
- Trustless

---

## Custodial Bridges

[ETH] -> [USDT] -> [BTC]

- You can think of Tether as a custodial bridge
- You trade your Ether for Tether, which you can swap for BTC

[ETH] -> [Central Party] -> [BTC]

- You send your ETH to the central party
- Expectation is that they will credit your account with correct amount of BTC

---

## Collateral Backed Bridges

[ETH]            [BTC]
    \ [Relayer] /


- You send your funds to a multisig

[ETH] -> [Multisig]

- Relayer is listening for "proof-of-lock" events from contract
- Relays these proofs to recepient chain

[Relayer] -> Proof -> [BTC]

- BTC is unlocked on receiving end
- Relayer submits proof that they actually released funds
- Important because if challenged they could get slashed on ETH

---

## Trustless Bridges

[ETH] -> [SUB]

- Basically an on-chain light client for a foreign chain

[ETH Headers] -> [SUB]

- A light client will track headers of the source chain
- If applicable, needs to keep track of finality

[ETH Tx] -> [SUB]

- Now we can send an ETH Tx which locks funds
- Bridge can verify by itself that the Tx is included in a valid block

---

## Railto Bridge

[PoA Ethereum] <-> [Substrate]

- Two way trustless bridge between an Ethereum PoA chain and a Substrate chain
- Ethereum chain is using Aura consensus
- Substrate chain is using Grandpa consensus

---

## Authority Round (Aura) Consensus

- Blocks are produced at regular intervals by trusted authorities
- Authorities implicitly vote on blocks by building on blocks
- A block is considered finalized if it has (n/2) + 1 votes
- If we had three validators: (3/2) + 1 = 2

[A] <- [B] <- [C]
        |
        --- Finalized when C is built

---

## Grandpa Consensus

- Authorities are staked
- Authorities vote on chains of blocks which they think are final
- Authority sets change periodically
- Blocks that signal these changes have a special log in the header

```
sp_runtime::DigestItem::Consensus(ConsensusEngineId, Vec<u8>)
sp_finality_grandpa::ConsensusLog::ScheduledChange
```
---

## Rialto Architecture

   OpenEthereum Node                    Substrate Node

+-------------------+                +-------------------+
|                   |                |                   |
| Grandpa Built-In  |                |  Ethereum Bridge  |
| Contract          |                |  Pallet           |
|                   |                |                   |
|                   |                |  Currency Exchange|
| Grandpa Finality  |                |  Pallet           |
| Smart Contract    |                |                   |
|                   |                |                   |
|                   |                |                   |
|                   |                |                   |
+-------------------+                +-------------------+

---

## Bridge Relay

[OpenEthereum Node]                 [Substrate Node]
                  \ [Bridge Relay] /

- The chains can't acutally talk to eachother
- Need a piece of helper software called a relay
- Syncs each node and relays messages via RPC

---

## Rialto Applications

+--------------------+-------------------+
|Currency Exchange   | Message Passing   |
|                    |                   |
+--------------------+-------------------+
|                                        |
|    Light Client Base Layer             |
+----------------------------------------+

---

<!-- effect=matrix-->

# SHOW ME CODE!

---

## Future Plans

[DOT] <-> [KSM]

- Want to connect Substrate based chains
- May be in parachain form

[ETH] <-> [DOT]

- Want to connect Ethereum maininet to Substrate chains

[DOT::Call] <-> [KSM::Call]

- Arbitrary message passing between Substrate chains

---

## Running the Code

- Go to the `parity-bridges-common` repo
- Can run simple Eth2Sub header sync using scripts in `./scripts` folder
- Can do a full network deployment with `docker-compose`

---
## Braindump

- Could show a local version of Eth2Sub first
    - Give people a feel for the header sync
    - Show the dashboards
- Could show Rialto deployment dashboards next
    - Show them each of the header sync dashboards
    - Then show them the currency exchange dashboard
- Could do a code walkthrough after that
    - Show the Eth pallet
    - Then the CE pallet
    - If there's time, talk about the relay
- Finality, do a demo of the Rialto UI
    - Send RLT -> SUB
    - Check to see if others were able to send tokens
- Can show other bits and bobs
    - Deployment process
