---
layout: post
title: "Overview: Perpetual contracts"
toc: true
excerpt: "Perpetual contracts, pioneered by Bitmex within the cryptocurrency space, have become one of the most popular derivative products. They allow traders to speculate on the future price of an asset by going long (buying) or going short (selling) perpetual futures contracts. Unlike traditional futures contracts, perpetual contracts don't have an expiration date and remain effective until the trader closes their position.<br><br>Perpetual contracts allow traders to speculate on the future price of a given asset by buying (going long) or selling (going short). Unlike typical futures contracts, perpetuals do not expire and instead remain effective until the trader closes their position."
---

## Context and problem

As the first technical writer at [Advanced Blockchain](https://www.advancedblockchain.com/), an incubator for blockchain projects, upon joining I noticed  a significant lack of technical documentation. In a dynamic, fast-paced setting developers were forging ahead with highly novel DeFi projects, but the documentation gaps presented critical bottlenecks. This resulted in challenges for testing code, maintenance, and onboarding.
<br><br>
I assessed existing documentation, aligning it with the company goals to develop a scope and strategy for technical documentation. I joined the Infinix product team at the design/implementation stage.<br><br>
The writing sample below is an excerpt of a high-level overview with elements of explanation. The purpose is to explain the product to the general reader who may want to interact with the DeFi product.

## Contribution 

 I worked closely with the Infinix product manager and engineers on the following:

- Planned scope and structure of the documentation, including gaps analysis
- Initiated Docs-as-code processes in GitHub
- Collaborated with key engineers to prepare documentation for an external audit
- Documentated processes and tutorials
- Collaborated with engineers on Software Development Plan (SDP)
- Curated and edited internal knowledge base

## Impact

- My work to simplify the format and language of the technical details enabled the auditors to better carry out their assessments
- Clear documentation streamlined collaboration, improving efficiency and facilitating a more thorough testing process. 
- Maintenance procedures were enhanced, enabling easier codebase updates and issue resolution. 
- The onboarding process for new team members became more efficient, 
- The adoption of Docs-as-code practices in GitHub revolutionized the documentation workflow, enhancing collaboration and transparency.
- Collaboration on the SDP ensured alignment between management and project goals and development approach.
- The knowledge base became easier to use and highly valued.

In essence, the impacts of my work extended across collaboration, testing, maintenance, onboarding, and adherence to high standards, significantly optimizing the overall development process.

---

# Understanding perpetual contracts: A technical overview

Perpetual contracts, pioneered by Bitmex within the cryptocurrency space, have become one of the most popular derivative products. They allow traders to speculate on the future price of an asset by going long (buying) or going short (selling) perpetual futures contracts. Unlike traditional futures contracts, perpetual contracts don't have an expiration date and remain effective until the trader closes their position.

Perpetual contracts allow traders to speculate on the future price of a given asset by buying (going long) or selling (going short). Unlike typical futures contracts, perpetuals do not expire and instead remain effective until the trader closes their position.

The price of perpetual contracts often diverges from the broader market, known as the spot market. These deviations signal sentiment on the exchange - if a majority of traders expect the underlying asset price to increase over time, the perpetual contract price will likely exceed the spot price. Conversely, if most traders expect the price to fall, the perpetual contract price will drop below the spot price.

Two key mechanisms moderate this process, their purpose being to keep the perpetual contract price close to the spot price:

## 1) Funding payments

Traders with open long or short positions make funding payments to each other every hour, based on market conditions:

- If the contract price is above the spot price, longs pay shorts.
- If the contract price is below the spot price, shorts pay longs.

The size of the funding payment depends on the difference between the contract price and the spot price, as well as the user's position size. This system incentivizes traders to take the less popular side of the market.

## 2) Arbitrage

When the contract price significantly diverges from the spot price on other exchanges, arbitrageurs can benefit in two ways:

1. If they hold a position elsewhere, they can use Infinix Protocol to take the inverse position and earn funding payments.
2. If they buy or sell an asset elsewhere, they can long or short that asset using Infinix Protocol, expectating the price to move back toward the spot price.

![image](https://github.com/firoyj/firoyj.github.io/assets/101256554/4aafe62e-a43f-4c88-831d-cc4eddc9d447)
Fig. 1 - Oracle API

## Oracle API

The Infinix Protocol relies on a fundamental component: an index or spot price feed sourced from an oracle, specifically supplied by [API3](https://api3.org/). Functioning as a DAO, API3 provides open-source technology and tooling, facilitating the seamless integration of Web APIs directly into smart contracts through first-party oracles. Think of this as connecting off-chain, real world data points in Web2 to applications in Web3. This integration is critical in ensuring the precision of pricing information and integrity of data throughout the Infinix Protocol.

Infinix accesses INFX data beacons, which are atomic data feeds powered by the oracle. These beacons serve as inputs for calculating the logarithmic options pricing model. Partnering with INFX allows Infinix to scale its data categories and pairs while ensuring the data feeds are covered in the event of problems or manipulation, common in the data oracle space. INFX has a well-reserved coverage pool for valid claims.

INFX is a critical Web3 building block known for its reliable and reputable data feeds. Diverse data sets are essential for attracting users, thereby increasing the total value locked as well as fee revenue from trading positions opening and closing.

## vAMM (Virtual Automated Market Maker)

The exchange model of Infinix Protocol differs significantly from other exchanges, including AMM-based exchanges in the following main ways:

- Does not use liquidity providers or liquidity.
- 100% AMM-based; there is no Order Book.
- On-chain price reflects trades on Infinix Protocol; the price only moves when positions are opened or closed.
- Tracks the log2 of the underlying asset's price as mark price, meaning mark price = log2 (index price).

The log vAMM acts as an independent cash-settlement market. For the log vAMM mark price to approach an underlying index, Infinix Protocol needs to add a funding rate similar to those used in perpetual contracts on central limit order book exchanges.

Traders use collateral (USDC) to open long or short positions in an asset. The vAMM calculates the entry or exit price for each trade, similar to how Uniswap or other AMM-style exchanges calculate prices.

What sets vAMM apart is no asset swap happens. Unlike Uniswap, traders always start with USDC and finish with USDC, allowing the protocol to operate without needing to hold the underlying asset.
