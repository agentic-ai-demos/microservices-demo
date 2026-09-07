---
type: Service
title: Load Generator
description: The Locust load generator drives realistic browser-like traffic against the frontend.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/src/loadgenerator/locustfile.py
tags: [service, python, locust, load-test]
timestamp: 2026-08-24T16:44:54-04:00
source_files:
  - src/loadgenerator/locustfile.py
  - src/loadgenerator/requirements.txt
  - src/loadgenerator/Dockerfile
  - kubernetes-manifests/loadgenerator.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Is the Locust traffic mix intended to approximate a documented business workload, or only keep the demo visibly active?"
---

# Responsibilities

The load generator runs Locust tasks that browse the home page, switch currency, view products, manage the cart, checkout, and log out. It targets [Frontend Service](frontend.md) rather than calling backends directly.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `index` | Visits the storefront home page. |
| `setCurrency` | Exercises currency switching. |
| `browseProduct` | Opens product pages. |
| `viewCart` | Opens the cart page. |
| `addToCart` | Adds a selected product to the cart. |
| `empty_cart` | Clears the cart. |
| `checkout` | Posts the checkout form. |
| `UserBehavior` | Locust task set. |
| `WebsiteUser` | Locust user definition. |

# Dependencies

Load generator depends on [Frontend Service](frontend.md) HTTP routes and is deployed by [Kubernetes Manifests](../operations/kubernetes-manifests.md), [Skaffold and Cloud Build](../operations/skaffold-cloudbuild.md), and [Helm Chart](../operations/helm-chart.md). It co-changes with [Shopping Assistant Service](shoppingassistantservice.md), [Email Service](emailservice.md), and [Recommendation Service](recommendationservice.md) due to shared Python dependency and image updates.

# Gotchas

* The loadgenerator init container has its own hardening history; preserve init-container security context when changing startup checks (`9052f4c3`).
* Python image/dependency security updates often apply here with email and recommendation (`21b5bd60`, `61c019bc`).

# Citations

1. `9052f4c3` - loadgenerator - securityContext for initContainer (#1046).
2. `21b5bd60` - Python container images upgrade to latest minor version (security upgrade) (#2233).
3. `61c019bc` - chore(deps): update dependency certifi to v2022.12.7 [security] (#1371).
