# Payments API Webhooks Documentation

This repository is my technical writing portfolio. I use a **docs-as-code** workflow and follow the **Diátaxis** framework.

When writing these examples, I assumed a role of a Technical Writer in a fictional company that develops a **Payments API**, so all examples are illustrative. The documentation is written for imaginative backend developers integrating our API into their, also fictional, infrastructure.

## Document Map

* **[Tutorial](./docs/tutorial/receive-your-first-webhook.md)**: A step-by-step guide to receiving your first test webhook.
* **[How-to Guide](./docs/how-to/how-to-handle-payments-webhooks.md)**: Practical instructions on handling deliveries and ensuring idempotency.
* **[Explanation](./docs/explanation/why-webhooks-are-delivered-at-least-once.md)**: Conceptual background on at-least-once delivery and retry policies.
* **[Reference](./docs/reference/webhooks-reference.md)**: Technical specifications for payloads, headers, and status codes.

## Tools

This repository contains a [Postman collection](./tools/postman/webhooks-collection.json) that helps developers to quickly test and verify webhook deliveries.
