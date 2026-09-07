---
type: Data Model
title: Product Catalog Data
description: The product catalog service ships a static JSON product set that must match the protobuf `Product` shape.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/productcatalogservice/products.json
tags: [catalog, json, products]
timestamp: 2026-09-03T15:38:37-04:00
source_files:
  - src/productcatalogservice/products.json
  - src/productcatalogservice/catalog_loader.go
  - src/productcatalogservice/go.mod
  - protos/demo.proto
generated_by: catalogify/0.7.0
open_questions:
  - "Is `products.json` intended to remain the default source of truth when AlloyDB catalog loading is enabled, or only a local/demo fallback?"
---

# Schema

| Field | Source | Notes |
| --- | --- | --- |
| `id` | `Product.id` | Stable product identifier used by cart, frontend routes, and recommendations. |
| `name` | `Product.name` | Display name. |
| `description` | `Product.description` | Display copy and search input. |
| `picture` | `Product.picture` | Image path consumed by the frontend. |
| `price_usd` | `Product.price_usd` | Base price stored as protobuf `Money`. |
| `categories` | `Product.categories` | Related-product and ad context hints. |

# Responsibilities

The product catalog data is read by [Product Catalog Service](../services/productcatalogservice.md), then surfaced to the frontend and recommendation flow through [Storefront gRPC API](../apis/storefront-grpc-api.md). `catalog_loader.go` can load from the local JSON file or from AlloyDB-related paths depending on environment configuration.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `loadCatalog` | Chooses the configured catalog loading source. |
| `loadCatalogFromLocalFile` | Parses `products.json` into protobuf product records. |
| `loadCatalogFromAlloyDB` | Loads catalog records from an AlloyDB connection. |

# Dependencies

This data concept is owned by [Product Catalog Service](../services/productcatalogservice.md) and shared indirectly with [Frontend Service](../services/frontend.md), [Checkout Service](../services/checkoutservice.md), and [Recommendation Service](../services/recommendationservice.md) through product IDs and `Product` messages.

# Gotchas

Product catalog service history includes a security update to the `pgx` dependency, so AlloyDB-backed catalog loading inherits database client maintenance obligations even though the default data file is static (`1db7e998`).

# Citations

1. `1db7e998` - fix(deps): update module github.com/jackc/pgx/v5 to v5.9.2 [security] (#3317).
