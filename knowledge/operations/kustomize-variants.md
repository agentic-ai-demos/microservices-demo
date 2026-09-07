---
type: Configuration
title: Kustomize Variants
description: Overlays that switch the cart store, add service mesh support, or enable optional components.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/kustomize
tags: [kustomize, overlays, configuration]
timestamp: 2026-09-03T17:10:06-04:00
source_files:
  - kustomize
generated_by: catalogify/0.8.0
---
# Responsibilities

Overlays that switch the cart store, add service mesh support, or enable optional components.

# Interfaces

| Overlay group | Purpose |
| --- | --- |
| `components/alloydb` | Swap the cart store to AlloyDB. |
| `components/spanner` | Swap it to Spanner. |
| `components/service-mesh-istio` | Run under Istio. |
| `components/shopping-assistant` | Enable the optional LLM assistant. |

# Dependencies

Layered over [Kubernetes Manifests](kubernetes-manifests.md). The store overlays correspond
to the `ICartStore` implementations described in [Cart Service](../services/cartservice.md).

# Key files

The overlays are the only place the optional pieces are switched on, so a service that works
in the base manifests may be absent entirely in a variant. Check which overlay a bug report
came from before reproducing. Read off the overlay tree, not from history.
