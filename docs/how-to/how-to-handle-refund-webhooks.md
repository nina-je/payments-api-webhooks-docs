# How to handle refund webhooks

## Purpose

This guide explains how to handle refund webhook deliveries and update the customer's account balance.

## Prerequisites

- Publicly accessible HTTP endpoint.
- Database or cache to store and track event IDs.
- Ability to parse JSON payloads.
- Active API key for signature verification.

## Steps

### Step 1: Verify the request signature

When your endpoint receives an HTTP POST request, verify the `X-Webhook-Signature` header using your secret key and HMAC SHA256. If it is invalid, reject it with a `401 Unauthorized` status code.

**Header example:**

```HTTP
X-Webhook-Signature: t=1770201201,v1=5b7f1e948c2d3a6b5e0f9a2c4d8e7b1a6c5f0e9d8b7a6c5d4e3f2b1a0d9c8b7a
```

### Step 2: Extract the event data

Extract the JSON body from the request. The `paymentId` identifies the payment related to the refund, while the `refundId` identifies the refund request.

**Payload example:**

```JSON
{
  "eventId": "evt_999888777",
  "eventType": "refund.succeeded",
  "paymentId": "pay_98765XYZ",
  "refundId": "ref_4567MNO",
  "created": "2026-02-10T14:30:00Z",
  "data": {
    "amount": 50.00,
    "currency": "EUR",
    "reason": "customer_request"
  }
}
```

### Step 3: Check for duplicate events

Check if the `eventId` is already in your database or cache. If it is, ignore it and return a `200 OK` response to stop unnecessary retries. This ensures idempotency.

### Step 4: Handle the event logic

Process the event based on its `eventType` (e.g., `refund.created`, `refund.succeeded`, `refund.failed`) according to your business logic.

### Step 5: Store the event record

Store the processed `eventId` internally. This is required for the idempotency check in Step 3.

### Step 6: Return a response

Send a `200 OK` HTTP response. Make sure your server responds within 5 seconds to prevent the Payments API from triggering a retry.

## Delivery flow overview

```mermaid
%%{init: { "sequence": { "mirrorActors": false } } }%%
sequenceDiagram
    autonumber
    participant csys as Client<br/>System
    participant isys as Internal<br/>System
    participant esys as External<br/>System

    csys ->> isys: Request refund (API Call)
    isys ->> esys: Process refund request
    esys ->> esys: Verify funds &<br/>execute refund
    esys -->> csys: Webhook event: refund.succeeded<br/>(HTTP POST)
    csys ->> csys: Identify event,<br/>check idempotency
    alt Already processed
        csys -->> esys: 200 OK
    else Not processed
        csys ->> csys: Update internal balance
        csys ->> csys: Save result
        csys -->> esys: 200 OK
    end
```
