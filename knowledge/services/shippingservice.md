---
type: Service
title: Shipping Service
description: Go service returning shipping quotes and issuing tracking ids. Both values are simulated.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/shippingservice
tags: [go, shipping, quote, tracking]
timestamp: 2026-09-03T15:42:22-04:00
source_files:
  - src/shippingservice
generated_by: catalogify/0.8.0
open_questions:
  - "Is the quote formula meant to be plausible or merely non-zero? A caller writing assertions needs to know."
  - "Is the tracking id required to be unique, or only well-formed?"
---
# Responsibilities

Answers a quote for a cart and returns a tracking id when an order ships. Both are computed, not fetched from a carrier: this is a demo boundary standing in for a real integration.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `GetQuote` | Price a shipment for the items in the cart. |
| `ShipOrder` | Issue a tracking id for a placed order. |

# Dependencies

A leaf: it calls nothing. Called by [Frontend](frontend.md) for the cart-page quote and by
[Checkout Service](checkoutservice.md) during the order flow, over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

# Gotchas

**The quote arithmetic was wrong, and the randomness was seeded by hand.** `fc3a1f72` fixed
`GetQuote` to use the actual item count rather than a placeholder, and removed a manual
`rand.Seed` call that has been deprecated since Go 1.20 — the global generator seeds itself now,
so the seeded flag was dead weight pretending to be determinism. If you need reproducible
quotes for a test, inject a source rather than reinstating global seeding.

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `fc3a1f72` — clean up shippingservice: remove deprecated API, fix quote logic, improve tests (#3276).
2. `ed7b9419` — Revert "Add support for arm64 (#2589)".
3. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
