---
type: Service
title: Checkout Service
description: The Go checkout service orchestrates cart, catalog, currency, payment, shipping, and email calls into an order.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/checkoutservice/main.go
tags: [service, go, grpc, checkout]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/checkoutservice/main.go
  - src/checkoutservice/go.mod
  - src/checkoutservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Should checkout empty the cart before or after all downstream side effects succeed, or is the current ordering only demo behavior?"
---

# Responsibilities

Checkout owns the cart-to-order workflow. It opens gRPC client connections to product catalog, cart, currency, shipping, email, and payment services; `PlaceOrder` prepares order items, totals localized money, charges the card, ships the order, sends confirmation, and empties the user cart.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `PlaceOrder` | Main CheckoutService RPC implementation. |
| `prepareOrderItemsAndShippingQuoteFromCart` | Reads cart contents, loads product data, and gets localized shipping cost. |
| `quoteShipping` | Calls shipping `GetQuote`. |
| `getUserCart` | Calls cart `GetCart`. |
| `emptyUserCart` | Calls cart `EmptyCart`. |
| `prepOrderItems` | Resolves cart product IDs into order items and localized costs. |
| `convertCurrency` | Calls currency `Convert`. |
| `chargeCard` | Calls payment `Charge`. |
| `sendOrderConfirmation` | Calls email `SendOrderConfirmation`. |
| `shipOrder` | Calls shipping `ShipOrder`. |

# Dependencies

Checkout is the densest backend orchestration point and depends on [Cart Service](cartservice.md), [Product Catalog Service](productcatalogservice.md), [Currency Service](currencyservice.md), [Payment Service](paymentservice.md), [Shipping Service](shippingservice.md), [Email Service](emailservice.md), and [Storefront gRPC API](../apis/storefront-grpc-api.md).

History shows high-lift co-change with [Frontend Service](frontend.md), [Product Catalog Service](productcatalogservice.md), and [Shipping Service](shippingservice.md), consistent with checkout form shape, product pricing, and shipping totals moving together.

# Gotchas

* Trace propagation crosses checkout and multiple downstream services; replacing gRPC setup must preserve both client and server OpenTelemetry handlers (`1c8abe20`).
* Docker platform changes have previously been reverted across all service images, so checkout image architecture changes should be tested as part of the full Skaffold build (`ed7b9419`).

# Key files

| Path | Role |
| --- | --- |
| `src/checkoutservice/main.go` | All checkout server and orchestration code. |

# Citations

1. `1c8abe20` - Propagate trace context always (#1345).
2. `ed7b9419` - Revert "Add support for arm64 (#2589)".
