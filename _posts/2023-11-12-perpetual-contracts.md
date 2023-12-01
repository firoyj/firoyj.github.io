---
layout: post
title: "Perpetual Contracts"
toc: true
excerpt: "Perpetual contracts, pioneered by Bitmex within the cryptocurrency space, have become one of the most popular derivative products. They allow traders to speculate on the future price of an asset by going long (buying) or going short (selling) perpetual futures contracts. Unlike traditional futures contracts, perpetual contracts don't have an expiration date and remain effective until the trader closes their position."
---

## Context

I was the first technical writer at [Advanced Blockchain](https://www.advancedblockchain.com/), an incubator for decentralized blockchain projects. Initially, I assessed the status of existing documentation, against the background of the company's requirements requirements and goals, to arrive at a scope and strategy for technical documentation. I worked closely with the product manager for Infinix, as well as stakeholders in the engineering team.

## Problem

Upon joining Advanced Blockchain, there was a noticeable lack of technical documentation. In a dynamic, fast-paced setting developers were forging ahead with highly novel DeFi projects, but the gaps in documentation presented critical bottlenecks. This resulted in challenges for testing code, maintenance, and onboarding.

## Contribution 

- Planned the scope and structure of documentation, including gaps analysis
- Initiated Docs-as-code processes in GitHub
- Documentated processes and tutorials
- Curated and edited internal knowledge base
- Software Development Plan (SDP)
- Worked in partnerwhip with key engineers to prepare documentation for an external audit

## Impact

Engineering team benefited from having clear and easily accessible documentation to 

# Understanding Perpetual Contracts: A Technical Overview

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

## Oracle API

The Infinix Protocol relies on an index or spot price feed for the underlying asset, provided by IFX. This connection ensures accurate pricing and data integrity.

Infinix accesses IFX data beacons, which are atomic data feeds powered by a first-party oracle. These beacons serve as inputs for calculating the logarithmic options pricing model. Partnering with IFX allows Infinix to scale its data categories and pairs while ensuring the data feeds are covered in the event of problems or manipulation, common in the data oracle space. _YYYY_ has a well-reserved coverage pool for valid claims.

IFX is a critical Web3 building block known for its reliable and reputable data feeds. Diverse data sets are essential for attracting users, thereby increasing the total value locked as well as fee revenue from trading positions opening and closing.

## vAMM (Virtual Automated Market Maker)

The exchange model of Infinix Protocol differs significantly from other exchanges, including AMM-based exchanges in the following main ways:

- Does not use liquidity providers or liquidity.
- 100% AMM-based; there is no Order Book.
- On-chain price reflects trades on Infinix Protocol; the price only moves when positions are opened or closed.
- Tracks the log2 of the underlying asset's price as mark price, meaning mark price = log2 (index price).

The log vAMM acts as an independent cash-settlement market. For the log vAMM mark price to approach an underlying index, Infinix Protocol needs to add a funding rate similar to those used in perpetual contracts on central limit order book exchanges.

Traders use collateral (USDC) to open long or short positions in an asset. The vAMM calculates the entry or exit price for each trade, similar to how Uniswap or other AMM-style exchanges calculate prices.

What sets vAMM apart is no asset swap happens. Unlike Uniswap, traders always start with USDC and finish with USDC, allowing the protocol to operate without needing to hold the underlying asset.
