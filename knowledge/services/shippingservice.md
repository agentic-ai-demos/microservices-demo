---
type: Service
title: Shipping Service
description: The Go shipping service estimates shipping cost and produces mock shipment tracking IDs.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/shippingservice/main.go
tags: [service, go, grpc, shipping]
timestamp: 2026-09-03T15:42:22-04:00
source_files:
  - src/shippingservice/main.go
  - src/shippingservice/quote.go
  - src/shippingservice/tracker.go
  - src/shippingservice/go.mod
  - src/shippingservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Is the shipping quote formula deliberately stable for demos and screenshots, or may it change with no compatibility concern?"
---

# Responsibilities

Shipping service implements quote estimation and mock shipment creation for checkout. It calculates `Money` from cart item count and produces tracking identifiers without calling an external carrier.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `GetQuote` | Implements quote estimation RPC. |
| `ShipOrder` | Implements mock shipping RPC. |
| `Quote` | Internal quote representation. |
| `CreateQuoteFromCount` | Creates a quote from item count. |
| `CreateQuoteFromFloat` | Converts float values into quote units and nanos. |
| `CreateTrackingId` | Builds a tracking ID from random components and salt. |

# Dependencies

Shipping implements [Storefront gRPC API](/apis/storefront-grpc-api.md) and is called by [Frontend Service](/services/frontend.md) for cart quotes and [Checkout Service](/services/checkoutservice.md) during order placement. It co-changes strongly with frontend, checkout, and product catalog because shipping totals appear in cart and order flows.

# Gotchas

* gRPC dependency updates have included security fixes in shipping; keep module updates and generated gRPC compatibility together (`b23a4b0b`, `1a098aac`).
* Multi-architecture Docker changes were reverted across all services, including shipping (`ed7b9419`).

# Citations

1. `b23a4b0b` - Update module google.golang.org/grpc to v1.83.1 [SECURITY] (#3505).
2. `1a098aac` - fix(deps): update module golang.org/x/net to v0.38.0 [security] (#2943).
3. `ed7b9419` - Revert "Add support for arm64 (#2589)".
