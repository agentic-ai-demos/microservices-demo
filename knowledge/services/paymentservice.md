---
type: Service
title: Payment Service
description: Node service that validates a card and returns a simulated transaction id. It charges nothing.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/paymentservice
tags: [nodejs, payment, leaf]
timestamp: 2026-09-03T15:39:39-04:00
source_files:
  - src/paymentservice
generated_by: catalogify/0.8.0
open_questions:
  - "Card validation rejects some inputs. Are the rejection reasons a contract the frontend renders, or internal?"
  - "Is `Charge` safe to retry, given checkout has no compensating action after it?"
---
# Responsibilities

Validates the card number, expiry and type, then returns a generated transaction id. No money moves and no external processor is contacted. It is the seam where a real payment provider would go.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `Charge` | Validate the card and return a transaction id. |

# Dependencies

A leaf, and deliberately the deepest one: it calls nothing and nothing calls it except
[Checkout Service](checkoutservice.md), over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

It changes with [Currency Service](currencyservice.md) in 88% of its commits at lift 7.1
because they share a Node dependency set, not because they share a code path.

# Gotchas

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

This service is one of the leaves that deliberately does **not** propagate trace context, because it makes no downstream calls, a decision recorded in the trace-context change described on
[Checkout Service](checkoutservice.md).

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
