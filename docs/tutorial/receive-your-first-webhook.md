# Receive your first webhook

## Purpose

In this tutorial, we will learn how to set a temporary endpoint and inspect a test webhook payload. By the end of this tutorial, you will be able to successfully receive and check webhook payloads.

## Prerequisites

- A unique webhook.site URL: your temporary endpoint
- Postman: an active account (web or desktop)
- Test collection: [webhooks-collection.json](../../tools/postman/webhooks-collection.json) file included in this repository.

## Steps

### Step 1: Setting up an endpoint

First, we need to set a temporary endpoint using a [webhook.site](https://webhook.site/). When you navigate to this website, you will automatically get a unique URL that acts as your endpoint that receives a webhook from the Payments API. Copy your unique URL and keep this browser window open.

### Step 2: Sending test event in Postman

We will use Postman to simulate our Payments API since we are in a testing environment.

1. Open Postman and click the **Import** button in the left sidebar, next to your workspace name.
2. Select and upload the [webhooks-collection.json](../../tools/postman/webhooks-collection.json) file.
3. In the **Collections** tab on the left, expand the **Payments API Webhooks** collection and select the **Send Test Webhook** request.
4. Paste your unique URL in the field next to the `POST` method.
5. Click **Send**, and you will see a green `200 OK` status in the response pane at the bottom.

### Step 3: Verifying the event

Finally, to verify the event, navigate back to your endpoint (webhook.site), and you will see a new POST request.

- In the **Request Details & Headers** section, check X-Webhook-Signature to see the HMAC security token.
- In the **Raw Content** section, you will see the JSON body that contains details for a specific event.
