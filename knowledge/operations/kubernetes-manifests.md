---
type: Configuration
title: Kubernetes Manifests
description: Plain Deployment and Service manifests, one pair per service, wiring addresses through environment variables.
resource: https://github.com/agentic-ai-demos/microservices-demo/blob/main/kubernetes-manifests
tags: [kubernetes, deployment, manifests]
timestamp: 2026-09-03T15:37:52-04:00
source_files:
  - kubernetes-manifests
generated_by: catalogify/0.8.0
---
# Responsibilities

Plain Deployment and Service manifests, one pair per service, wiring addresses through environment variables.

# Interfaces

| Manifest | Purpose |
| --- | --- |
| `frontend.yaml` | The browser entry point and its backend addresses. |
| `checkoutservice.yaml` | The orchestrator and its six downstream addresses. |
| `cartservice.yaml` | Cart plus its store address. |
| `redis.yaml` | Default cart backing store. |

# Dependencies

The service graph is expressed here as `*_SERVICE_ADDR` environment variables, so this
directory is the deployed equivalent of the
[architecture overview](../architecture/overview.md). Change a service name and this is where
the rename has to land.

# Key files

Addresses are injected, not discovered. A service added to the code without a matching
`*_SERVICE_ADDR` entry here will build, deploy and fail at the first call. No commit records
this as a past incident; it is read off the manifests.
