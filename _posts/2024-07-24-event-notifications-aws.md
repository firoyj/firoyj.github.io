---
title: "API Docs: Amazon SQS"
mathjax: true
layout: post
categories: github, website
excerpt: "You can be notified of incoming transactions and other asynchronous events. For this we use a notification mechanism that allows you to specify a webhook URL or your own Amazon SQS queue to register for specific types of events.<br><br>This section details the required steps to set up a connection with Amazon SQS queues."
---

## Event Notifications

Excert from [Form3 API docs](https://www.api-docs.form3.tech/api/schemes/bacs/event-notifications/overview).

### Overview

You can be notified of incoming transactions and other asynchronous events. For this we use a notification mechanism that allows you to specify a webhook URL or your own Amazon SQS queue to register for specific types of events.

>**Please Note:<br>**
>This feature is designed to help you monitor resources across the entire Form3 API. Some examples and parameters in this section may not apply to single-scheme integrations.

Whenever an event occurs in the system, Form3 will either call the webhook or post to the queue.

The [subscription resource](https://www.api-docs.form3.tech/api/schemes/bacs/event-notifications/create-a-subscription) allows you to manage notification endpoints, and subscribe, cancel and query existing subscriptions.

You can configure a maximum of 150 notification endpoints (either `http` or `queue`) and filter across a number of different [events](https://www.api-docs.form3.tech/api/schemes/bacs/event-notifications/create-a-subscription). If you exceed this limit of 150 subscriptions, you will not be able to create new subscriptions but you can update existing subscriptions or delete them to create more.

All notifications are cryptographically signed so that you can verify that they originated from Form3. See our [tutorial](https://www.api-docs.form3.tech/api/tutorials/event-notifications/verify-event-notifications) on how to validate a notification's signature.

## SQS Setup

This section details the steps required to set up a connection with an Amazon SQS queue.

Use either the [AWS console](https://aws.amazon.com/console/) or the [AWS CLI tool](https://aws.amazon.com/cli/).

Contact Form3 to obtain the `AWS_ACCOUNT_ID` and `AWS_REGION` variables for the Form3 environment you're using.

`AWS_QUEUE_URL` is the URL of your SQS queue where the notification messages should be posted.

1. Using your AWS account, create an SQS queue:

```
aws sqs create-queue --queue-name acme-co --region AWS_REGION
```

This call returns the URL of the new queue. Note: This command sets the message size limit to the default maximum value. We do not recommend using a smaller message size limit than the maximum.

2\. Add the required permission to the queue:

```
aws sqs add-permission --aws-account-ids AWS_ACCOUNT_ID --actions SendMessage --queue-url AWS_QUEUE_URL --label foo --region AWS_REGION
```

For more details on AWS permissions, see the [AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_policy-examples.html#example-delegate-xaccount-SQS).

## SQS with Server-Side Encryption

In addition to the above steps:

Set the SQS queue to use an AWS KMS Customer Master Key (CMK).

1. Log in to the [AWS console](https://aws.amazon.com/console/)<br>
     i) Navigate to **KMS**.<br>
     ii) Select **Customer Managed Keys**.<br>
     iii) Locate the encryption key used by your notification queue.<br>
     iv) Go to the **Key Policy** tab and select **Edit**.

2. Append the following statement block into the existing key policy AWS created for you:

```
{
     "Sid": "Enable Form3",
     "Effect": "Allow",
     "Principal": {
         "AWS": "arn:aws:iam::AWS_ACCOUNT_ID:root"
     },
     "Action": [
         "kms:Decrypt",
         "kms:GenerateDataKey"
     ],
     "Resource": "*"
}
```

Contact Form3 to obtain the `AWS_ACCOUNT_ID` shown in the above call.

Adding this statement to the key policy enables the Form3 notification service to send messages to the encrypted SQS queue.

3\. Ensure your service consuming the SQS queue has the same KMS permissions to read the messages.

You've now set up your Amazon SQS queue with server-side encryption, enabling you to securely receive event notifications from Form3.
