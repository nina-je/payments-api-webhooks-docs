# Webhooks reference

## Headers

| Header | Description |
| ------ | ----------- |
| `X-Webhook-Signature` | HMAC256 signature for verifying that the request originated from the Payments API. |
| `Content-Type` | `application/json` |

## Common fields

These fields are present in any webhook payload regardless of the event type.

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| `eventId` | string | Unique webhook event identifier. | `evt_12345ABC` |
| `eventType` | string | The type of event. See [Event types](#event-types) for possible values. | `payment.completed` |
| `paymentId` | string | The unique ID of the payment the event relates to. | `pay_98765XYZ` |
| `created` | string | ISO 8601 timestamp of event generation. | `2026-02-05T09:42:14Z` |
| `data` | object | A container for specific payment details (like amount, currency, or reason). | - |

## Payment-specific fields

These fields are contained in the `data` object when the `eventType` is in the `payment.*` category.

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| `data.amount` | number | Payment amount. | `5000.00` |
| `data.currency` | string | ISO 4217 currency code. | `EUR` |
| `data.status` | string | Current transaction state. | `succeeded` |

## Refund-specific fields

These fields are related to the `eventType` in the `refund.*` category.

**Top-level**

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| `refundId` | string | Unique refund request identifier. | `ref_4567MNO` |

**`data` object**

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| `data.amount` | number | Refund amount. | `50.00` |
| `data.currency` | string | ISO 4217 currency code. | `EUR` |
| `data.reason` | string | Reason for the refund. | `customer_request` |

### Event types

`eventType` shows the current state of the related process.

| Type | Description |
| ---- | ----------- |
| `payment.completed` | The payment was successfully authorized and captured. |
| `payment.failed` | The payment was declined by the external system or failed due to technical issues. |
| `payment.cancelled` | The payment has been cancelled by the user or the internal system before completion. |
| `refund.created` | The refund request is initiated. |
| `refund.succeeded` | Funds are successfully returned to the customer. |
| `refund.failed` | The refund request is declined by the external system. |

## Responses

| Status Code | Source | Description |
| ----------- | ------ | ----------- |
| `200 OK` | Client | We received the webhook and stopped retries. |
| `401 Unauthorized` | Client | The signature verification failed. |
| `4XX / 5XX` | Client | Any error code triggers our retry policy. |
| `Timeout` | System | No response within 5 seconds triggers a retry. |

> **Note:** Your server should return an empty `200 OK` response since the Payments API does not process data in the response body.
