## Fetch Recall Decision

Get a single Recall Decision resource.

### HTTP Request

`v1/transaction/payments/{payment_id}/recalls/{recall_id}/decisions/{decision_id}`

### URL Parameters

`decision_id`
REQUIRED  
**string, unique identifier (UUID)**  
ID of the Payment Recall Decision resource to fetch.

`payment_id`
REQUIRED  
**string, unique identifier (UUID)**  
ID of the Payment resource that is being recalled.

`recall_id`
REQUIRED  
**string, unique identifier (UUID)**  
ID of the Payment Recall resource that is admitted.

### Response Attributes

`answer`
ALWAYS  
**string, enum**  
Answer to the recall request.

<details>
  <summary>Show values</summary>
  <ul>
    <li><code>accepted</code>
      Recall has been accepted. If partially accepted then `recall_amount.amount` will advise what amount will be returned.
    </li>
    <li><code>pending</code>
      Recall is still pending. Used in FedNow only.
    </li>
    <li><code>rejected</code>
      Recall has been rejected.
    </li>
  </ul>
</details>

`agents`
SOMETIMES  
**object**  
Information around the Agent who has taken charges in relation to the recall.

<details>
  <summary>Show children</summary>
  <ul>
    <li><strong>.identification</strong>
      Information about the financial institution.
      <details>
        <summary>Show children</summary>  
        <ul>
          <li><code>bank_id</code>
            SOMETIMES  
            **string**  
            Identification code of the financial institution.
          </li>
          <li><code>bank_id_code</code>  
            SOMETIMES  
            **string**  
            The type of identification provided in `bank_id`. Always `USABA`.
          </li>
        </ul>
      </details>
    </li>
    <li><strong>.role</strong>  
      SOMETIMES  
      **string, enum**  
      Role of the agent in the payment chain. Enum of pre-defined values, new values can be added when needed, e.g. `InstructingAgent`.
      <details>
        <summary>Show values</summary>  
        <ul>
          <li><code>ChargesAgent</code>  
            Agent that takes the transaction charges or to which the transaction charges are due.
          </li>
        </ul>
      </details>
    </li>
  </ul>
</details>

`charges_amount`
SOMETIMES  
**object**  
Information about the charges.

<details>
  <summary>Show children</summary>  
  <ul>
    <li><code>amount</code>  
      SOMETIMES  
      **string**  
      Amount of charges to be taken for the recall and not returned to the recall originator.
    </li>
    <li><code>currency</code>  
      SOMETIMES  
      **string**  
      [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html) for the charges amount. Must be `USD`.
    </li>
  </ul>
</details>

`reason`
SOMETIMES  
**string**  
Further explanation of the reason given in `reason_code`. Only supported for certain reason codes, see the value descriptions in `reason_code` for which codes require a reason. Max 105 characters.

`reason_code`
SOMETIMES  
**string, enum**  
Reason for a rejected decision. Populated when answer is rejected, otherwise ignored. Must be a 4-character code allowed from the [ISO 20022 externalized PaymentCancellationRejection1Code](https://www.iso20022.org/catalogue-messages/additional-content-messages/external-code-sets) list.

`recall_amount`
SOMETIMES  
**object**  
Amount to be returned if the recall is partially accepted by the counterparty.

`resolution_related_information`
SOMETIMES  
**object**  
Information about the return transaction that is returning the payment.

### Response Relationships

`decision_admission`
ALWAYS  
**object**  
Information about the Recall Decision Admission resource.

`decision_submission`
ALWAYS  
**object**  
Information about the Recall Decision Submission resource.

`payment`
ALWAYS  
**object**  
Information about the original Payment Recall resource that is being recalled.

`recall`
ALWAYS  
**object**  
Information about the Recall resource.

### Example - Fetch a Payment Recall Decision

```
GET v1/transaction/payments/c419bbef-f586-48b1-bbbd-7ef4aef21ec2/recalls/a74b9b67-fa61-4013-ab0e-a252963d4151/decisions/c15ee5ce-2d78-42e1-a03f-26805f526f8d
Accept: application/json
Host: api.form3.tech
Date: Mon, 10 June 2024 20:04:24 GMT
Authorization: Signature keyId="49a7aac7-3cd0-4eca-9697-b195fc2a898a",algorithm="rsa-sha256", headers="(request-target) accept host date", signature="dGVzdHNldHNldHNldHNldHNhZGFkYXNkIGkgdGFraWUgdGFt"
```

### Example - Response

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
