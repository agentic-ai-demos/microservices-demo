---
type: API Resource
title: Storefront gRPC API
description: The protobuf contract defines the RPC services and messages shared by all Online Boutique backends.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/protos/demo.proto
tags: [grpc, protobuf, api]
timestamp: 2024-09-05T13:22:39-04:00
source_files:
  - protos/demo.proto
generated_by: catalogify/0.7.0
open_questions:
  - "Are changes to `protos/demo.proto` expected to remain wire-compatible for externally deployed demo variants, or is the repo allowed to make breaking contract changes?"
---

# Responsibilities

`protos/demo.proto` is the canonical API contract for the storefront domain. Generated code is checked into each service language tree, but this file is the source contract for cart, recommendation, product catalog, shipping, currency, payment, email, checkout, and ads.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `CartService.AddItem` | Adds an item and quantity to a user's cart. |
| `CartService.GetCart` | Reads the current cart for a user. |
| `CartService.EmptyCart` | Clears all items for a user. |
| `RecommendationService.ListRecommendations` | Returns related product IDs for a user and cart context. |
| `ProductCatalogService.ListProducts` | Lists all products in the catalog. |
| `ProductCatalogService.GetProduct` | Fetches a single product by ID. |
| `ProductCatalogService.SearchProducts` | Searches catalog items by query. |
| `ShippingService.GetQuote` | Estimates shipping cost for an address and cart items. |
| `ShippingService.ShipOrder` | Produces a mock shipping tracking ID. |
| `CurrencyService.GetSupportedCurrencies` | Lists supported ISO currency codes. |
| `CurrencyService.Convert` | Converts `Money` to a target currency. |
| `PaymentService.Charge` | Validates and mock-charges a credit card. |
| `EmailService.SendOrderConfirmation` | Sends a mock order confirmation. |
| `CheckoutService.PlaceOrder` | Orchestrates cart-to-order checkout. |
| `AdService.GetAds` | Returns contextual ads for page context keys. |

# Dependencies

Every service concept links back here because its server implementation or generated client code is derived from this contract. The contract also defines shared data shapes documented in [Product Catalog Data](/data/product-catalog.md) where `Product` records are serialized into `products.json`.

# Key files

| Path | Role |
| --- | --- |
| `protos/demo.proto` | Canonical storefront service and message definitions. |
