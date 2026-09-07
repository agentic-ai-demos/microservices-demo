---
type: Service
title: Email Service
description: The Python email service implements mock order confirmation delivery.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/emailservice/email_server.py
tags: [service, python, grpc, email]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/emailservice/email_server.py
  - src/emailservice/email_client.py
  - src/emailservice/logger.py
  - src/emailservice/requirements.txt
  - src/emailservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Should dummy mode be treated as the only supported mode for this demo, or is real email delivery expected in downstream forks?"
---

# Responsibilities

Email service implements order confirmation delivery for checkout. In this demo repository it is mock-oriented: `EmailService` and `DummyEmailService` share a base gRPC servicer, and the client helper formats a confirmation request for the service.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `BaseEmailService` | Base gRPC servicer shape. |
| `EmailService` | Confirmation email implementation. |
| `DummyEmailService` | No-op/mock email implementation. |
| `HealthCheck` | gRPC health response implementation. |
| `start` | Starts the gRPC server. |
| `initStackdriverProfiling` | Optional profiler setup. |
| `send_confirmation_email` | Client helper for manual/test calls. |
| `getJSONLogger` | JSON logger factory. |

# Dependencies

Email implements [Storefront gRPC API](/apis/storefront-grpc-api.md) and is called by [Checkout Service](/services/checkoutservice.md). It shares Python dependency maintenance patterns with [Recommendation Service](/services/recommendationservice.md) and [Load Generator](/services/loadgenerator.md).

# Gotchas

* Python base image and certificate dependency updates have been security-driven across email, recommendation, and load generation, so update these images and requirements consistently (`21b5bd60`, `61c019bc`).
* Docker platform support was reverted across all service images (`ed7b9419`).

# Citations

1. `21b5bd60` - Python container images upgrade to latest minor version (security upgrade) (#2233).
2. `61c019bc` - chore(deps): update dependency certifi to v2022.12.7 [security] (#1371).
3. `ed7b9419` - Revert "Add support for arm64 (#2589)".
