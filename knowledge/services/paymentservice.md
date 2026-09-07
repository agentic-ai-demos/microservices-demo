---
type: Service
title: Payment Service
description: The Node.js payment service mock-validates cards and returns transaction IDs.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/paymentservice/server.js
tags: [service, nodejs, grpc, payment]
timestamp: 2026-09-03T15:39:39-04:00
source_files:
  - src/paymentservice/server.js
  - src/paymentservice/charge.js
  - src/paymentservice/index.js
  - src/paymentservice/package.json
  - src/paymentservice/package-lock.json
  - src/paymentservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Are accepted card brands intentionally limited to Visa and Mastercard for demo UX, or should tests treat that as business policy?"
---

# Responsibilities

Payment service implements the `Charge` RPC and delegates validation/mock charging to `charge.js`. It validates card number, accepted brand, and expiration date, then returns a generated transaction ID instead of integrating with a real payment processor.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `HipsterShopServer` | gRPC server wrapper. |
| `charge` | Validates and mock-charges a request. |
| `CreditCardError` | Base error mapped to invalid-argument behavior. |
| `InvalidCreditCard` | Raised for invalid card details. |
| `UnacceptedCreditCard` | Raised for unsupported card brands. |
| `ExpiredCreditCard` | Raised for expired cards. |

# Dependencies

Payment implements [Storefront gRPC API](../apis/storefront-grpc-api.md) and is called by [Checkout Service](checkoutservice.md). It shares Node.js gRPC and observability dependency maintenance with [Currency Service](currencyservice.md).

# Gotchas

* Several payment dependencies have security-tagged updates, including `uuid`, `@grpc/grpc-js`, and OpenTelemetry; lockfile changes here are operationally significant, not noise (`5096a85b`, `01b3dbc0`, `1ea14802`).
* `charge.js` logs card type and last four digits, so logging changes should avoid expanding sensitive payment data exposure.

# Citations

1. `5096a85b` - fix(deps): update dependency uuid to v14 [security] (#3332).
2. `01b3dbc0` - fix(deps): update dependency @grpc/grpc-js to v1.14.4 [security] (#3388).
3. `1ea14802` - fix(deps): update dependency @opentelemetry/sdk-node to v0.217.0 [security] (#3357).
