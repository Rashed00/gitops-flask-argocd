# GitOps Deployment Pipeline — Flask + ArgoCD + Kubernetes

![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=flat&logo=argo&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat&logo=helm&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat&logo=flask&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white)

A full GitOps workflow that continuously deploys a Flask microservice to a self-hosted Kubernetes cluster using ArgoCD. Any push to the `main` branch automatically triggers a sync, rolling update, and Slack notification.

---

## Architecture

```
Developer Push
     │
     ▼
GitHub (app repo)
     │
     ├── GitHub Actions CI
     │       ├── Lint & test
     │       ├── Docker build & push → Docker Hub
     │       └── Update image tag in Helm values
     │
     ▼
GitHub (config repo / this repo)
     │
     ▼
ArgoCD (watches this repo)
     │
     ├── Detects drift → syncs to cluster
     ├── Helm chart rollout (RollingUpdate strategy)
     └── Slack webhook alert on success/failure
```

---

## Stack

| Tool | Purpose |
|------|---------|
| Flask | Python microservice (REST API) |
| Docker | Containerization |
| Kubernetes | Container orchestration (self-hosted) |
| Helm | Kubernetes package manager & templating |
| ArgoCD | GitOps continuous delivery |
| GitHub Actions | CI pipeline (lint, test, build, push) |
| Slack Webhooks | Deploy notifications |

---

## Repository Structure

```
gitops-flask-argocd/
├── app/                        # Flask application source
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── helm/                       # Helm chart for the Flask app
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── deployment.yaml
│       ├── service.yaml
│       └── ingress.yaml
├── argocd/                     # ArgoCD Application manifests
│   └── application.yaml
├── .github/
│   └── workflows/
│       └── ci.yaml             # GitHub Actions CI pipeline
└── README.md
```

---

## Features

- **Automated sync** — ArgoCD polls the repo every 3 minutes and auto-syncs on any drift
- **Helm rollbacks** — one-command rollback to any previous release via `helm rollback`
- **Slack alerts** — deployment success/failure notifications with commit SHA and author
- **Health checks** — ArgoCD monitors pod readiness before marking sync as healthy
- **Self-healing** — any manual change to the cluster is automatically reverted by ArgoCD

---

## Setup & Usage

> 🚧 **Implementation in progress** — setup instructions will be added as the project is built out.

Prerequisites:
- A running Kubernetes cluster (kind / minikube / cloud)
- `kubectl`, `helm`, `argocd` CLI installed
- Docker Hub account (or any container registry)

```bash
# 1. Clone the repo
git clone https://github.com/Rashed00/gitops-flask-argocd.git

# 2. Install ArgoCD on your cluster
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 3. Apply the ArgoCD Application manifest
kubectl apply -f argocd/application.yaml

# 4. Watch ArgoCD sync the app automatically
argocd app get flask-app
```

---

## Roadmap

- [x] Project structure & README
- [ ] Flask app with health endpoint
- [ ] Dockerfile & Docker Hub CI push
- [ ] Helm chart (Deployment, Service, Ingress)
- [ ] ArgoCD Application manifest
- [ ] GitHub Actions CI workflow
- [ ] Slack webhook integration
- [ ] Rollback demo

---

## Author

**Rashed Wahdan** — [LinkedIn](https://www.linkedin.com/in/rashed-wahdan-a4b124145/) · [GitHub](https://github.com/Rashed00)
