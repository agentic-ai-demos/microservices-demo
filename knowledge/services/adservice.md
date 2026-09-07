---
type: Service
title: Ad Service
description: The Java ad service returns contextual text ads based on page context keys.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/adservice/src/main/java/hipstershop/AdService.java
tags: [service, java, grpc, ads]
timestamp: 2026-09-03T15:38:03-04:00
source_files:
  - src/adservice/src/main/java/hipstershop/AdService.java
  - src/adservice/src/main/java/hipstershop/AdServiceClient.java
  - src/adservice/build.gradle
  - src/adservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Are ad context keys intended to be a stable product-category contract with the frontend, or only sample data?"
---

# Responsibilities

Ad service implements contextual ad lookup for frontend pages. The service maps request context keys to predefined ads and falls back to generic ads when no context match is available.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `AdService` | Java gRPC server implementation and ad lookup logic. |
| `AdServiceClient` | Standalone client for exercising the ad RPC. |
| `main` | Starts either server or client entry points. |

# Dependencies

Ad implements [Storefront gRPC API](../apis/storefront-grpc-api.md) and is called by [Frontend Service](frontend.md), which derives context keys from product/category pages. Deployment and resource settings are maintained in [Kubernetes Manifests](../operations/kubernetes-manifests.md), [Kustomize Variants](../operations/kustomize-variants.md), and [Helm Chart](../operations/helm-chart.md).

# Gotchas

* Gradle and Java runtime churn is high in history; treat dependency updates as part of regular service maintenance rather than a rare event.
* Docker platform support was reverted across all service images, including the Java image (`ed7b9419`).

# Citations

1. `ed7b9419` - Revert "Add support for arm64 (#2589)".
