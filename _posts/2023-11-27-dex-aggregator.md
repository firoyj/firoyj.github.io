---
layout: post
title: "Overview: DEX aggregator"
excerpt: "Entropic protocol delivers a sophisticated cross-ledger DEX aggregator. It enables optimal execution through market aggregation. By tapping into Automated Market Makers (AMMs) across multiple chains, it achieves a dark pool-like execution of orders. This means users can seamlessly initiate a swap of any supported token, then the protocol finds the best route for that swap, whether it's a simple AMM swap or a more complex movement of tokens across different networks to achieve the best outcome."
---

## Context and problem

As the first technical writer at [Advanced Blockchain](https://www.advancedblockchain.com/), an incubator for blockchain projects, I arrived to a notable lack of technical documentation. In a dynamic, fast-paced environment developer teams were forging ahead creating novel DeFi projects. However, the gaps in documentation resulted in critical bottlenecks and challenges for onboarding, code testing, maintenance, and importantly, shipping the products.

As most of the documentation I created at Advanced Blockchain is protected by non-disclosure agreements, I'm unable to share it here. The writing sample below is an excerpt of a high-level overview, the intended audience being the general public. 

## Contribution

In my role, I started by evaluating the existing status of the documentation. I then developed a scope and strategy in alignment with the company's goals. I joined the Entropic product team at the design stage with much of the product still to be detailed.

In terms of my contribution, I worked closely with the Entropic product manager and engineers on the following:

- Planned for scope and strategy of the documentation, including gaps analysis
- Initiated a Docs-as-code process in GitHub
- Created documentation for processes and tutorials
- Collaborated with engineers on Software Development Plan (SDP)
- Curated and edited internal knowledge base

## Impact

- The simplified format and language of the technical details enabled the auditors to better carry out their assessments
- My clear documentation helped streamline collaboration and improved efficiency within the team
- New joiners were able to onboard quicker and with more confidence.
- My work to deploy and demonstrate Docs-as-code practices in GitHub revolutionized the documentation workflow for the whole team, enhancing collaboration and transparency.

---

# Exploring cross-ledger DEX aggregation with Entropic protocol and X token

## Understand the protocol

Entropic delivers a sophisticated cross-ledger DEX aggregator. It enables optimal execution through market aggregation. By tapping into Automated Market Makers (AMMs) across multiple chains, it achieves a [dark pool-like](https://www.investopedia.com/articles/markets/050614/introduction-dark-pools.asp) execution of orders. This means users can seamlessly initiate a swap of any supported token, then the protocol finds the best route for that swap, whether it's a simple AMM swap or a more complex movement of tokens across different networks to achieve the best outcome.

The protocol excels in 3 fundamental features:

### 1. Coincidence of wants

Entropic introduces a cross-chain adaptation of Coincidence of Wants (CoW). It collects CoWs from various ledgers and matches the orders off-chain. This smart matching engine sorts orders looking for any potential matches. It then navigates through larger networks and tokens to access the more exotic ones. Entropic's algorithm optimizes output for the user since CoW is often cheaper, faster, and more reliable.

### 2. Cross-chain functionality

Being capable of transferring any cryptographic asset from one ledger to another, Entropic is, by definition, cross-chain and cross-layer. It utilizes TROPIC token for cross-chain transfers, which involves bundling swaps and settling asset exchanges through cross-chain CoWs. The remaining orders are processed utilizing bridges.

### 3. DEXA

Entropic harnesses the potential of chain-agnostic computing to optimize for best execution via decentralized exchange aggregation (DEXA) across multiple EVM-compatible L1s, L2s, and other ecosystems. When a user initiates a token swap, the exchange is intelligently routed through a network of blockchain bridges, being sensitive to transfer cost and time. Information on tokens from DEXes, such as Uniswap, Sushiswap, and other compatible protocols is aggregated to gather token details. Liquidity data and the liquidity itself are sourced to determine the most efficient route for the token swap.

## Process

The fundamental process of Entropic involves the following steps:

1. User configurations are gathered by an API and forwarded for processing. The algorithm mainly operates off chain*, interacting with multiple networks (chains and layers) and bridges.
2. Orders are accumulated in batches over a predetermined period.
3. The protocol then works to identify the best swap routes, considering DEXes and bridges.
4. The problem is sent out via an API to external solvers, who attempt to solve the problem using custom algorithms. Solvers stake some TROPIC token before submitting their solution.
5. Meanwhile, Entropic protocol also solves the problem using an in-house algorithm.
6. The solvers' solutions are compared with Entropic's solution, and the best solver is rewarded.
7. The best solution is submitted on the blockchain.

* Note: initiating a swap request is not triggered by a blockchain event. Instead, the user directly starts it by inputting information using a frontend tool on a desktop or mobile application. This approach allows sufficient time to find the best route and to check for any CoWs.
