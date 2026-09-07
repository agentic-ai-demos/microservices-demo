---
type: Service
title: Load Generator
description: Python Locust client that drives synthetic shopper traffic against the frontend.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/loadgenerator
tags: [python, locust, load-testing, http]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/loadgenerator
generated_by: catalogify/0.8.0
open_questions:
  - "Is the traffic mix meant to be representative, or simply non-trivial?"
---
# Responsibilities

A Locust load client, not part of the storefront. It walks the site as a user would — browse, add to cart, check out — so the system has traffic without a human. It talks HTTP to the frontend only.

# Interfaces

| Behaviour | Purpose |
| --- | --- |
| Browse and view products | Exercises the catalog and ad read paths. |
| Add to cart and check out | Exercises the full order orchestration. |

# Dependencies

Talks HTTP to [Frontend](frontend.md) only, and never to a backend directly, so it exercises
exactly the paths a browser would.

# Gotchas

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

It is also easy to leave out of a deployment by accident; the build configuration has needed
a fix for exactly that, recorded on [CI and Release](../operations/ci-and-release.md).

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
