---
type: Service
title: Recommendation Service
description: Python service returning related product ids, chosen at random from the catalog.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/recommendationservice
tags: [python, recommendations, grpc]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/recommendationservice
generated_by: catalogify/0.8.0
open_questions:
  - "If the catalog call fails, is an empty recommendation list acceptable, or should the page fail?"
  - "Is randomness expected to be stable per product, or fresh per request?"
---
# Responsibilities

Given a product and the ids already on the page, returns other product ids to show. The selection is random rather than modelled, so it exists to exercise the call path rather than to recommend well.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `ListRecommendations` | Product ids to show alongside the current one. |

# Dependencies

Calls [Product Catalog Service](productcatalogservice.md) to learn what products exist,
which makes it the only backend that is itself a client of another backend.
Called by [Frontend](frontend.md) over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

It changes with [Email Service](emailservice.md) in 93% of its commits at lift 5.4 —
a shared Python dependency set, not a shared code path.

# Gotchas

**Trace context is propagated unconditionally, by design.** `1c8abe20` removed the environment-variable gate so a service mesh or another process can inject context and have it survive the hop. The stated reason is that a span created upstream appears orphaned if this service drops the headers. The same commit deliberately leaves propagation out of leaf services that make no downstream calls.

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `1c8abe20` — Propagate trace context always (#1345).
2. `ed7b9419` — Revert "Add support for arm64 (#2589)".
3. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
