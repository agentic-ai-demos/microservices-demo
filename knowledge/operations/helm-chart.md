---
type: Configuration
title: Helm Chart
description: The Helm chart parameterizes the application deployment for chart-based installation.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/helm-chart/values.yaml
tags: [helm, kubernetes, chart]
timestamp: 2026-09-03T15:39:13-04:00
source_files:
  - helm-chart/Chart.yaml
  - helm-chart/values.yaml
  - helm-chart/templates/adservice.yaml
  - helm-chart/templates/cartservice.yaml
  - helm-chart/templates/checkoutservice.yaml
  - helm-chart/templates/currencyservice.yaml
  - helm-chart/templates/emailservice.yaml
  - helm-chart/templates/frontend.yaml
  - helm-chart/templates/loadgenerator.yaml
  - helm-chart/templates/paymentservice.yaml
  - helm-chart/templates/productcatalogservice.yaml
  - helm-chart/templates/recommendationservice.yaml
  - helm-chart/templates/shippingservice.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Should the Helm chart reach feature parity with Kustomize components, especially for `shoppingAssistantService`, or intentionally stay narrower?"
---

# Responsibilities

The Helm chart packages Online Boutique for chart-based Kubernetes deployment. `values.yaml` controls image repository/tag, service creation, resources, service accounts, network policies, service mesh policies, OpenTelemetry collector creation, Google Cloud Operations flags, security context, seccomp, frontend exposure, and cart database settings.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `images.repository` | Default repository for service images. |
| `serviceAccounts.create` | Controls service account creation. |
| `networkPolicies.create` | Enables per-app NetworkPolicy resources. |
| `googleCloudOperations` | Toggles profiler, tracing, and metrics. |
| `securityContext.enable` | Applies pod/container hardening. |
| `cartDatabase.type` | Chooses cart backend behavior. |
| `frontend.externalService` | Controls public frontend Service exposure. |

# Dependencies

The chart mirrors [Kubernetes Manifests](kubernetes-manifests.md) and overlaps heavily with [Kustomize Variants](kustomize-variants.md). It deploys all default [Services](../services/) except shopping assistant, which is marked unavailable in the chart values.

# Gotchas

* Security context and seccomp values were fixed after nesting problems, so value schema changes must be checked against every service template (`342916ba`, `b6ab75e6`).
* Cloud Trace service naming changes crossed Helm and Kustomize; chart-only observability edits risk divergence (`02015ee8`).

# Citations

1. `342916ba` - fix: nest .Values.seccompProfile.enable and .Values.securityContext.enabled correctly (#2874).
2. `b6ab75e6` - Add optin for securitycontext in helm (#2758).
3. `02015ee8` - fix: display service name in Cloud Trace (#2350).
