---
title: "Overview: Order Book"
mathjax: true
layout: post
categories: github, website
excerpt: "The Order Book collects groups of requests which are waiting to be processed. It does this at intervals of 5-10 seconds. Subsequently, it employs an algorithm to determine optimal routes for each request, optimizing the overall transaction efficiency.
<br><br>
After receiving a response from the algorithm, the Order Book also sorts requests for any possible *batch* transactions. The Order Book seeks to batch requests, e.g., into the same tokens on the same chain. If possible batches are found, the `Order Manager` sends them to the `Order Executer` for processing."
---

## Context and background

As the first technical writer at [Advanced Blockchain](https://www.advancedblockchain.com/), an incubator for blockchain projects, I encountered a significant lack of technical documentation upon joining. In a dynamic, fast-paced environment developer teams were forging ahead creating novel DeFi projects. However, the gaps in documentation resulted in critical bottlenecks and challenges for onboarding, code testing, maintenance, and importantly, shipping the products.

As most of the documentation I created at Advanced Blockchain is protected by non-disclosure agreements, I'm only able to share limited excerpts here. The writing sample below is part of a high-level overview, the intended audience being potential users of the dAPP Entropic.

It describes a function of the Entropic protocol, the Order Book, and it is related to the writing sample: [Overview: DEX aggregator](https://firoyj.github.io/dex-aggregator/), which harnesses the concept of 'Coincidence of Wants'.

---

## Purpose of the Order Book

The Order Book’s role is to handle users' requests - to receive them from frontend, validate, and save them in the database. it consists of _3 steps_:

1.	receive user request
2.	verify allowance
3.	save in the database

The Order Book collects groups of requests which are waiting to be processed. It does this at intervals of 5-10 seconds. Subsequently, it employs an algorithm to determine optimal routes for each request, optimizing the overall transaction efficiency.

After receiving a response from the algorithm, the Order Book also sorts requests for any possible **batch** transactions. The Order Book seeks to batch requests, e.g., into the same tokens on the same chain. If possible batches are found, the `Order Manager` sends them to the `Order Executer` for processing.

## User request interface

This is an example of the user request interface:

```javascript
{
  owner: "0x123", // string - Ethereum address of the user initiating the request.
  source: {
    chainId: 1, // number - Numeric identifier for the source chain.
    tokenAddress: "0x123", // string - Ethereum address of the source token.
    amount: 1000, // number - Numeric value representing the amount to swap.
  },
  destination: {
    chainId: 3, // number - Numeric identifier for the destination chain.
    tokenAddress: "0x123", // string - Ethereum address of the destination token.
    minAmount: 950, // number - Numeric value representing the minimum acceptable amount.
  },
  route: "fastest", // Enum representing the preferred route - "fastest," "cheapest," or "best."
  cow: "both", // Enum indicating whether the request involves both chains ("both"), only the source chain ("only-cow"), or no chains ("no-cow").
}
```

## Receiving user requests

Upon submitting a valid request — via the user request interface — to the specified endpoint (through a POST request), the system responds with an acknowledgment, ensuring the user is promptly informed of the successful receipt of their request.

Whereas, if any discrepancies or errors are detected (e.g., missing parameters, negative numbers, or incorrect addresses), the system promptly rejects the request, providing users with clear and actionable feedback.

## Verifying allowance

The user’s spending allowance is checked against Entropic’s contract. If the user has sufficient balance and allowance, the process advances to the next step. If not, the system promptly rejects the transaction, ensuring the integrity of the overall process, and the user is informed.

## Saving in the database

If all the above steps are successful, the system saves the user’s request in the database. The database captures key data elements, such as user details, source and destination details, selected route, and any additional relevant parameters. 

This stored data contributes to strategic decision-making within the system and is utilized by the `Order Manager` for ongoing order management.

## Final considerations

In summary, the Order Book plays a pivotal role in handling user requests efficiently through a structured process of request reception, batching, allowance verification, and database storage. The described user interface, request processing steps, and data storage mechanisms collectively contribute to the overall functionality of the system. By understanding these key components, users and developers can gain insights into the inner workings of Entropic, fostering a deeper appreciation for its robust and effective transaction management system.
