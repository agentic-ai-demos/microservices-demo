---
type: Service
title: Frontend
description: Go HTTP server that renders the storefront and fans every page out to the backing gRPC services.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/frontend
tags: [http, storefront, grpc-client, session]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/frontend
generated_by: catalogify/0.8.0
open_questions:
  - "Is the session cookie the only client state, or do any handlers assume server-side affinity?"
  - "When a downstream service is unavailable, is the page degraded or failed outright, and which behaviour is contractual?"
---
# Responsibilities

The only service exposed to a browser. It renders server-side HTML templates and, for each page, fans out to the gRPC backends: catalog and ads for the home page, cart and currency for the cart, shipping for a quote, checkout to place an order. It holds no state of its own beyond a session cookie.

# Interfaces

| Route | Method and purpose |
| --- | --- |
| `/` | GET — home page: product list, ads, cart size, currency. |
| `/product/{id}` | GET — product detail with recommendations. |
| `/cart` | GET to view, POST to add an item. |
| `/cart/empty` | POST — clear the session cart. |
| `/cart/checkout` | POST — hand off to the checkout service. |
| `/setCurrency` | POST — persist a currency choice in a cookie. |
| `/assistant` | GET — shopping assistant page. |

# Dependencies

Derived from the gRPC clients constructed in `rpc.go` and `handlers.go`, and from the
`*_SERVICE_ADDR` variables the deployment injects. It calls, and is called by nothing:

* [Product Catalog Service](productcatalogservice.md) — listing and detail
* [Cart Service](cartservice.md) — read and mutate the session cart
* [Currency Service](currencyservice.md) — convert displayed prices
* [Recommendation Service](recommendationservice.md) — related products
* [Ad Service](adservice.md) — contextual ads
* [Shipping Service](shippingservice.md) — quote on the cart page
* [Checkout Service](checkoutservice.md) — place the order

All seven speak the [storefront gRPC contract](../apis/storefront-grpc-api.md).

# Gotchas

**Trace context is propagated unconditionally, by design.** `1c8abe20` removed the environment-variable gate so a service mesh or another process can inject context and have it survive the hop. The stated reason is that a span created upstream appears orphaned if this service drops the headers. The same commit deliberately leaves propagation out of leaf services that make no downstream calls.

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `1c8abe20` — Propagate trace context always (#1345).
2. `ed7b9419` — Revert "Add support for arm64 (#2589)".
3. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
