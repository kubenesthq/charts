# Kubenest Workload Helm Chart

Default Helm chart for Kubenest workloads with comprehensive support for deployments, workers, services, Gateway API HTTPRoutes, and configuration management.

## Features

- **Deployment**: Configurable replicas, rolling updates, optional health checks, resource limits
- **Worker Deployment**: Optional background workers using the same image
- **Service**: ClusterIP, NodePort, or LoadBalancer with additional ports support
- **HTTPRoute**: Gateway API exposure — the route on KubeNest platform clusters, whose core ingress is Traefik with Gateway API (TLS terminates at the Gateway)
- **Ingress** (legacy): Multiple hosts and paths with TLS support, for clusters that run their own Ingress controller
- **ConfigMap**: Always created for application configuration
- **Secret**: Optional secret management with volume mounting
- **Persistent Volume**: Optional PVC for stateful applications
- **Autoscaling**: HPA based on CPU/memory metrics
- **Pod Disruption Budget**: High availability configuration
- **Service Account**: With annotations for cloud IAM integration
- **Health Probes**: Optional liveness, readiness, and startup probes (disabled by default)

## Installation

```bash
helm repo add kubenest https://kubenesthq.github.io/charts
helm repo update

helm install my-app kubenest/kubenest-workload \
  --set image.repository=myapp \
  --set image.tag=1.0.0
```

## Configuration

See [values.yaml](values.yaml) for all configuration options.

### Common Examples

#### Simple Web Application (Gateway API)

```yaml
image:
  repository: myapp
  tag: "1.0.0"

container:
  port: 3000

httpRoute:
  enabled: true
  parentRefs:
    - name: kubenest-gateway
      namespace: kubenest-system
  hostnames:
    - myapp.example.com
```

#### Simple Web Application (legacy Ingress)

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

#### Application with Health Checks

```yaml
image:
  repository: myapp
  tag: "1.0.0"

container:
  port: 8080

  livenessProbe:
    enabled: true
    httpGet:
      path: /health
      port: http
    initialDelaySeconds: 30
    periodSeconds: 10

  readinessProbe:
    enabled: true
    httpGet:
      path: /ready
      port: http
    initialDelaySeconds: 5
    periodSeconds: 5
```

> **Note**: Health probes are disabled by default. Enable them explicitly if your application provides health endpoints.

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
