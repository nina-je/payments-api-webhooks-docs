# Payments API webhooks documentation

This repository is my technical writing portfolio. I use a **docs-as-code** workflow and follow the **Diátaxis** framework.

When writing these examples, I assumed a role of a Technical Writer in a fictional company that develops a **Payments API**, so all examples are illustrative. The documentation is written for imaginative backend developers integrating our API into their, also fictional, infrastructure.

## Document map

* **Tutorial**: A step-by-step guide to receiving your first test webhook.
  * [Receive your first webhook](./docs/tutorial/receive-your-first-webhook.md)  
* **How-to guides**: Practical instructions on handling deliveries and ensuring idempotency.
  * [How to handle payments webhooks](./docs/how-to/how-to-handle-payments-webhooks.md)
  * [How to handle refund webhooks](./docs/how-to/how-to-handle-refund-webhooks.md)
* **Explanation**: Conceptual background on at-least-once delivery and retry policies.
  * [Why webhooks are delivered at least once](./docs/explanation/why-webhooks-are-delivered-at-least-once.md)
* **Reference**: Technical specifications for payloads, headers, and status codes.
  * [Webhooks reference](./docs/reference/webhooks-reference.md)

## Tools

This repository contains a [Postman collection](./tools/postman/webhooks-collection.json) that helps developers to quickly test and verify webhook deliveries.
