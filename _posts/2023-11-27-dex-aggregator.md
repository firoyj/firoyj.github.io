---
layout: post
title: "[Draft] Overview: DEX Aggregator"
excerpt: "The Protocol delivers a sophisticated cross-ledger DEX aggregator, enabling optimal execution through market aggregation. By tapping into Automated Market Makers (AMMs) across multiple chains, it achieves a dark pool-like order execution. This way, users can seamlessly initiate a swap of any supported token, and the protocol finds the best route for that swap, whether it's a simple AMM swap or a more complex movement of tokens across different networks to achieve the most favorable outcome."
---

## Context and problem

As the first technical writer at Advanced Blockchain, an incubator for blockchain projects, upon joining I noticed a significant lack of technical documentation. In a dynamic, fast-paced setting developers were forging ahead with highly novel DeFi projects, but the documentation gaps presented critical bottlenecks. This resulted in challenges for testing code, maintenance, and onboarding.

I assessed existing documentation, aligning it with the company goals to develop a scope and strategy for technical documentation. I joined the Entropic product team at the design stage.
The writing sample below is an excerpt of a high-level overview with elements of explanation. The purpose is to explain the product to the general reader.

## Contribution

I worked closely with the Entropic product manager and engineers on the following:

- Planning scope and structure of the documentation, including gaps analysis
- Initiating Docs-as-code processes in GitHub
- Processes and tutorials
- Collaborated with engineers on Software Development Plan (SDP)
- Curating and editing internal knowledge base

## Impact



# Exploring Cross-ledger DEX Aggregation with ENtropic Protocol and X  Token

## Understand the Protocol

Entropic delivers a sophisticated cross-ledger DEX aggregator, enabling optimal execution through market aggregation. By tapping into Automated Market Makers (AMMs) across multiple chains, it achieves a dark pool-like order execution. This way, users can seamlessly initiate a swap of any supported token and the protocol finds the best route for that swap, whether it's a simple AMM swap or a more complex movement of tokens across different networks to achieve the most favorable outcome.

The protocol excels in 3 fundamental features:

### 1. Coincidence of Wants

Entropic introduces a cross-chain adaptation of Coincidence of Wants (CoW). It collects CoWs from various ledgers and matches the orders off-chain. This smart matching engine sorts orders to find any potential matches, and then navigates through larger networks and tokens to access the more exotic ones. Entropic's algorithm optimizes output for the user since CoW is often cheaper, faster, and more reliable.

### 2. Cross-Chain Functionality

Being capable of transferring any cryptographic asset from one ledger to another, Entropic is by definition cross-chain and cross-layer. It utilizes TROPIC token for cross-chain transfers and swaps by bundling swaps and settling asset exchanges through cross-chain CoWs, while utilizing bridges for the rest of the order processing.

### 3. DEXA

Entropic harnesses the potential of chain-agnostic computing to achieve the best execution through decentralized exchange aggregation (DEXA) across multiple EVM-compatible L1s, L2s, and other ecosystems. A user initiating a token swaps has the exchange intelligently routed through a network of blockchain bridges, being sensitive to transfer cost and time. Token information is aggregated from DEXs, such as Uniswap, Sushiswap, and other compatible protocols. Liquidity data and liquidity itself are sourced to find the optimal route for the token swap.

## Process

The fundamental process of Entropic involves the following steps:

1. An API gathers all the user configurations, forwarding them for processing. The algorithm primarily operates off-chain*, interacting with multiple networks (chains and layers) bridges.
2. Orders are accumulated in batches over a predetermined period. Subsequently, the protocol endeavors to identify the best swap routes encompassing DEXs and bridges.
3. The problem is dispatched to external solvers via an API, where they attempt to solve it using custom algorithms. Solvers stake TROPIC token before submitting their solution.
4. The protocol also solves the problem using an in-house algorithm.
5. A comparison is made between the protocol's solution and the solvers' solutions, rewarding the best solver.
6. The best solution is then submitted on the blockchain.

* It's worth noting that initializing a swap request is not triggered by a blockchain event. Instead, it's initiated directly by the user, inputting information into a database through a frontend desktop tool or mobile application. This approach allows for sufficient time to determine the best route and to check for any CoWs.
