# Webhooks Reference

## Headers

| Header | Description |
| ------ | ----------- |
| `X-Webhook-Signature` | HMAC256 signature for verifying that the request originated from the Payments API. |
| `Content-Type` | `application/json` |

## Common Fields

These fields are present in any webhook payload regardless of the event type.

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| `eventId` | string | Unique webhook event identifier. | `evt_12345ABC` |
| `eventType` | string | The type of event. See [Event Types](#event-types) for possible values. | `payment.completed` |
| `created` | string | ISO 8601 timestamp of the event generation. | `2026-02-05T09:42:14Z` |
| `data` | object | A container for specific payment details (like amount, currency, status). | - |

## Payment Fields

These fields are specific to payment-related events.

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| `paymentId` | string | The unique ID of the payment this event relates to. | `pay_98765XYZ` |

### Event Types

`eventType` shows the current state of the payment process.

| Type | Description |
| ---- | ----------- |
| `payment.completed` | The payment was successfully authorized and captured. |
| `payment.failed` | The payment was declined by the external system or failed due to technical issues. |
| `payment.cancelled` | The payment has been cancelled by the user or the internal system before completion. |

## Responses

| Status Code | Source | Description |
| ----------- | ------ | ----------- |
| `200 OK` | Client | We received the webhook and stopped retries. |
| `401 Unauthorized` | Client | The signature verification failed. |
| `4XX / 5XX` | Client | Any error code triggers our retry policy. |
| `Timeout` | System | No response within 5 seconds triggers a retry. |

> **Note:** Your server should return an empty `200 OK` response since the Payments API does not process data in the response body.
