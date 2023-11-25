## Purpose of the Order Book
---
layout: default
title: Excerpt Introduction to Order Book
---

The Order Book’s responsibility is to handle users' requests - to receive them from frontend, validate, and save them in the database. it consists of _3 steps_:

1.	receive user request
2.	verify allowance
3.	save in the database

The Order Book collects a group of requests which are waiting to be processed. It does this periodically every 5-10 seconds. It then calculates the best routes for each request by accessing the algorithm. 

After receiving a response from the algorithm, the Order Book also sorts requests for any possible **batch** transactions. The Order Book tries to batch requests, e.g., into the same tokens on the same chain. If possible batches are found, the Order Manager sends them to the Order Executer for processing.


## User request interface
```
`{
  owner: "0x123", // string - user
  source: {
    chainId: 1, // number - source chain
    tokenAddress: "0x123", // string - source token
    amount: 1000, // number - amount to swap
  },
  destination: {
    chainId: 3, // number - destination chain
    tokenAddress: "0x123", // string - destination token
    minAmount: 950, // number - minimal acceptable amount
  },
  route: "fastest", // enum "fastest" | "cheapest" | "best"
  cow: "both", // enum "both" | "only-cow" | "no-cow"
}`
```

## Receive user request

The user sends a request to an endpoint (POST) with the interface, which is rejected if the request interface is invalid (e.g., missing parameters, negative numbers, incorrect addresses), otherwise it proceeds to the next step.

## Verify allowance

The user’s spending allowance is checked against _XXXXX_’s contract. If there's sufficient balance and allowance to carry out the transaction, it proceeds to the next step, otherwise it's rejected.

## Save in the database

If all the above steps succeed, the user’s request is saved in the database for later use. This database contains the data used by the Order Manager.
