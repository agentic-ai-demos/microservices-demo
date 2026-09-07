---
type: Service
title: Product Catalog Service
description: The Go product catalog service exposes product listing, lookup, and search over the repository catalog data.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/productcatalogservice/server.go
tags: [service, go, grpc, catalog]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/productcatalogservice/server.go
  - src/productcatalogservice/product_catalog.go
  - src/productcatalogservice/catalog_loader.go
  - src/productcatalogservice/products.json
  - src/productcatalogservice/go.mod
  - src/productcatalogservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Is AlloyDB catalog loading intended to be feature-equivalent with local JSON loading for search and category behavior?"
---

# Responsibilities

Product catalog service implements product listing, lookup, and substring search. It loads product data into memory from [Product Catalog Data](/data/product-catalog.md) and serves it over the product RPCs in [Storefront gRPC API](/apis/storefront-grpc-api.md).

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `ListProducts` | Returns all loaded catalog products. |
| `GetProduct` | Returns one product by ID or a not-found error. |
| `SearchProducts` | Searches products by query. |
| `parseCatalog` | Reads the in-memory catalog payload. |
| `loadCatalog` | Chooses the active catalog source. |
| `loadCatalogFromLocalFile` | Parses `products.json`. |
| `loadCatalogFromAlloyDB` | Loads catalog rows from AlloyDB. |

# Dependencies

The service depends on protobuf definitions from [Storefront gRPC API](/apis/storefront-grpc-api.md) and shares product IDs with [Frontend Service](/services/frontend.md), [Checkout Service](/services/checkoutservice.md), and [Recommendation Service](/services/recommendationservice.md). Its optional AlloyDB path ties it to [Terraform GKE Deployment](/operations/terraform-gke-deployment.md) and Kustomize components for database-backed variants.

# Gotchas

* OpenTelemetry trace propagation was changed in product catalog alongside frontend, checkout, currency, and recommendation; preserve both tracer provider setup and gRPC handlers when changing startup code (`1c8abe20`).
* The AlloyDB loader brings `pgx` security maintenance into this otherwise static-catalog service (`1db7e998`).

# Key files

| Path | Role |
| --- | --- |
| `src/productcatalogservice/product_catalog.go` | Product RPC implementation. |
| `src/productcatalogservice/catalog_loader.go` | Data loading paths. |
| `src/productcatalogservice/products.json` | Default catalog data. |

# Citations

1. `1c8abe20` - Propagate trace context always (#1345).
2. `1db7e998` - fix(deps): update module github.com/jackc/pgx/v5 to v5.9.2 [security] (#3317).
