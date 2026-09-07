---
type: Service
title: Recommendation Service
description: The Python recommendation service returns product IDs related to the current cart context.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/recommendationservice/recommendation_server.py
tags: [service, python, grpc, recommendations]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/recommendationservice/recommendation_server.py
  - src/recommendationservice/client.py
  - src/recommendationservice/logger.py
  - src/recommendationservice/requirements.txt
  - src/recommendationservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Are recommendation results expected to be deterministic for a given cart, or is randomness acceptable for demo behavior?"
---

# Responsibilities

Recommendation service implements `ListRecommendations` and returns product IDs related to the cart context sent by the frontend. It also instruments gRPC server/client behavior for traces and exposes optional profiling.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `RecommendationService` | gRPC servicer implementation. |
| `initStackdriverProfiling` | Optional profiler setup. |
| `getJSONLogger` | JSON logger factory. |

# Dependencies

Recommendation implements [Storefront gRPC API](../apis/storefront-grpc-api.md). It is called by [Frontend Service](frontend.md) and returns IDs that the frontend resolves through [Product Catalog Service](productcatalogservice.md). Co-change shows very strong historical coupling with [Email Service](emailservice.md), mostly through shared Python dependency management.

# Gotchas

* Trace propagation was added to recommendation with frontend, checkout, currency, and product catalog; gRPC instrumentation is part of the cross-service trace contract (`1c8abe20`).
* Python dependency and base image security updates commonly span this service and email/loadgenerator (`21b5bd60`, `61c019bc`).

# Citations

1. `1c8abe20` - Propagate trace context always (#1345).
2. `21b5bd60` - Python container images upgrade to latest minor version (security upgrade) (#2233).
3. `61c019bc` - chore(deps): update dependency certifi to v2022.12.7 [security] (#1371).
