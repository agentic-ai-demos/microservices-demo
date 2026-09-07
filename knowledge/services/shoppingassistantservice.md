---
type: Service
title: Shopping Assistant Service
description: Python Flask service answering shopping questions with an LLM, reached over HTTP rather than gRPC.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/shoppingassistantservice
tags: [python, flask, llm, http, optional]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/shoppingassistantservice
generated_by: catalogify/0.8.0
open_questions:
  - "Which external model and vector store is this expected to run against, and who holds those credentials?"
  - "Is the storefront required to degrade gracefully when the assistant is not deployed?"
---
# Responsibilities

The one service outside the gRPC contract. It exposes a single HTTP POST endpoint and answers a free-text shopping question using an LLM and a vector store. It is optional: the storefront works without it, and the deployment wires it in only when configured.

# Interfaces

| Endpoint | Purpose |
| --- | --- |
| `/` | POST — answer one shopping question. |

| Symbol | Purpose |
| --- | --- |
| `create_app` | Flask application factory. |
| `talkToGemini` | The single request handler. |

# Dependencies

Reached by [Frontend](frontend.md) over plain HTTP, not the
[storefront gRPC contract](../apis/storefront-grpc-api.md). It depends on external LLM and
vector-store services configured at deploy time, not on anything in this repository.

# Gotchas

**Platform support is a cross-cutting change, not a per-service one.** arm64 support was added, reverted wholesale (`ed7b9419`, 13 files: every service Dockerfile plus `skaffold.yaml`), and later reapplied (`ccfb5908`). A base-image or platform change that touches one Dockerfile almost certainly has to touch all of them and the build config together, or the release does not hold.

Its Dockerfile also carried legacy `ENV` syntax that had to be corrected (`0a09588e`) — a reminder that this service is newer than the rest and does not always follow their conventions.

# Citations

1. `ed7b9419` — Revert "Add support for arm64 (#2589)".
2. `ccfb5908` — Reapply "Add support for arm64 (#2589)".
3. `0a09588e` — fix legacy ENV syntax in Dockerfile (#3341).
