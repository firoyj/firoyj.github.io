---
title: "Tutorial: Payment Exception Handling"
mathjax: true
layout: post
categories: github, website
excerpt: "Sending and receiving payments with the Form3 Payments API is quick and simple, but sometimes a payment submission fails. When this happens, the API provides an error message to help identify the cause.<br><br>This tutorial looks at different errors that can occur during a payment submission and explains how to diagnose them."
---

## Overview

From [Form3 API Docs](https://www.api-docs.form3.tech/api/tutorials/getting-started/payment-exception-handling)

Sending and receiving payments with the Form3 Payments API is quick and simple, but sometimes a payment submission fails. When this happens, the API provides an error message to help identify the cause.

## Tutorial: Payment Exception Handling

This tutorial looks at different errors that can occur during a payment submission and explains how to diagnose them. This section covers the following topics:

- Handling incomplete payment data.
- Managing incorrect receiving account information.

## Prerequisites

Before you proceed, we recommend that you complete the previous tutorials on sending and receiving payments. Ensure you have the following ready to follow along:

- An organisation ID (contact Form3 to obtain it).
- A private-public key pair to authenticate with the Form3 API via HTTP Request Signing.
- At least one UK sortcode and BIC registered with Form3.
- Access to the transaction simulator — a sandbox environment for you to simulate transactions in order to test your application.

## Invalid Payment Data

If an outbound payment is created with invalid or incomplete data, it cannot be processed correctly.

## Incorrect Syntax

The server responds differently depending on the kind of error. If the JSON syntax is incorrect, the server will send a response with the status `400 Bad Request`. If the syntax is correct, but a field contains invalid data (e.g., the `amount` attribute contains letters), the creation of the resource will fail and an error message will be returned:

```javascript
{
    "error_code": "5fa8f56c-d0d1-422b-aba7-f8c9522d6388",
    "error_message": "Validation failed: Field data.attributes.amount : must match \"^[0-9.]{0,20}$\" "
}
```

## Missing Attributes

If the JSON syntax is correct, the payment resource will be created successfully. You can then send the payment by creating a submission resource. However, invalid data may cause the payment submission to fail, resulting in a `delivery_failed` status.

To check the status of the submission, you can use the fetch call for the submission resource itself, you can use audits to see the entire history of the resource, or you can subscribe to the creation event of submission resources to receive a notification whenever a new submission resource is created.

Using either of these methods, you can read the `status` attribute of the submission. If the submission has failed, the `status` will say: `delivery_failed`. In this case, the `status_reason` attribute provides further information about the error. If, for example, the `amount` attribute in the payment resource was missing, the status reason says "attributes.amount:may not be null. Value passed was null".

The status reason will change for different error cases. In case several attributes are missing, it will list all missing fields.

The example below sends an FPS payment with a missing `amount` field. The first snippet is the body of a `POST` call to `/v1/transaction/payments` to create a Payment resource. It is a complete and correct payment but missing the `amount`:

## Example: Creating a Payment Resource Without an Amount Field

Here’s an example of a `POST` request to `/v1/transaction/payments` to create a payment resource. This request is complete except for the missing `amount` field:

```javascript
{
    "data": {
        "type": "payments",
        "id": "d596fa25-6943-4490-b94f-61f4c44a8941",
        "organisation_id": "fa3c28ca-90ac-41ef-8542-21f1bf74d0b5",
        "attributes": {
            "beneficiary_party": {
                "account_name": "J Doe",
                "account_number": "71268996",
                "account_number_code": "BBAN",
                "account_with": {
                    "bank_id": "400302",
                    "bank_id_code": "GBDSC"
                }
            },
            "currency": "GBP",
            "debtor_party": {
                "account_name": "H Smith",
                "account_number": "87654321",
                "account_number_code": "BBAN",
                "account_with": {
                    "bank_id": "%s",
                    "bank_id_code": "GBDSC"
                }
            },
            "reference": "payment-without-amount",
            "scheme_payment_sub_type": "TelephoneBanking",
            "scheme_payment_type": "ImmediatePayment",
            "payment_scheme": "FPS"
        }
    }
}
```

This call returns a `201 Created` HTTP code. To submit the payment, create a Payment Submission resource with the following `POST` request to:

```
v1/transaction/payments/d596fa25-6943-4490-b94f-61f4c44a8941/submissions
```

## Example: Creating a Payment Submission Resource

```javascript
{
  "data": {
    "id": "abf05458-69de-4949-9c5d-8ed3c15d966a",
    "type": "payment_submissions",
    "organisation_id": "fa3c28ca-90ac-41ef-8542-21f1bf74d0b5"
  }
}
```

This call also succeeds with a `201 Created` response.

Now, we check the submission status using `GET` to fetch the following recource:

```
v1/transaction/payments/d596fa25-6943-4490-b94f-61f4c44a8941/submissions/abf05458-69de-4949-9c5d-8ed3c15d966a
```

The response will contain an explanation of the error in the `status_reason` attribute.

## Payment Submission Fails Due to Missing Amount Attribute in the Payment Resource

```javascript
{
    "data": {
        "type": "payment_submissions",
        "id": "abf05458-69de-4949-9c5d-8ed3c15d966a",
        "version": 2,
        "organisation_id": "fa3c28ca-90ac-41ef-8542-21f1bf74d0b5",
        "attributes": {
            "status": "delivery_failed",
            "status_reason": "attributes.amount:may not be null. Value passed was null",
            "submission_datetime": "2022-12-11T15:45:10.887Z"
        },
        "relationships": {
            "payment": {
                "data": [
                    {
                        "type": "payments",
                        "id": "d596fa25-6943-4490-b94f-61f4c44a8941"
                    }
                ]
            }
        }
    }
}
```

## Invalid Receiving Account Data

Another problem can occur if all the attributes in the payment resource are correct, but the account number or sort code for the recipient are invalid.

Since the payment is formally correct, it will be submitted without an error, but an invalid sort code will be flagged by the transaction scheme, resulting in a `delivery_failed` status. Likewise, if the account number is invalid, the receiving bank will reject the payment, and an error message will be relayed back.

If a non-existing account number is specified, the receiving bank will reject the inbound payment and an error message will be returned.

In both cases, the `status` attribute of the submission resource will contain the status `delivery_failed`. The `status_reason` attribute in the submission resource will provide more detail on the specific error message, and this varies according to the payment scheme.

For example, an unknown account number or invalid sort code using Faster Payments will be indicated by the `status_reason` attribute as "beneficiary-sort-code/account-number-unknown".

## Triggering a Rejected Payment

You can use the [transaction simulator](https://www.api-docs.form3.tech/api/staging/transaction-simulator/overview) to create an outbound payment that will be rejected. For example, sending an FPS payment with the amount `216.16` in the test environment will trigger a failed delivery with the `status_reason` mentioned above.

[See here](https://www.api-docs.form3.tech/api/tutorials/getting-started/send-a-payment) to learn more about sending a payment.

Create a new Payment resource and set the `amount` field to `216.16`. Then, create a Payment Submission resource for this Payment.

Once the submission has been processed by Form3 and sent to the FPS scheme simulator, the submission status will change to `delivery_failed` and the `status_reason` attribute will contain an error message. Note that some schemes may also provide additional details in the `scheme_status_code` attribute.

## Example: Payment Submission Failed Due to Unknown Sort Code or Account Number

```javascript
{
    "data": {
        "type": "payment_submissions",
        "id": "190d8768-0775-4839-a6c5-73402029e5a3",
        "version": 4,
        "organisation_id": "f2098716-8c51-4843-9498-61bf3bd5d0f5",
        "attributes": {
            "status": "delivery_failed",
            "scheme_status_code": "1114",
            "status_reason": "beneficiary-sort-code/account-number-unknown",
            "submission_datetime": "2022-12-11T14:59:52.804Z",
            "settlement_date": "2022-12-11",
            "settlement_cycle": 1
        },
        "relationships": {
            "payment": {
                "data": [
                    {
                        "type": "payments",
                        "id": "08ee2634-7c99-4735-91e2-4ec070db2677"
                    }
                ]
            }
        }
    }
}
```
