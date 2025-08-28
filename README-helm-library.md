# Deployment Library Chart

A generic Helm library chart for creating Kubernetes deployments with configmaps support.

## Integration

### 1. Add as Dependency

Add to your chart's `Chart.yaml`:

```yaml
dependencies:
  - name: deployment
    version: 1.0.3
    repository: "file://path/to/deployment-chart"  # or your chart repository
```

### 2. Use in Templates

Create `templates/deployment.yaml`:

```yaml
{{- include "deployment.deployment" . }}
```

Create `templates/configmap.yaml` (if using configmaps):

```yaml
{{- include "deployment.configmaps" . }}
```

### 3. Configure Values

In your `values.yaml`:

```yaml
deployment:
  image:
    repository: "your-app"
    tag: "v1.0.0"
    pullPolicy: IfNotPresent
  
  replicaCount: 3
  
  env:
    - name: "APP_ENV"
      value: "production"
  
  livenessProbe:
    enabled: true
    httpGet:
      path: "/health"
      port: 8080

# Optional: ConfigMaps
configmaps:
  app-config:
    enabled: true
    mountPath: "/etc/config"
    data:
      config.yaml: |
        debug: false
        port: 8080
  
  env-vars:
    enabled: true
    envFrom: true  # Inject as environment variables
    data:
      DATABASE_URL: "postgres://..."
```

### 4. Install

```bash
helm dependency update
helm install myapp .
```

## Features

- Standard Kubernetes deployment with configurable replicas, resources, and probes
- ConfigMap support with file mounting and environment variable injection
- Automatic rolling restart when configmaps change
- Security contexts, node selection, and affinity support
