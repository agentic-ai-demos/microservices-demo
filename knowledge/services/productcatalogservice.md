---
type: Service
title: Product Catalog Service
description: Go service serving the product list, single products and search from a JSON catalog or a database.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/productcatalogservice
tags: [go, catalog, search, read-path]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/productcatalogservice
generated_by: catalogify/0.8.0
open_questions:
  - "Is the catalog reloaded on change, or only at startup? Callers may be caching against a stale generation."
  - "Is `SearchProducts` expected to stay a linear scan, or is a real index intended?"
---
# Responsibilities

Reads the catalog and answers list, get and search. It is the read-hottest service in the system, sitting behind both the frontend home page and every checkout pricing step.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `ListProducts` | The whole catalog, used by the home page. |
| `GetProduct` | One product by id. |
| `SearchProducts` | Substring search over name and description. |

# Dependencies

A leaf: it calls nothing. Read by [Frontend](frontend.md),
[Checkout Service](checkoutservice.md) and
[Recommendation Service](recommendationservice.md) over the
[storefront gRPC contract](../apis/storefront-grpc-api.md). The catalog itself is described in
[Product Catalog data](../data/product-catalog.md).

# Gotchas

**The read path has been quietly quadratic before.** `48edfe97` found `GetProduct` calling
`parseCatalog()` three times per loop iteration with no early return on match. Parsing is not
free and this service is on every page load, so re-reading the catalog inside a loop is the
mistake to watch for here.

**Trace context is propagated unconditionally, by design.** `1c8abe20` removed the environment-variable gate so a service mesh or another process can inject context and have it survive the hop. The stated reason is that a span created upstream appears orphaned if this service drops the headers. The same commit deliberately leaves propagation out of leaf services that make no downstream calls.

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `48edfe97` — avoid redundant parseCatalog calls in GetProduct (#3280).
2. `1c8abe20` — Propagate trace context always (#1345).
3. `ed7b9419` — Revert "Add support for arm64 (#2589)".
4. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
