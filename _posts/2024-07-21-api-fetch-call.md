## Fetch Recall Decision

Get a single Recall Decision resource.

#### HTTP Request

`v1/transaction/payments/{payment_id}/recalls/{recall_id}/decisions/{decision_id}`

### URL Parameters

`decision_id`
REQUIRED<br>
**string, unique identifier (UUID)**<br>
ID of the Payment Recall Decision resource to fetch.

`payment_id`
REQUIRED<br>
**string, unique identifier (UUID)**<br>
ID of the Payment resource that is being recalled.

`recall_id`
REQUIRED<br>
**string, unique identifier (UUID)**<br>
ID of the Payment Recall resource that is admitted.

### Response Attributes

`answer`
ALWAYS<br>
**string, enum**<br>
Answer to the recall request.

`agents`
SOMETIMES<br>
**object**<br>
Information around the Agent who has taken charges in relation to the recall.

`charges_amount`
SOMETIMES<br>
**object**<br>
Information about the charges.

`reason`
SOMETIMES<br>
**string**<br>
Further explanation of the reason given in reason_code. Only supported for certain reason codes, see the value descriptions in reason_code for which codes require a reason. Max 105 characters. Only applicable for FedNow.

`reason_code`
SOMETIMES<br>
**string, enum**<br>
Reason for a rejected decision. Populated when answer is rejected, otherwise ignored.
Must be a 4-character code allowed from the ISO 20022 externalized PaymentCancellationRejection1Code list .

`recall_amount`
SOMETIMES<br>
**object**<br>
Amount to be returned if the recall is partially accepted by the counterparty.

`resolution_related_information`
SOMETIMES<br>
**object**<br>
Information about the return transaction that is returning the payment.

### Response Relationships

`decision_admission`
ALWAYS
LINK RELATIONSHIP<br>
**object**<br>
Information about the Recall Decision Admission resource.

`decision_submission`
ALWAYS<br>
**object**<br>
Information about the Recall Decision Submission resource.

`payment`
ALWAYS<br>
**object**<br>
Information about the original Payment Recall resource that is being recalled.

`recall`
ALWAYS<br>
**object**<br>
Information about the Recall resource.

### Example - Fetch a Payment Recall Decision

```
GET v1/transaction/payments/c419bbef-f586-48b1-bbbd-7ef4aef21ec2/recalls/a74b9b67-fa61-4013-ab0e-a252963d4151/decisions/c15ee5ce-2d78-42e1-a03f-26805f526f8d
Accept: application/json
Host: api.form3.tech
Date: Mon, 10 June 2024 20:04:24 GMT
Authorization: Signature keyId="49a7aac7-3cd0-4eca-9697-b195fc2a898a",algorithm="rsa-sha256", headers="(request-target) accept host date", signature="dGVzdHNldHNldHNldHNldHNhZGFkYXNkIGkgdGFraWUgdGFt"
```

### Response

``` 
{
  "data": {
    "id": "9513e122-d8db-4947-92e0-4c3f4010afa7",
    "type": "recall_decisions",
    "organisation_id": "8cfe9afd-86c1-4caa-b7b8-c00fc43aeb6b",
    "attributes": {
      "answer": "rejected",
      "recall_amount": {
        "currency": "USD",
        "amount": "24.00"
      },
      "reason_code": "NARR",
      "reason": "Narrative reason provided here"
    },
    "relationships": {
      "payment": {
        "data": [
          {
            "type": "payments",
            "id": "c419bbef-f586-48b1-bbbd-7ef4aef21ec2"
          }
        ]
      },
      "recall": {
        "data": [
          {
            "type": "recalls",
            "id": "a74b9b67-fa61-4013-ab0e-a252963d4151"
          }
        ]
      },
      "decision_submission": {
        "data": [
          {
            "type": "recall_decision_submissions",
            "id": "786942cd-e886-47b3-be30-a336e1aaa2ba"
          }
        ]
      },
      "decision_admission": {
        "data": [
          {
            "type": "recall_decision_admissions",
            "id": "4c98e12c-c9df-48a6-9f06-5cadf8458b1c"
          }
        ]
      }
    }
  }
}
``` 
