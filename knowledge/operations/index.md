# Operations

* [Kubernetes Manifests](kubernetes-manifests.md) - The baseline manifests define Deployments, Services, service accounts, probes, resources, and security context for the demo application.
* [Kustomize Variants](kustomize-variants.md) - Kustomize overlays and components compose optional deployment variants around the baseline manifests.
* [Helm Chart](helm-chart.md) - The Helm chart parameterizes the application deployment for chart-based installation.
* [Terraform GKE Deployment](terraform-gke-deployment.md) - Terraform provisions Google Cloud APIs, an Autopilot GKE cluster, optional Memorystore, and applies the manifests.
* [Skaffold and Cloud Build](skaffold-cloudbuild.md) - Skaffold builds all service images and deploys manifests locally or through Cloud Build.
* [GitHub Actions Pipelines](github-actions.md) - GitHub Actions validate code, manifests, Terraform, Helm, releases, and pull-request deployments.
