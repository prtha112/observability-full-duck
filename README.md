# observability-full-duck

# Start minikube
```bash
minikube start
```

# Check pod
```bash
kubectl get pods -n kube-system
```

# Install helm
```bash
brew install helm
```

# Install opentelemetry-operator
```bash
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update
helm install opentelemetry-operator open-telemetry/opentelemetry-operator \
--namespace opentelemetry-operator-system \
--create-namespace \
--set "manager.collectorImage.repository=ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-k8s" \
--set admissionWebhooks.certManager.enabled=false \
--set admissionWebhooks.autoGenerateCert.enabled=true
```

# Install ingress-nginx
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx . -n api-gateway --create-namespace
```

# Install prometheus server
```bash
helm upgrade prometheus oci://ghcr.io/prometheus-community/charts/prometheus \
    --namespace prometheus \
    --set alertmanager.enabled=false \
    --set prometheus-pushgateway.enabled=false \
    --set prometheus-node-exporter.enabled=false \
    --set kube-state-metrics.enabled=false \
    --set "server.extraFlags[0]=web.enable-remote-write-receiver"
```
