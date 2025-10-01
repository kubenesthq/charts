# Kubenest Workload Helm Chart

Default Helm chart for Kubenest workloads with comprehensive support for deployments, workers, services, ingress, and configuration management.

## Features

- **Deployment**: Configurable replicas, rolling updates, health checks, resource limits
- **Worker Deployment**: Optional background workers using the same image
- **Service**: ClusterIP, NodePort, or LoadBalancer with additional ports support
- **Ingress**: Multiple hosts and paths with TLS support
- **ConfigMap**: Always created for application configuration
- **Secret**: Optional secret management with volume mounting
- **Persistent Volume**: Optional PVC for stateful applications
- **Autoscaling**: HPA based on CPU/memory metrics
- **Pod Disruption Budget**: High availability configuration
- **Service Account**: With annotations for cloud IAM integration

## Installation

```bash
helm repo add kubenest https://charts.kubenest.io
helm repo update

helm install my-app kubenest/kubenest-workload \
  --set image.repository=myapp \
  --set image.tag=1.0.0
```

## Configuration

See [values.yaml](values.yaml) for all configuration options.

### Common Examples

#### Simple Web Application

```yaml
image:
  repository: myapp
  tag: "1.0.0"

container:
  port: 3000

ingress:
  enabled: true
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix
```

#### Application with Worker

```yaml
image:
  repository: myapp
  tag: "1.0.0"

deployment:
  replicaCount: 3

worker:
  enabled: true
  replicaCount: 2
  container:
    command: ["npm", "run", "worker"]
```

#### Stateful Application

```yaml
image:
  repository: myapp
  tag: "1.0.0"

persistence:
  enabled: true
  size: 20Gi
  mountPath: /data

secret:
  enabled: true
  data:
    DB_PASSWORD: cGFzc3dvcmQxMjM=
```

## Testing

```bash
# Lint the chart
helm lint .

# Test rendering
helm template my-app . --values test-values.yaml

# Install and test
helm install my-app .
helm test my-app
```

## License

Apache 2.0
