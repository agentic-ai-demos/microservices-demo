---
type: Service
title: Currency Service
description: The Node.js currency service exposes supported currencies and currency conversion over gRPC.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/currencyservice/server.js
tags: [service, nodejs, grpc, currency]
timestamp: 2026-09-03T15:39:39-04:00
source_files:
  - src/currencyservice/server.js
  - src/currencyservice/package.json
  - src/currencyservice/package-lock.json
  - src/currencyservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "What freshness guarantee, if any, is expected for exchange-rate data fetched by `_getCurrencyData`?"
---

# Responsibilities

Currency service lists supported currencies and converts `Money` values into a target code. It is a Node.js gRPC server with health checks, OpenTelemetry gRPC instrumentation, optional profiling, and a conversion path used by frontend and checkout totals.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `_loadProto` | Loads protobuf definitions for gRPC service registration. |
| `_getCurrencyData` | Retrieves exchange-rate data. |
| `_carry` | Normalizes fractional money units. |
| `getSupportedCurrencies` | Implements supported currency listing. |
| `convert` | Implements currency conversion. |
| `check` | Implements gRPC health check. |
| `main` | Starts the gRPC server. |

# Dependencies

Currency implements [Storefront gRPC API](../apis/storefront-grpc-api.md) and is called by [Frontend Service](frontend.md) and [Checkout Service](checkoutservice.md). It historically moves with [Payment Service](paymentservice.md) because both are Node.js gRPC services with shared dependency and observability updates.

# Gotchas

* Currency and payment have repeated shared security updates to `@grpc/grpc-js` and OpenTelemetry packages; keep Node gRPC dependency updates coordinated (`01b3dbc0`, `1ea14802`).
* Trace propagation was explicitly added across this service and Go/Python peers; do not remove gRPC instrumentation while refactoring startup (`1c8abe20`).

# Citations

1. `01b3dbc0` - fix(deps): update dependency @grpc/grpc-js to v1.14.4 [security] (#3388).
2. `1ea14802` - fix(deps): update dependency @opentelemetry/sdk-node to v0.217.0 [security] (#3357).
3. `1c8abe20` - Propagate trace context always (#1345).
