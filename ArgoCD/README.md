## GitOps-Driven Microservices Demo: Robot Shop Deployment with Argo CD, Helm, and Minikube

A comprehensive demonstration of GitOps practices using Argo CD to deploy and manage a microservices-based e-commerce application (Robot Shop) on a local Kubernetes environment. This project showcases:

- GitOps workflow with Argo CD for continuous deployment
- Helm charts for Kubernetes resource management
- Microservices architecture in a local development environment
- Development-to-production practices using Minikube

This guide provides a complete walkthrough of deploying and managing the Robot Shop microservices application using declarative GitOps patterns with Argo CD and Helm on Minikube.

## Prerequisites

- Minikube installed and running
- kubectl installed and configured to talk to your Minikube cluster
- Helm installed

Start Minikube (if not already running):

```bash
minikube start
```

Install Argo CD

1. Create the Argo CD namespace:

```bash
kubectl create namespace argocd
```

2. Install Argo CD into that namespace:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3. Wait until all Argo CD pods are in the Running state:

```bash
kubectl get pods -n argocd --watch
```

Access the Argo CD UI

Port-forward the Argo CD server to port 8888 (this repo uses port 8080 for Robot Shop's frontend):

```bash
kubectl port-forward svc/argocd-server -n argocd 8888:443
```

Open your browser to:

http://localhost:8888

Get the initial admin password:

```bash
# Decode the Argo CD initial admin password (POSIX/base64)
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath='{.data.password}' | base64 --decode
```

Login with:

- Username: admin
- Password: (the value shown by the command above)

Deploy Robot Shop App via Argo CD

1. From any location, clone this repository if you haven't already:

```bash
git clone https://github.com/DimitryZH/robot-shop-clone.git
cd robot-shop-clone/ArgoCD
```

2. Apply the Argo CD Application manifest included in this folder (`app.yaml`). This creates an Argo CD Application which points to the Helm chart in the repo:

```bash
kubectl apply -f app.yaml -n argocd
```

3. Confirm the Application object exists:

```bash
kubectl get applications -n argocd
```

Verify Deployment

Argo CD will:

- create the `robot-shop` namespace (the Application has CreateNamespace enabled)
- deploy the Helm chart located at `K8s/helm` in this repository
- keep the cluster in sync with the Git repository (auto-sync and self-heal are enabled)

Watch progress in the Argo CD UI or with kubectl by listing pods in the `robot-shop` namespace:

```bash
kubectl get pods -n robot-shop --watch
```

Access the Robot Shop Application

Port-forward the frontend service to access it locally:

```bash
kubectl port-forward svc/web -n robot-shop 8080:8080
```

Then open:

http://localhost:8080

If you prefer a LoadBalancer IP (Minikube tunneled service), you can use:

```bash
minikube tunnel
```

Argo CD Automation

The `app.yaml` in this folder enables the following sync behaviors:

- Auto-sync: Argo CD will automatically apply changes pushed to the Git repo
- Self-heal: Argo CD will revert manual changes in the cluster to match Git
- Prune: Resources removed from Git will be deleted from the cluster

## Summary

| Component          | Description              | Access                                         |
| ------------------ | ------------------------ | ---------------------------------------------- |
| Argo CD UI         | GitOps Dashboard         | http://localhost:8888                          |
| Robot Shop App     | Microservices storefront | http://localhost:8080                          |
