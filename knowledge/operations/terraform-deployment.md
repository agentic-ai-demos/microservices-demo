---
type: Configuration
title: Terraform Deployment
description: Provisions a GKE cluster and supporting Google Cloud resources for the storefront.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/terraform
tags: [terraform, gcp, gke, infrastructure]
timestamp: 2026-09-03T15:39:28-04:00
source_files:
  - terraform
generated_by: catalogify/0.8.0
---
# Responsibilities

Provisions a GKE cluster and supporting Google Cloud resources for the storefront.

# Interfaces

| File | Purpose |
| --- | --- |
| `main.tf` | Cluster and project resources. |
| `variables.tf` | Project, region and cluster inputs. |

# Dependencies

Provisions the cluster that [Kubernetes Manifests](kubernetes-manifests.md) or the
[Helm Chart](helm-chart.md) are then applied to. It does not deploy the services itself.
