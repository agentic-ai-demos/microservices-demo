---
type: Service
title: Ad Service
description: Java service returning contextual ads for a set of category keywords.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/adservice
tags: [java, ads, grpc, jvm]
timestamp: 2026-09-03T15:39:57-04:00
source_files:
  - src/adservice
generated_by: catalogify/0.8.0
open_questions:
  - "Is the random-ad fallback a product decision the frontend relies on, or a placeholder?"
  - "Who owns the JVM base image and Gradle toolchain upgrades, given no other service shares them?"
---
# Responsibilities

Serves ads keyed by product category, falling back to random ads when no category matches. It is the only JVM service in the repository, which makes its build and base image different from every other.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `GetAds` | Ads for the given context keys, with a random fallback. |

# Dependencies

A leaf: it calls nothing. Called by [Frontend](frontend.md) over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

Being the only JVM service, it shares a dependency ecosystem with nothing else here, and its
co-change lift against the other services sits near 1 — it moves on its own schedule.

# Gotchas

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
