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

### Trustful Bridges
- Typically have some sort of operator in the middle that ensures blocks/transactions are valid
- This is done when its hard to verify the consensus mechanism of a chain
- Examples: xDai

### Trustless Bridges
- Don't require any sort of operator
- Consensus is verified independently by the chain
- Q: Why not make only trustless bridges?
- A: It's kinda hard.

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

<!-- effect=matrix-->

## Finality

---
```rust
let foo = Foo {bar, baz};
```
