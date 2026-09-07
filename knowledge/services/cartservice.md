---
type: Service
title: Cart Service
description: The C# cart service stores per-user cart items behind the CartService gRPC API.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/cartservice/src/services/CartService.cs
tags: [service, csharp, grpc, cart, storage]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/cartservice/src/services/CartService.cs
  - src/cartservice/src/cartstore/ICartStore.cs
  - src/cartservice/src/cartstore/RedisCartStore.cs
  - src/cartservice/src/cartstore/SpannerCartStore.cs
  - src/cartservice/src/cartstore/AlloyDBCartStore.cs
  - src/cartservice/src/Startup.cs
  - src/cartservice/src/Dockerfile
  - kubernetes-manifests/cartservice.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Which cart backends are considered supported production-like paths: Redis only, or Redis plus Spanner and AlloyDB?"
---

# Responsibilities

Cart service implements the cart portion of the shared gRPC contract and delegates persistence behind `ICartStore`. The repository includes Redis, Spanner, and AlloyDB cart store implementations, allowing deployment components to swap storage behavior while keeping the RPC interface stable.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `CartService` | gRPC service implementation. |
| `AddItem` | Adds a product and quantity to a user's cart. |
| `GetCart` | Returns the current cart for a user. |
| `EmptyCart` | Clears a user's cart. |
| `ICartStore` | Storage abstraction used by the gRPC service. |
| `RedisCartStore` | Redis-backed implementation. |
| `SpannerCartStore` | Spanner-backed implementation. |
| `AlloyDBCartStore` | AlloyDB-backed implementation. |
| `HealthCheckService` | gRPC health check implementation. |

# Dependencies

Cart depends on [Storefront gRPC API](../apis/storefront-grpc-api.md) and on a storage backend configured by deployment manifests. It is called by [Frontend Service](frontend.md) for cart views and mutations and by [Checkout Service](checkoutservice.md) during order placement.

# Gotchas

* Docker platform support was reverted across all services, including cart, so debug and release Dockerfiles need architecture changes together (`ed7b9419`).

# Key files

| Path | Role |
| --- | --- |
| `src/cartservice/src/services/CartService.cs` | RPC implementation. |
| `src/cartservice/src/cartstore/` | Persistence implementations. |
| `src/cartservice/src/Startup.cs` | Dependency injection and backend selection. |

# Citations

1. `ed7b9419` - Revert "Add support for arm64 (#2589)".
