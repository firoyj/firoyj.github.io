> ## Background to writing Quickstart<br>
> When I wrote this Quickstart guide, I had a specific user in mind: "Joe" is a junior developer juggling a heavy backlog, pressed for time, and with a lot riding on getting this integration done quickly. Maybe Joe didn’t sleep well. Maybe his manager is stressed too. I wanted this guide to feel like a helping hand, not another burden. My goal was to make every step fast, clear and reassuring, so Joe can get the integration done and move on to his other priorities.<br><br>I took this project over from a colleague who had carried out initial research, competitor analysis and discovery. Building on that foundation, and my past experience writing API documentation, I drove the overhaul end-to-end, leading more in-depth research, detailed stakeholder reviews and the full content creation. I was the sole technical writer responsible for this work, with assistance from an engineer to verify the Java code examples. This Quickstart guide was part of a comprehensive [documentation package](https://api-docs.fortris.com/) that fully replaced the old API docs you can see here [here](/assets/img/old-api-docs.pdf)

# Quickstart

Welcome. If you're new to the Fortris API, this guide will help you make your first successful API call in under five minutes.

You'll use **test credentials** and a **dedicated test account** within our production environment — giving you access to real endpoints in a secure, controlled environment.

All requests are **cryptographically signed** using HMAC. To access the API, your IP address must be whitelisted. Contact your Fortris Integration Lead to confirm your setup.

> Once your first call succeeds, you'll know your credentials, headers and payloads are valid — and you're ready to build confidently.

The Fortris API is designed for **payment execution** at scale for both fiat and cryptocurrency transactions.

This guide describes **Version 3** (V3) of the API, which follows standard **REST** methods like `GET` and `POST`.

---

## What you need before you begin

Before sending your first API call, make sure you have:

- received your **API key** and **test credentials** from your Integration Lead
- been granted access to your **test businesses** (one for `BTC` and one for `Altcoin`)
- at least **one test account enabled** for payouts or deposits (within those businesses above)
- decrypted your encrypted secret key
- had your **IP address whitelisted** by the Fortris team
- registered a **callback URL** (optional) if your flow expects asynchronous updates

If you're missing any of the above, contact your Integration Lead. For more setup details on how to manage credentials, configure IP whitelisting, and enforce MFA, see Security and access control.

---

## Key terms you'll use in this guide

Before we walk through your first API call, these are a few terms you'll see in the examples. You don’t need to know everything about them yet, just enough to follow along. These are the core objects the Fortris API works with:

**Account** — The source or destination of funds. You'll need an account ID to create payouts or deposits.

**Payout** — A request to send funds from your Fortris account to an external or internal destination. In the Treasury UI, this is known as a **send order**.

**Deposit** — A request to receive funds into your Fortris account. In the Treasury UI, this is known as a **receive order**.

**Resource ID** — A unique identifier used to track an API object, such as a payout or deposit. This may be:

- the `payoutId` or `depositId` (returned when the object is created)
- the `reference` (a value you set yourself when submitting the request)
- or another system-assigned ID such as `sendOrderId`

The reference or resource ID can be used to query status or debug issues, depending on your flow.

**Status** — Each payout or deposit moves through a lifecycle, like `REQUESTED`, `CREATED`, `COMPLETED` or `CREATE_FAILED`. You can check statuses manually or receive updates via callback.

**Callback** — A notification sent to your server when something changes, like a payout being approved or a deposit being confirmed. These are optional, but highly recommended.

For a full breakdown of how the Fortris API is structured, see Core concepts in the Overview.

---

## Where to send your API requests

Every API request begins with a **base URL** — the root part of the API address that all endpoint paths build on.

Since Fortris operates multiple production environments, your exact base URL depends on your account and region.

You'll receive your specific base URL from your Integration Lead. It will look like:

| Environment	| Base URL |
|---------------|--------------------|
| Staging      | `https://staging-url.com` |
| Production	  | `https://your-base-url.com`  |

To construct a full API request URL:

All endpoint paths (for example, `/v3/accounts/balance`) are relative to the base URL. In other words, to form the full request URL, you need to add the base URL (for example, `https://your-base-url.com`) in front of the endpoint path.

For example, a balance query in the production environment looks like this: `https://your-base-url.com/v3/accounts/balance?queryDate=2025-07-02T14%3A15%3A22Z&accountIds=23d577a4-f9d3-4eb7-bb2f-e5f5867caedf,17f1e5a9-813b-4f33-922c-5a1e87dc3322`

You may include one or more account IDs in the query string, separated by commas.

> Access to your base URL is restricted to approved IP addresses for security. These IPs must be whitelisted with Fortris ahead of time. Even with valid credentials, requests from unapproved networks will fail. Confirm your IP access with your Integration Lead.

---

## Required headers for authentication

Headers are the key-value pairs included at the top of an API request. They provide **metadata** about the request or instructions to the server on how to handle it.

The Fortris API uses **HMAC-based authentication**. Every request must include these headers:

| Header |Value	| Required for |
|---------------|----------------|-----------------------------------------|
| `Key`	| Your public API key (provided by Fortris). Identifies your client account.	| All requests |
| `Signature` |	A unique HMAC-SHA512 signature. Generated using your secret key.	| All requests |
| `Content-Type` |	Always set to `application/json`. Specifies the format of the request body	Only for requests with a body | (`POST`, `PUT`) |

**Notes:**

- Headers are **not** case-sensitive
- Fortris does not use basic authentication (username/password) or OAuth. Instead, it uses **HMAC-based authentication** — with `Key` and `Signature` passed as headers.

> You only need `Content-Type: application/json` for requests that include a body (`POST` or `PUT`). It's not required for `GET` or `DELETE` but may appear in examples for consistency.

---

## When and how to use a nonce

A nonce is a unique value **used only once**. It's included in requests that change state to protect against replay attacks.

Include a nonce in every request that changes state (`POST` and `PUT`). It must appear in the **body** or **query string**, depending on the endpoint — never as a header.

Always use a unique **value per request** — even if the other fields are identical.

**Recommended format:**

Use the **current UTC timestamp in milliseconds** (for example, `1720100123456`). For high-volume systems, use **nanoseconds** to ensure uniqueness across rapid requests.

> You can find the exact format and placement of the nonce for each endpoint in the API Reference.

---

## How to generate your signature

Every request to the Fortris API must be securely signed to prove it's authentic and hasn't been tampered with in transit.

The signature is a cryptographic **HMAC-SHA512 hash**, calculated using your **decrypted secret key** and the request data. It's included in the `Signature` header of every API call, and is required for access.

---

## How to decrypt your secret key

Before generating a signature, you normally need to **decrypt the API** secret provided by Fortris. You'll use the following two files:

1. `privatekey.pem` — Your private RSA key, generated as part of your keypair. You'll send the matching public key to Fortris during onboarding.
2. `encrypted_secret.b64` — Your encrypted API secret, typically delivered as a Base64-encoded file. It's encrypted using your public key so only you can decrypt it.

#### Step 1: Convert Base64-encoded secret to binary

In most cases, Fortris provides your secret as a `.b64` file. You must decode it into a binary `.bin` file before decrypting.

```
base64 --decode -i encrypted_secret.b64 -o encrypted_secret.bin
```

#### Step 2: Decrypt the binary secret using your private key

Use [OpenSSL](https://www.openssl.org/) to decrypt the binary file and retrieve your plaintext secret:

```
openssl rsautl -decrypt -inkey privatekey.pem -in encrypted_secret.bin -out secret.txt
```

Avoid writing your decrypted secret to disk or exposing it in logs or version control. Load it directly into memory using a secure secrets manager or environment variable.

> If your secret was already delivered as a `.bin` file, you can skip this decoding step. For a full walkthrough of key setup and decryption, see How to authenticate.

### Java example: Generate an HMAC-SHA512 signature:

Now that you've decrypted your API secret, you can use it to generate a signature.

The example below shows how to build a reusable signature function in Java:

```java
// Import Apache Commons Codec dependency
import org.apache.commons.codec.digest.DigestUtils;
import org.apache.commons.codec.digest.HmacAlgorithms;
import org.apache.commons.codec.digest.HmacUtils;

import java.util.Base64;

class SignatureGenerator {

    public String generateSignature(String url, String body, String secretBase64Encoded) {
        // Generate hash of the payload
        String sha256Hex = body != null && !body.isEmpty() // Check if the payload is not empty
                ? DigestUtils.sha256Hex(body) // Use DigestUtils to generate the hash
                : ""; // If the payload is empty, return an empty string. No need to generate a hash for empty or null body

        // Concatenate url with body hash (empty string if body is null or empty)
        String payloadToBeSigned = url + sha256Hex;

        // Decode the base64-encoded secret
        var keyInBytes = Base64.getDecoder().decode(secretBase64Encoded);

        // Use HmacUtils to generate the signature
        return new HmacUtils(HmacAlgorithms.HMAC_SHA_512, keyInBytes).hmacHex(payloadToBeSigned);
    }
}
```

This method expects the following inputs:

- `url`: the full request path (and query string, if present)
- `body`: the raw JSON body (if applicable)
- `secretBase64Encoded`: your decrypted secret, still in Base64 format

---

### What to include in your signature (based on request type)

Your signature must cover specific parts of the request — depending on the HTTP method.

- for `GET` and `DELETE` (requests without a body), you sign the **full request path** including query parameters
- for `POST` and `PUT` (requests with a body), you sign the **path plus the raw JSON body**

The result is a cryptographic **HMAC-SHA512 hash** created using your decrypted secret key.

#### 1. For GET and DELETE requests

`GET` and `DELETE` requests do not include a body, so you should base your signature only on the full request path including any query parameters.

Here's what you need to sign:

```
/v3/accounts/balance?queryDate=2025-07-02T14%3A15%3A22Z&accountIds=23d577a4-f9d3-4eb7-bb2f-e5f5867caedf,17f1e5a9-813b-4f33-922c-5a1e87dc3322
```

Use your decrypted secret key to generate an HMAC-SHA512 signature of the exact string above (including punctuation and encoding). The resulting hash is your `Signature` header.

#### 2. For POST and PUT requests

These requests include a body. Your signature must cover:

- the **full request path** (without query string)
- the **exact hashed body payload as a raw JSON string**

You concatenate the two and sign the combined string.

Request line in HTTP and request payload in JSON:

```http
POST /v3/deposits
```

```json
{
  "accountId": "3d07c219-0a88-45be-9cfc-91e9d095a1e9",
  "nonce": 1720100123456,
  "reference": "test-deposit-001"
}
```

Your signature input is the request path = "/v3/deposits" concatenated with the raw, serialized JSON body.

If you want to create deposits for multiple accounts, you need to send a **separate request for each one** — don't include multiple IDs in a single call.

> Make sure the body is serialized consistently. Variations in whitespace or field order can cause the signature to fail.

### Summary: How to get your signature

1. Decrypt your secret key (see above)
2. Assemble the string to sign:<br>a) `GET`, `DELETE` — full request path with query string<br>b) `POST`, `PUT` — request path + raw JSON body
3. Generate an HMAC-SHA512 signature using your decrypted secret
4. Include the resulting hash in the Signature header of your request

If your request changes state (`POST`, `PUT`), you'll also need to include a unique nonce. Learn more about nonces [here](#when-and-how-to-use-a-nonce).

---

## Validate your credentials

Before sending your first transaction, you can optionally test that your credentials and headers are working. The most reliable non-destructive test is to retrieve your account balance.

To do that, send a `GET` request to the `/v3/accounts/balance` endpoint with the following two query parameters:

- `accountIds` — the UUID of an account you have access to
- `queryDate` — current UTC timestamp in ISO 8601 format

**Java example:** `GET` request to retrieve balance:

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Instant;

public class FortrisBalanceRequest {

    // Create an instance of SignatureGenerator shown in the previous example
    private static SignatureGenerator signatureGenerator = new SignatureGenerator();

    public static void main(String[] args) throws Exception {
        String fortrisApiServer = "https://your-base-url.com";

        String clientKey = "your-client-key";
        String secret = "your-secret";

        String accountId = "your-account-id";
        Instant queryDate = Instant.now();
        String apiUrl = "%s/v3/accounts/balance?queryDate=%s&accountIds=%s".formatted(fortrisApiServer, queryDate, accountId);

        URI uri = URI.create(apiUrl);
        String pathWithQuery = uri.getPath() + "?" + uri.getQuery(); // String to be signed

        // Create an HttpClient
        HttpClient client = HttpClient.newHttpClient();

        // Create a GET request
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(apiUrl))
                .GET()
                .header("Key", clientKey)
                .header("Signature", signatureGenerator.generateSignature(pathWithQuery, "", secret))
                .build();

        // Send the request and get the response as a string
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

        // Print status and body
        System.out.println("Status: " + response.statusCode());
        System.out.println("Response: " + response.body());
    }
}
```

This request checks that:

- your API key is valid
- your signature is correctly generated
- your IP is allowed to access the API

> This `GET` request does not require a nonce. Only state-changing operations (`POST`, `PUT`) require one.

If the call succeeds, you'll receive a `200 OK` response with the balance of the specified account.

If not, check your headers and keys:
| Status Code   | What it means  |
|---------------|----------------|
| `401 Unauthorized`	| Signature is invalid, missing or doesn't match the request. Double-check your HMAC and decrypted secret. |
| `404 Not Found` |	Your API key is not registered, or the client does not exist. |
| No response |	Your IP is not whitelisted. Talk to your Integration Lead to check setup. |

If this call returns `200 OK`, your credentials and headers are correctly configured and you're ready to make your first transaction.

---

## Make your first call

Now you're ready to send your first API request.

Use the example below to make a request to retrieve an account balance using the production base URL and your test credentials.

**Example request (bash/cURL):**

```bash
curl -X GET "https://your-base-url.com/v3/accounts/balance?queryDate=2025-07-02T14%3A15%3A22Z&accountIds=23d577a4-f9d3-4eb7-bb2f-e5f5867caedf,17f1e5a9-813b-4f33-922c-5a1e87dc3322" \
  -H "Key: your_api_key" \
  -H "Signature: your_hmac_signature"
```

**Notes:**

- This `GET` request retrieves the balance for one or more accounts you're authorized to use.
- Replace `accountIds` with one or more of your real account IDs (in UUID format). If you're querying multiple accounts, separate the IDs with commas, for example: `accountIds=uuid1,uuid2,uuid3`
- Replace `queryDate` with the current UTC timestamp in ISO 8601 format (for example, `2025-07-02T14:15:22Z`).
- You don't need to include a nonce for this request. Nonce is only required for requests that change state (`POST`, `PUT`) — not for `GET` and `DELETE` requests.

This `GET` request asks the API to return balance details for one or more accounts you specify. It doesn't create or modify any resources — so it's safe to use for validation or testing.

If the request is successful, you'll receive a `200 OK` response with the balance information for each account.

> Make sure you calculate the signature from the **full request path and body** (if present), using your decrypted secret. See How to authenticate for guidance.

---

### Minimum transaction amount (threshold)

The minimum allowed amount for API transactions is **$1.00**. Requests below this threshold (for example, $0.99) will be rejected.

---

### Troubleshooting common errors

If your request fails, it's typically due to credentials or headers. Check the following common causes:

| Error code |	What it usually means |
|----------------------|-----------------------------------------------------|
| `400 Bad Request` |	One or more required fields are missing or malformed. |
|`401 Unauthorized`	| Signature is invalid, missing or doesn't match the request. Double-check your HMAC and decrypted secret. |
|`403 Forbidden`	| Your IP address isn't whitelisted by Fortris. |
|`404 Not Found`	| The accountId may be invalid or unregistered. |
|`422 Unprocessable Entity`	| The request was structurally correct but failed validation.<br>This can happen if:<br> • a required field is missing or incorrectly formatted<br> • a business rule is violated (for example, the account is not in `ACTIVE` state, the amount is below the $1.00 threshold or the currency doesn't match the account type) |
| No response / timeout |	Your IP may not be whitelisted, or the server rejected the connection due to invalid headers. |

---

## Successful response: 200 OK

If your request is valid, you'll receive a `200 OK` status and a JSON response with your account balance details. This confirms that your credentials, IP and signature are correct — and that you're ready to build on this integration.

This status indicates that the request was correctly authenticated, properly signed and the account IDs provided were valid. If you receive a different response, check your credentials, signature or request format.

---

## What your credentials should look like

Use the test keys provided during onboarding. If you haven't received them yet, contact your Integration Lead.

Use the table below to see how your credentials and IDs should be structured. The following examples are for illustration only, your credentials will be different.

| Field	| Example value	| Purpose |
|-----------|------------|------------|
| API key	| `live_key_example_abc123`	| Public key provided by Fortris for all API requests. |
| Encrypted secret key |	`U2VjdXJlU2VjcmV0Tm90UmVhbA==`	| Encrypted secret used to generate the signature. |
| Decrypted secret key	| `example-decrypted-secret` |	Used for HMAC-SHA512 signature generation. |
| Account ID (USD)	| `acc_live_usd_001`	| Required for payouts or deposits. The account currency depends on your configuration (not always USD). Use your real account ID(s) here. |
| Customer ID	| `client-456789`	| Used to associate the request with your customer. |
| Callback URL |	`https://clientapp.example.com/api/callbacks`	| Your HTTPS endpoint to receive asynchronous events. |
| OTP code (example only)	| `123456`	| Used when approving payouts. Always required. Include the OTP in /v3/payouts/{id}/authorize or use authorize=true at creation. |

> These are example values, they won't work in live requests. Use your assigned credentials when integrating.

---

## Authorize payouts with MFA

All payouts require **Multi-factor authentication (MFA)** as part of the approval process. You must supply a valid OTP (one-time password) when calling the `/v3/payouts/{payoutId}/authorize` endpoint, or when using `authorize=true` in a payout creation request.

Approval flows are tied to account-level permissions. You'll need:

- your API credentials
- a valid OTP code from your MFA device or app
- the correct payload structure as defined in the API specification

For details on how payout approvals work, see Approval models (coming soon). If you're unsure how to retrieve or use your OTP, contact your Integration Lead.

---

## Recap

In this Quickstart guide, you will have:

1. Set up your credentials and decrypted your secret key.
2. Generated a valid signature using your decrypted secret.
3. Sent your first authenticated API request using a real endpoint.
4. Verified that your credentials, IP and headers are working correctly.

---

This documentation describes API V3.

---

## What's next?

Now that you've completed your first API call, here are a few suggested next steps:

- explore the full API Reference to see all available endpoints and required fields
- see Authentication and security overview to understand the security model behind signed requests
- explore Core concepts to learn how deposits, payouts and accounts work
- configure IP access and MFA in Security and access control
- refer to Approval models to understand how multi-factor approval works for secure transactions and how to set up MFA (coming soon)
