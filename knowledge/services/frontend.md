---
type: Service
title: Frontend Service
description: The Go frontend serves the web UI and translates browser workflows into backend gRPC calls.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/frontend/main.go
tags: [service, go, http, grpc, storefront]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/frontend/main.go
  - src/frontend/handlers.go
  - src/frontend/rpc.go
  - src/frontend/middleware.go
  - src/frontend/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Is the 100 ms timeout in `getAd` a user-facing latency budget or a best-effort implementation detail?"
---

# Responsibilities

The frontend owns browser-facing HTTP routes, template rendering, session cookies, currency selection, and conversion from form submissions into backend gRPC requests. It is the only public application service in the default Kubernetes manifests; everything after the browser request is delegated to typed gRPC clients.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `homeHandler` | Renders the home page with products, cart count, currencies, recommendations, and ads. |
| `productHandler` | Renders the product details page for a catalog item. |
| `addToCartHandler` | Validates form input and calls cart insertion. |
| `viewCartHandler` | Renders cart contents and localized totals. |
| `placeOrderHandler` | Accepts checkout form data and calls checkout. |
| `assistantHandler` | Renders the shopping assistant view. |
| `chatBotHandler` | Proxies assistant prompts and product image data. |
| `setCurrencyHandler` | Persists selected currency in a cookie. |
| `getProducts` | Calls product catalog `ListProducts`. |
| `getCart` | Calls cart `GetCart`. |
| `getShippingQuote` | Calls shipping and then localizes the quote. |
| `getRecommendations` | Calls recommendation and resolves returned product IDs. |
| `getAd` | Calls ad service with a short timeout. |

# Dependencies

The frontend depends on [Storefront gRPC API](../apis/storefront-grpc-api.md) clients for product catalog, cart, currency, recommendation, shipping, ad, and checkout. It reads backend addresses from environment variables set by [Kubernetes Manifests](../operations/kubernetes-manifests.md), [Kustomize Variants](../operations/kustomize-variants.md), or [Helm Chart](../operations/helm-chart.md).

Co-change with [Checkout Service](checkoutservice.md), [Product Catalog Service](productcatalogservice.md), and [Shipping Service](shippingservice.md) has high lift because frontend pages expose the same browse/cart/checkout contracts those services implement.

# Gotchas

* Trace context propagation was made explicit across the frontend and backend services; keep client and server gRPC instrumentation together when changing request plumbing (`1c8abe20`).
* Listen-address refactoring was reverted with Kubernetes manifest changes, so endpoint binding changes need to be checked against service/deployment wiring (`b4862517`).

# Key files

| Path | Role |
| --- | --- |
| `src/frontend/handlers.go` | HTTP route handlers and template payload assembly. |
| `src/frontend/rpc.go` | gRPC client calls to backend services. |
| `src/frontend/main.go` | Server startup, environment mapping, tracing, profiling, and route registration. |

# Citations

1. `1c8abe20` - Propagate trace context always (#1345).
2. `b4862517` - Revert "frontend: use LISTEN_ADDR, refactor Listen code".
