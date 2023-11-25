---
layout: default
title: Excerpt of Overview of Cross-ledger DEX Aggregator
---

# Exploring Cross-ledger DEX Aggregation with XXXXX Protocol and X  Token

> Note: For confidentiality purposes, the protocol name is expressed here as _XXXXX_ and the token is expressed as _X TOKEN_

## Understand the Protocol

_XXXXX_ Protocol delivers a sophisticated cross-ledger DEX aggregator, enabling optimal execution through market aggregation. By tapping into Automated Market Makers (AMMs) across multiple chains, it achieves a dark pool-like order execution. Users can seamlessly initiate a swap of any supported token and the protocol finds the best route for that swap, whether it's a simple AMM swap or a more complex movement of tokens across different networks to achieve the most favorable outcome.

The protocol excels in 3 fundamental features:

### 1. Coincidence of Wants

_XXXXX_ introduces a cross-chain adaptation of Coincidence of Wants (CoW). It collects CoWs from various ledgers, matching orders off-chain. This cross-ledger smart matching engine organizes orders for potential matches and navigates through larger networks and tokens to reach the more exotic ones. _XXXXX_'s algorithm optimizes output for the user since CoW is often cheaper, faster, and more reliable.

### 2. Cross-Chain Functionality

Being capable of transferring any cryptographic asset from one ledger to another, _XXXXX_ is by definition cross-chain and cross-layer. It utilizes _X TOKEN_ for cross-chain transfers and swaps by bundling swaps and settling asset exchanges through cross-chain CoWs, while utilizing bridges for the rest of the order processing.

### 3. DEXA

_XXXXX_ harnesses the potential of chain-agnostic computing to achieve the best execution through decentralized exchange aggregation (DEXA) across multiple EVM-compatible L1s, L2s, and other ecosystems. A user initiating a token swaps has the exchange intelligently routed through a network of blockchain bridges, being sensitive to transfer cost and time. Token information is aggregated from DEXs, such as Uniswap, Sushiswap, and other compatible protocols. Liquidity data and liquidity itself are sourced to find the optimal route for the token swap.

## Process

The fundamental process of _XXXXX_ involves the following steps:

1. An API gathers all the user configurations, forwarding them for processing. The algorithm primarily operates off-chain*, interacting with multiple networks (chains and layers) bridges.
2. Orders are accumulated in batches over a predetermined period. Subsequently, the protocol endeavors to identify the best swap routes encompassing DEXs and bridges.
3. The problem is dispatched to external solvers via an API, where they attempt to solve it using custom algorithms. Solvers stake $_X TOKEN_ before submitting their solution.
4. The protocol also solves the problem using an in-house algorithm.
5. A comparison is made between the protocol's solution and the solvers' solutions, rewarding the best solver.
6. The best solution is then submitted on the blockchain.

* It's worth noting that initializing a swap request is not triggered by a blockchain event. Instead, it's initiated directly by the user, inputting information into a database through a frontend desktop tool or mobile application. This approach allows for sufficient time to determine the best route and to check for any CoWs.
