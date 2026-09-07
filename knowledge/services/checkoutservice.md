---
type: Service
title: Checkout Service
description: Go service that orchestrates placing an order across cart, catalog, currency, shipping, payment and email.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/checkoutservice
tags: [go, orchestration, order, grpc]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/checkoutservice
generated_by: catalogify/0.8.0
open_questions:
  - "`PlaceOrder` charges before shipping and empties the cart after. If ShipOrder fails post-charge, is the charge reversed anywhere, or is the order left inconsistent?"
  - "Is the step order itself contractual, or an implementation detail a refactor may change?"
---
# Responsibilities

Owns the one multi-service transaction in the system. `PlaceOrder` reads the cart, prices it through the catalog and currency services, obtains a shipping quote, charges the card, ships the order, empties the cart and sends a confirmation. It is a sequential orchestration, not a saga: there is no compensating action if a later step fails.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `PlaceOrder` | The whole order flow, end to end. The only method on this service. |

# Dependencies

Constructed in `main.go`; the widest fan-out in the repository.

* [Cart Service](cartservice.md) — read then empty
* [Product Catalog Service](productcatalogservice.md) — price the items
* [Currency Service](currencyservice.md) — convert to the order currency
* [Shipping Service](shippingservice.md) — quote, then ship
* [Payment Service](paymentservice.md) — charge
* [Email Service](emailservice.md) — confirmation

Called by [Frontend](frontend.md) over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

# Gotchas

**Trace context is propagated unconditionally, by design.** `1c8abe20` removed the environment-variable gate so a service mesh or another process can inject context and have it survive the hop. The stated reason is that a span created upstream appears orphaned if this service drops the headers. The same commit deliberately leaves propagation out of leaf services that make no downstream calls.

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `1c8abe20` — Propagate trace context always (#1345).
2. `ed7b9419` — Revert "Add support for arm64 (#2589)".
3. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
