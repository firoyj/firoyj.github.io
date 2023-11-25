---
layout: default
title: Guide to Request Signing
---

# Guide to Request Signing

HTTP request signing is the process of cryptographically signing requests sent to the Form3 API to increase security and ensure access to the API is restricted to entities that have access to a secret private key. Form3 supports request signing as its default authentication method for all supported HTTP method types, including `GET`, `POST`, `PATCH`, `DELETE` and `PUT`.

The Form3 API uses request signing as defined by the Network Working Group in the [Signing HTTP Requests draft](https://datatracker.ietf.org/doc/html/draft-cavage-http-signatures-12) using the `RSA-SHA256` algorithm and [PKCS #1 v1.5 padding scheme](https://datatracker.ietf.org/doc/html/rfc8017) (if `PSS` padding is used, the API responds with a `401` "Unauthorized" error).

In this example, we will sign a `POST` request to create a Payment resource. We'll use the [openssl tool](https://www.openssl.org/) to create the keys and the signature. Make sure you have it installed on your machine to follow along.

The example will take you through the following steps:

- Generate a private key
- Generate a public key
- Create the Digest header
- Create the signature value
- Complete the Authorization header
- Send the request

Note that signing other requests that contain a body (`PUT` and `PATCH`) follows the same steps. `GET` and `DELETE` requests don't require creating a Digest header.
The request we want to sign in this example is a `POST` request to create a payment resource. The unsigned version of it looks like this:

## Unsigned request header

```bash
POST /v1/transaction/payments/ HTTP/1.1
Host: api.staging-form3.tech
Date: Fri, 29 Feb 2019 14:00:00 GMT
Accept: application/vnd.api+json
Content-Type: application/vnd.api+json
Content-Length: 1137
```
