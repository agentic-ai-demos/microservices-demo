---
type: API Resource
title: Storefront gRPC API
description: The single protobuf contract every backend implements and every caller generates stubs from.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/protos/demo.proto
tags: [grpc, protobuf, contract, api]
timestamp: 2024-09-05T13:22:39-04:00
source_files:
  - protos/demo.proto
generated_by: catalogify/0.8.0
open_questions:
  - "Is `protos/demo.proto` the single source of truth, or are the per-service copies under `src/*/proto/` allowed to drift?"
  - "Is there a compatibility policy for this file, given stubs are generated into five languages?"
---
# Responsibilities

One file defines nine gRPC services and every message they exchange. Each backend implements
its own service; each caller generates a client. There is no REST layer between them — the
[Frontend](../services/frontend.md) is the only HTTP surface in the system, and it translates.

# Interfaces

| Service | RPCs |
| --- | --- |
| CartService | `AddItem`, `GetCart`, `EmptyCart` |
| ProductCatalogService | `ListProducts`, `GetProduct`, `SearchProducts` |
| ShippingService | `GetQuote`, `ShipOrder` |
| CurrencyService | `GetSupportedCurrencies`, `Convert` |
| PaymentService | `Charge` |
| EmailService | `SendOrderConfirmation` |
| CheckoutService | `PlaceOrder` |
| RecommendationService | `ListRecommendations` |
| AdService | `GetAds` |

Health checking uses the standard `grpc.health.v1` protocol, vendored alongside.

# Dependencies

Implemented by every service except
[Load Generator](../services/loadgenerator.md), which drives HTTP, and
[Shopping Assistant](../services/shoppingassistantservice.md), which exposes HTTP.

# Why it is shaped this way

**This is the highest-blast-radius file in the repository.** A change here regenerates stubs in
Go, C#, Node, Python and Java simultaneously. Several services also keep their own copy under
`src/*/proto/`, so a change made in one place and not the others produces a mismatch that
compiles cleanly on both sides and fails at the wire.

Money is carried as `units` plus `nanos` rather than a float, which is correct, and means every
consumer has to do its own rounding. Nothing in the contract specifies how.

The blast radius is not theoretical: cross-cutting changes in this repository routinely land in
five services at once. See the trace-context and arm64 gotchas on
[Frontend](../services/frontend.md) and [Checkout Service](../services/checkoutservice.md) for
what that looks like in practice.
