---
type: Service
title: Cart Service
description: "C# service holding per-session carts behind a pluggable store: Redis, AlloyDB or Spanner."
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/cartservice
tags: [csharp, cart, redis, alloydb, spanner]
timestamp: 2026-08-24T16:45:04-04:00
source_files:
  - src/cartservice
generated_by: catalogify/0.8.0
open_questions:
  - "Are the three stores expected to be behaviourally identical, or is Redis allowed weaker durability?"
  - "Is cart expiry a store concern or a service concern? No TTL is visible at this layer."
---
# Responsibilities

Keeps the cart for a session id. The store is chosen at startup behind `ICartStore`, with Redis, AlloyDB and Spanner implementations, so the same service can run against an in-memory-ish cache or a managed database without the callers noticing.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `AddItem` | Append an item to a session cart. |
| `GetCart` | Read the whole cart. |
| `EmptyCart` | Clear it, called by checkout after a successful order. |

| Contract | Purpose |
| --- | --- |
| `ICartStore` | The seam every backend implements; pick one at startup. |

# Dependencies

Depends on no other service in this repository — it is a leaf. Its dependency is
its configured store.

Called by [Frontend](frontend.md) and [Checkout Service](checkoutservice.md) over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

# Gotchas

**The relational store had to be made idempotent, and had leaked credentials.** `dc88f8d8`
reworked `AlloyDBCartStore.cs` to hide database credentials and to handle duplicate inserts.
Adding an item is retried by callers, so a store backend that assumes a fresh row per call
is wrong. Treat "add the same item twice" as the normal case in any new backend.

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `dc88f8d8` — hide DB credentials and handle duplicate inserts when using AlloyDB (#3021).
2. `ed7b9419` — Revert "Add support for arm64 (#2589)".
3. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
