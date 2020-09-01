# Trustless Bridges

### By: Hernando Castano
### hernando@parity.io

---

## Presentation Outline

- What is a bridge?
    - Trustful vs. Trustless
- Finality
    - Authority Round
    - Grandpa
- Our Bridge
    - PoA -> Substrate
    - Substrate -> PoA
    - Currency Exchange Pallet
    - Deployment
- Future Plans
    - Substrate <-> Substrate

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

- We need to run a light client to verify headers ourselves

[ETH Headers] -> [SUB]

- A light client will track headers of the source chain
- If applicable, needs to keep track of finality

[ETH Tx] -> [SUB]

- Now we can send an ETH Tx which locks funds
- Bridge can verify by itself that the Tx is included in a valid block

---

## Making a Transfer

[Chain A] ---> [Chain B]

1. Generate block on Chain A
2. Send block to Chain B*
3. Chain B must verify integrity of Chain A's block
4. If Chain B is happy, it accepts it

\* This can be done through a few different ways, but is external to the system

---

## Verifying a foreign block
- Hard because you need to verify a chain's consensus algorithm
- It takes miners a long time on BTC or ETH to produce a block, now you think you can do
  it in under 6s on your Substrate chain?
- Who's to say that this block is even part of the canonical chain?

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

<!-- effect=matrix-->

# SHOW ME CODE!

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

## Rialto: Code Walkthrough

- Should maybe walk through repo README to show project layout
- Could mention deployment process
- Could metion monitoring tools
- Walk through the relay node code



