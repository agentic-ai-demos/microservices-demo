---
type: Configuration
title: Kubernetes Manifests
description: The baseline manifests define Deployments, Services, service accounts, probes, resources, and security context for the demo application.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/kubernetes-manifests/kustomization.yaml
tags: [kubernetes, manifests, deployment]
timestamp: 2026-09-03T15:37:52-04:00
source_files:
  - kubernetes-manifests/kustomization.yaml
  - kubernetes-manifests/frontend.yaml
  - kubernetes-manifests/cartservice.yaml
  - kubernetes-manifests/checkoutservice.yaml
  - kubernetes-manifests/currencyservice.yaml
  - kubernetes-manifests/emailservice.yaml
  - kubernetes-manifests/loadgenerator.yaml
  - kubernetes-manifests/paymentservice.yaml
  - kubernetes-manifests/productcatalogservice.yaml
  - kubernetes-manifests/recommendationservice.yaml
  - kubernetes-manifests/shippingservice.yaml
generated_by: catalogify/0.7.0
open_questions:
  - "Are Kubernetes resource requests/limits tuned to a target cluster size, or are they only demo-safe defaults?"
---

# Responsibilities

The baseline manifests are the direct `kubectl apply` deployment for Online Boutique. They define the service graph, container images, environment variables, probes, service accounts, Redis cart storage, frontend external exposure, and load generator startup checks.

# Interfaces

| Symbol | Purpose |
| --- | --- |
| `kustomization.yaml` | Lists baseline resources and image substitutions. |
| `frontend.yaml` | Public frontend Deployment and Service. |
| `cartservice.yaml` | Cart service and cart storage wiring. |
| `loadgenerator.yaml` | Locust Deployment and frontend readiness check. |
| `redis.yaml` | In-cluster Redis cart backend. |

# Dependencies

These manifests instantiate every concept in [Services](/services/) and consume image names produced by [Skaffold and Cloud Build](/operations/skaffold-cloudbuild.md). [Kustomize Variants](/operations/kustomize-variants.md), [Helm Chart](/operations/helm-chart.md), and [Terraform GKE Deployment](/operations/terraform-gke-deployment.md) either reuse or mirror this baseline shape.

# Gotchas

* Security context hardening is an explicit deployment invariant; new containers and init containers should follow it rather than silently running with weaker defaults (`b3debab5`, `9052f4c3`).
* Termination grace was added to services in early Kubernetes history, so probe and shutdown changes should account for gRPC server draining behavior (`77fac954`, `b4685dca`).

# Citations

1. `b3debab5` - Harden containers with securityContext (#887).
2. `9052f4c3` - loadgenerator - securityContext for initContainer (#1046).
3. `77fac954` - k8s: add terminationGracePeriodSeconds to some.
4. `b4685dca` - k8s: add termination grace to recommendationservice.
