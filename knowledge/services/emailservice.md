---
type: Service
title: Email Service
description: Python service that renders an order confirmation. In the default deployment it only logs it.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/emailservice
tags: [python, email, template, leaf]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/emailservice
generated_by: catalogify/0.8.0
open_questions:
  - "Is confirmation delivery required to succeed for an order to be considered placed? Checkout calls it last and does not appear to compensate."
  - "Is the template a supported customisation point, or internal?"
---
# Responsibilities

Renders a Jinja template into a confirmation email for a placed order. The default deployment logs the result rather than sending it, so the send path is a stub standing in for a mail provider.

# Interfaces

| RPC | Purpose |
| --- | --- |
| `SendOrderConfirmation` | Render and dispatch the confirmation for one order. |

# Dependencies

A leaf: it calls nothing. Called only by [Checkout Service](checkoutservice.md) over the
[storefront gRPC contract](../apis/storefront-grpc-api.md).

It changes with [Recommendation Service](recommendationservice.md) in 89% of its commits at
lift 5.4. They share no call path; they share a Python dependency set, and the dependency
updater moves both together.

# Gotchas

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

Like the payment service, this is a leaf that deliberately does not propagate trace context, because it makes no downstream calls, a decision recorded in the trace-context change described on
[Checkout Service](checkoutservice.md).

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
