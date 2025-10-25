# Enterprise-Grade Kubernetes Observability: Grafana Cloud Integration with Helm

A comprehensive implementation guide for deploying the **Grafana Cloud Kubernetes Monitoring Stack v2** using GitOps practices and Helm charts. This solution delivers enterprise-level observability for Kubernetes environments through:

- Complete cluster telemetry pipeline (metrics, logs, traces)
- Automated cost optimization with OpenCost integration
- Energy efficiency monitoring via Kepler
- Seamless Grafana Cloud integration
- Infrastructure-as-Code deployment using Helm

This guide demonstrates how to implement production-ready Kubernetes monitoring using Helm and Grafana Cloud, enabling comprehensive observability from development through production environments.

---

## Overview

Grafana Cloud Kubernetes Monitoring (v2) simplifies observability by providing:

- **Unified monitoring** for your Kubernetes clusters (metrics, logs, traces, profiles)  
- **Simplified Helm configuration** — no need to manage multiple agents or exporters manually  
- **Automatic integration** with Grafana Cloud (Prometheus, Loki, Tempo, Mimir)  
- **Cost monitoring** via **OpenCost** and energy tracking with **Kepler**  
- **Dynamic configuration** using **Grafana Alloy** agents  

---

##  Prerequisites

Before deployment, ensure you have:

1. **Helm** installed (v3 or higher):
```bash
   helm version
```  
2. **kubectl** connected to your cluster:
```bash
kubectl get nodes
```

3. A **Grafana Cloud account** with Kubernetes Monitoring integration created.
From your Grafana Cloud portal, go to:

**Connections → Kubernetes → Add cluster**

Copy the following values:

- Grafana Cloud Metrics URL

- Grafana Cloud Logs URL

- Grafana Cloud OTLP URL

- Organization ID (username)

- API Key (password)

## Deployment Steps

1. Clone this repository and navigate to the grafana/ folder:

```bash
cd grafana
```

2. Add the Grafana Helm repository and update:
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

3. Deploy the Kubernetes Monitoring stack using your configuration file:
```bash
helm upgrade --install --atomic --timeout 300s grafana-k8s-monitoring grafana/k8s-monitoring \
  --namespace grafana --create-namespace \
  --values values.yaml
```

4. Wait until all pods are running:
```bash
kubectl get pods -n grafana
```

Example output:
```bash
grafana-k8s-monitoring-alloy-logs-xxxxx
grafana-k8s-monitoring-alloy-metrics-0
grafana-k8s-monitoring-kepler-xxxxx
grafana-k8s-monitoring-opencost-xxxxx
grafana-k8s-monitoring-operator-xxxxx
...
```
## Verify in Grafana Cloud

After a few minutes, open your **Grafana Cloud** dashboard and verify:

- **Kubernetes Monitoring → Overview**
→ Cluster: minikube (or your custom cluster name)

- **Metrics** → CPU, memory, and node data should appear

- **Logs** → container and pod logs visible

- **Traces** → tracing data available if your apps export OTLP

- **Cost Overview** → cluster and pod cost data displayed

##  Uninstalling the Stack

To remove the monitoring stack from your cluster:

```bash
helm uninstall grafana-k8s-monitoring -n grafana
kubectl delete namespace grafana
```

## File Structure
grafana/
├── README.md        # Deployment instructions
└── values.yaml      # Helm configuration with Grafana Cloud endpoints and credentials

## Tips

You can add multiple destinations for telemetry data
(for example, send logs to both Grafana Cloud and a local Loki instance).

When deploying to multiple clusters, use unique cluster.name values
(e.g., dev-cluster, prod-cluster).

To enable tracing, ensure your applications export data to the OTLP receiver (:4317 or :4318).

