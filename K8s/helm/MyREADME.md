
# Overview

This section provides instructions for deploying the Robot Shop application using Helm. 
It includes commands to install or upgrade the Helm release, create the necessary namespace, 
and verify the deployment by listing all resources in the specified namespace.


# Deploy via Helm

To deploy the Robot Shop application using Helm, follow these steps:

## Install the Helm Release

```bash
helm install robot-shop --namespace robot-shop --create-namespace .
```

## Upgrade or Install the Helm Release

```bash
helm upgrade --install robot-shop --namespace robot-shop --create-namespace .
```

## Verify the Deployment

List all resources in the `robot-shop` namespace:

```bash
helm list --namespace robot-shop
kubectl get all -n robot-shop
```
## Additional Commands

### Get Pods and Services

To list all pods and services in the `robot-shop` namespace, use the following commands:

```bash
kubectl get pods -n robot-shop
kubectl get svc -n robot-shop
kubectl get all -n robot-shop
```

### Note the IP Address of the LoadBalancer Service

To retrieve the external IP address of the `web` LoadBalancer service, run:

```bash
kubectl get service web -n robot-shop | awk '{print $4}'
```

### Uninstall the Deployment

To uninstall the Helm release and optionally clean up the namespace, use:

```bash
helm uninstall robot-shop --namespace robot-shop
kubectl delete ns robot-shop  # Optional: Removes the namespace
```
