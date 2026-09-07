---
type: Service
title: Shopping Assistant Service
description: The Python shopping assistant accepts image input and calls a Gemini-backed recommendation path.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/shoppingassistantservice/shoppingassistantservice.py
tags: [service, python, flask, ai]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/shoppingassistantservice/shoppingassistantservice.py
  - src/shoppingassistantservice/requirements.txt
  - src/shoppingassistantservice/Dockerfile
generated_by: catalogify/0.7.0
open_questions:
  - "Which Gemini model and safety settings are expected for deployments, and are they intentionally configured outside this repository?"
---

# Responsibilities

Shopping assistant is an optional Flask service for image-based product suggestions. It is not part of the default Helm chart and is enabled through Kustomize components that add the service to the storefront experience.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `create_app` | Constructs the Flask app and POST route. |

# Dependencies

Shopping assistant is surfaced through [Frontend Service](frontend.md) assistant handlers and deployed by [Kustomize Variants](../operations/kustomize-variants.md). It depends on Python packages for image and Google AI interactions and co-changes with [Load Generator](loadgenerator.md), [Email Service](emailservice.md), and [Recommendation Service](recommendationservice.md) mostly through shared Python maintenance.

# Gotchas

* The service has security-tagged dependency updates for Flask and Pillow, so image upload and web framework dependencies need active maintenance (`26d1d1a0`, `035dcbfb`).
* Helm currently marks this service as unavailable, so enabling it is a Kustomize path unless the chart is extended.

# Citations

1. `26d1d1a0` - chore(deps): update dependency flask to v3.1.1 [security] (#2978).
2. `035dcbfb` - chore(deps): update dependency pillow to v10.3.0 [security] (#2470).
