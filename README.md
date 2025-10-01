# Kubenest Helm Charts

Official Helm charts for Kubenest operator.

## Usage

Add the Kubenest chart repository:

```bash
helm repo add kubenest https://kubenesthq.github.io/charts
helm repo update
```

Install the kubenest-workload chart:

```bash
helm install my-app kubenest/kubenest-workload \
  --set image.repository=nginx \
  --set image.tag=1.21 \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=my-app.example.com
```

## Charts

- **[kubenest-workload](charts/kubenest-workload)**: Default chart for Kubenest workloads with support for deployments, workers, services, ingress, and configuration management

## Development

### Linting

```bash
helm lint charts/kubenest-workload
```

### Testing

```bash
# Test chart rendering
helm template my-app charts/kubenest-workload --values test-values.yaml

# Install and test
helm install my-app charts/kubenest-workload
helm test my-app
```

### Packaging

```bash
helm package charts/kubenest-workload
```

## Contributing

Contributions are welcome! Please open an issue or pull request.

## License

Apache 2.0
