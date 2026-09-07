---
type: Service
title: Currency Service
description: Node service converting money between currencies from a static ECB rate table.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/currencyservice
tags: [nodejs, currency, conversion]
timestamp: 2026-09-03T15:39:39-04:00
source_files:
  - src/currencyservice
generated_by: catalogify/0.8.0
open_questions:
  - "Money is passed as units plus nanos. Is rounding specified anywhere, or left to each caller?"
  - "Are the checked-in rates expected to be refreshed, and by whom?"
---
# Responsibilities

Converts a Money value between currencies and lists what is supported. Rates come from a checked-in table, so conversions are deterministic and offline.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `GetSupportedCurrencies` | The currency codes the table covers. |
| `Convert` | Convert one Money value into another currency. |

# Dependencies

A leaf: it calls nothing. Called by [Frontend](frontend.md) to display prices and by
[Checkout Service](checkoutservice.md) to price an order, over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

Co-change is worth reading carefully here. This service moves with
[Payment Service](paymentservice.md) in 88% of its commits at lift 7.1, and the two share no
call path at all. They share a Node dependency set, and the automated dependency updates touch
both in one commit. The coupling is real but it is about the build, not the runtime.

# Gotchas

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

**Trace context is propagated unconditionally, by design.** `1c8abe20` removed the environment-variable gate so a service mesh or another process can inject context and have it survive the hop. The stated reason is that a span created upstream appears orphaned if this service drops the headers. The same commit deliberately leaves propagation out of leaf services that make no downstream calls.

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
3. `1c8abe20` — Propagate trace context always (#1345).
