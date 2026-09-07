---
type: Reference
title: Online Boutique Architecture Overview
description: How eleven gRPC services and one HTTP assistant turn a browser request into a placed order.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/README.md
tags: [architecture, overview, microservices, grpc]
timestamp: 2026-09-03T17:10:06-04:00
source_files:
  - README.md
  - src
  - protos/demo.proto
generated_by: catalogify/0.8.0
---
# Start here

Online Boutique is a web storefront split into twelve deployable services across five
languages. Only one of them, the [Frontend](../services/frontend.md), is reachable from a
browser. Everything else speaks gRPC and is defined by a single file,
[`protos/demo.proto`](../apis/storefront-grpc-api.md), which is the contract for the whole
system.

The shape to internalise: **the frontend fans out per page, and the checkout service fans out
per order.** Every other backend is a leaf that calls nothing, with one exception — the
recommendation service, which calls the catalog.

# The services

| Service | Language | Role |
| --- | --- | --- |
| [Frontend](../services/frontend.md) | Go | The only browser-facing service |
| [Checkout Service](../services/checkoutservice.md) | Go | Orchestrates the order |
| [Product Catalog Service](../services/productcatalogservice.md) | Go | List, get, search |
| [Shipping Service](../services/shippingservice.md) | Go | Quotes and tracking ids |
| [Cart Service](../services/cartservice.md) | C# | Session carts, pluggable store |
| [Currency Service](../services/currencyservice.md) | Node | Conversion from a static table |
| [Payment Service](../services/paymentservice.md) | Node | Card validation, simulated |
| [Email Service](../services/emailservice.md) | Python | Order confirmation |
| [Recommendation Service](../services/recommendationservice.md) | Python | Related products |
| [Ad Service](../services/adservice.md) | Java | Contextual ads |
| [Load Generator](../services/loadgenerator.md) | Python | Synthetic traffic |
| [Shopping Assistant](../services/shoppingassistantservice.md) | Python | LLM chat, HTTP not gRPC |

# The two fan-outs

**Per page**, the frontend calls catalog, ads, cart, currency, recommendations and shipping.
Which ones depends on the route.

**Per order**, `PlaceOrder` on the checkout service runs a fixed sequence: read cart, price it
through catalog and currency, quote shipping, charge payment, ship, empty cart, send email. It
is sequential orchestration with no compensating action, so a failure after the charge leaves
the order in a state nothing repairs. See the open questions on
[Checkout Service](../services/checkoutservice.md).

# Co-change tells you about the build, not the runtime

Mined from 2,000 commits, the strongest couplings in this repository are not call paths at all:

| Pair | Confidence | Lift | Why |
| --- | ---: | ---: | --- |
| email ↔ recommendation | 89% / 93% | 5.4 | Both Python; one dependency bump touches both |
| currency ↔ payment | 88% | 7.1 | Both Node; same |
| loadgen ↔ assistant | 22% | 4.2 | Both Python |

Read that as a warning about interpretation. High co-change with high lift is a real signal,
but here the mechanism is a shared dependency ecosystem and an automated updater, not a shared
contract. The services that genuinely call each other — frontend to everything, checkout to six
backends — barely co-change at all, because a caller and its callee are usually edited in
different weeks.

# Cross-cutting invariants

**Platform changes are all-or-nothing.** arm64 support was added, reverted across 13 files, and
later reapplied. Every service Dockerfile and `skaffold.yaml` move together (`ed7b9419`,
`ccfb5908`).

**Trace context propagates unconditionally** in services that make downstream calls, and
deliberately not in the leaves that do not (`1c8abe20`).

**One proto, five languages.** A change to `protos/demo.proto` regenerates client and server
stubs in Go, C#, Node, Python and Java. It is the highest-blast-radius file in the repository
and it is 300 lines long.

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
3. `1c8abe20` — Propagate trace context always (#1345).
