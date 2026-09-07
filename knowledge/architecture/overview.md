---
type: Reference
title: Online Boutique Architecture
description: Online Boutique is an 11-service Kubernetes demo storefront where HTTP traffic enters through the frontend and business flows continue over gRPC.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/README.md
tags: [architecture, microservices, kubernetes, grpc]
timestamp: 2026-09-03T17:10:06-04:00
source_files:
  - README.md
  - protos/demo.proto
  - skaffold.yaml
  - src/frontend/main.go
  - src/checkoutservice/main.go
  - src/currencyservice/server.js
generated_by: catalogify/0.7.0
open_questions:
  - "Which team or maintainer group owns production-like incident escalation for this demo repository?"
---

# Responsibilities

Online Boutique demonstrates a polyglot e-commerce application that runs on Kubernetes. The frontend exposes the user-facing HTTP site; all backend business capabilities are separate services connected by the shared gRPC contract in [Storefront gRPC API](../apis/storefront-grpc-api.md).

The checkout path is the main orchestration flow: [Frontend Service](../services/frontend.md) calls [Checkout Service](../services/checkoutservice.md), which reads [Cart Service](../services/cartservice.md), prices items from [Product Catalog Service](../services/productcatalogservice.md), converts money through [Currency Service](../services/currencyservice.md), charges [Payment Service](../services/paymentservice.md), requests [Shipping Service](../services/shippingservice.md), and asks [Email Service](../services/emailservice.md) to send confirmation.

# Interfaces

| Surface | Purpose |
| --- | --- |
| `skaffold.yaml` app config | Builds all service images and deploys `kubernetes-manifests` during local or Cloud Build workflows. |
| `demo.proto` | Defines the gRPC service and message contract for the application. |
| `kubernetes-manifests/*.yaml` | Baseline Kubernetes deployment shape for every service. |
| `helm-chart/` | Parameterized chart for deploying the same application shape. |
| `kustomize/` | Overlay and component system for operational variants such as service mesh, observability, Memorystore, Spanner, and the shopping assistant. |

# Dependencies

The system-level dependency is the protobuf contract: services either implement a service from `protos/demo.proto` or call generated clients derived from it. Operationally, [Kubernetes Manifests](../operations/kubernetes-manifests.md), [Kustomize Variants](../operations/kustomize-variants.md), [Helm Chart](../operations/helm-chart.md), and [Terraform GKE Deployment](../operations/terraform-gke-deployment.md) are alternative ways to instantiate the same service graph.

Co-change highlights two important clusters. `src/frontend`, `src/checkoutservice`, `src/productcatalogservice`, and `src/shippingservice` move together with lift between 6.82 and 8.10, matching their shared checkout and product browsing contract. `src/currencyservice` and `src/paymentservice` move together with lift 7.10 because both are Node.js gRPC services with shared dependency and observability churn.

# Gotchas

* Trace context is intentionally propagated across service boundaries; when changing gRPC initialization, keep server and client OpenTelemetry handlers aligned (`1c8abe20`).
* Multi-architecture image support was reverted across all service Dockerfiles and Skaffold, so image platform support should be tested as a cross-service build property, not one Dockerfile at a time (`ed7b9419`).

# Key files

| Path | Role |
| --- | --- |
| `README.md` | Human-facing architecture and quickstart. |
| `protos/demo.proto` | Shared service contract. |
| `skaffold.yaml` | Local and Cloud Build build/deploy graph. |

# Citations

1. `1c8abe20` - Propagate trace context always (#1345).
2. `ed7b9419` - Revert "Add support for arm64 (#2589)".
