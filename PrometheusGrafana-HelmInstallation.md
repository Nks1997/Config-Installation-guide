# Minikube Kubernetes Observability Setup

## Step 1: Start Minikube
```bash
minikube start --cpus=4 --memory=8192 --driver=docker
```

## Step 2: Enable required addons
```bash
minikube addons enable ingress
```

## Enable metrics-server
```bash
minikube addons enable metrics-server
```

## Step 3: Verify Minikube status
```bash
kubectl get nodes
kubectl get pods -n kube-system
```

## Step 4: Install Helm
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

## Step 5: Add Helm repositories, Add Prometheus Community repo
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## Step 6: Install kube-prometheus-stack-Install Prometheus + Grafana
```bash
kubectl create ns monitoring
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring -f ./custom_kube_prometheus_stack.yml (used for alert manager)
```

## Step 7: Verify installation
``bash
kubectl get pods -n default
kubectl get pods -n monitoring

## Kubectl port forward not ingress
## Step 8: Access Grafana- Expose Grafana service
```bash
kubectl port-forward service/monitoring-grafana -n monitoring 3000:80

## Open browser: http://localhost:3000
## Default login: admin / prom-operator

## Step 9: Access Prometheus - Expose Prometheus service
```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090:9090 -n monitoring
or
kubectl port-forward service/prometheus-operated -n monitoring 9090:9090
```
Open browser: http://localhost:9090

- **Alertmanager UI**:
```bash
kubectl port-forward service/alertmanager-operated -n monitoring 9093:9093
```

## Step 10: Cleanup - To uninstall Prometheus stack
```bash
helm uninstall prometheus
Stop Minikube
minikube stop
