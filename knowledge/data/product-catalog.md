---
type: Data Model
title: Product Catalog
description: The product records the storefront sells, served from a JSON file or a relational database.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/productcatalogservice/products.json
tags: [data, catalog, json, alloydb]
timestamp: 2021-09-23T16:56:59-04:00
source_files:
  - src/productcatalogservice/products.json
  - src/productcatalogservice/product_catalog.go
generated_by: catalogify/0.8.0
open_questions:
  - "When the database backend is used, is products.json still authoritative, or does the database win?"
  - "Is the product id format a contract other services may parse, or opaque?"
---
# Schema

| Field | Meaning |
| --- | --- |
| `id` | Product identifier, used by cart, recommendations and checkout. |
| `name`, `description` | Display text; both are searched by `SearchProducts`. |
| `picture` | Static asset path served by the frontend. |
| `priceUsd` | Money as currency code, `units` and `nanos`. |
| `categories` | Drives ad selection and recommendations. |

# Dependencies

Read by [Product Catalog Service](../services/productcatalogservice.md), which is the only
service that touches the store directly. Everything else reaches it over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

# Gotchas

Prices are `units` plus `nanos`, never floats. Any new consumer that converts to a float for
arithmetic will disagree with the checkout total eventually.

The catalog is small and read on every page. `48edfe97` fixed `GetProduct` re-parsing it three
times per loop iteration, which is the shape of mistake this data model invites.

# Citations

1. `48edfe97` — avoid redundant parseCatalog calls in GetProduct (#3280).
