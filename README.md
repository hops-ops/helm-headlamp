# helm-headlamp

Crossplane configuration that wraps the [Headlamp](https://headlamp.dev/) Helm chart, providing a minimal, stable interface for deploying the modern Kubernetes dashboard.

## Usage

### Minimal

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Headlamp
metadata:
  name: headlamp
  namespace: my-env
spec:
  clusterName: my-cluster
```

### With OIDC Authentication

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Headlamp
metadata:
  name: headlamp
  namespace: my-env
spec:
  clusterName: my-cluster

  labels:
    team: platform

  helm:
    namespace: headlamp
    values:
      replicaCount: 2
      config:
        oidc:
          clientID: "headlamp"
          clientSecret: "your-secret"
          issuerURL: "https://dex.example.com"
          scopes: "openid profile email groups"
      ingress:
        enabled: true
        hosts:
          - host: headlamp.example.com
            paths:
              - path: /
                pathType: Prefix
```

## Configuration

| Field | Description | Default |
|-------|-------------|---------|
| `spec.clusterName` | Target cluster name (used as default providerConfigRef) | Required |
| `spec.labels` | Labels applied to all resources | `{}` |
| `spec.providerConfigRef.name` | Helm ProviderConfig name | `<clusterName>` |
| `spec.providerConfigRef.kind` | Helm ProviderConfig kind | `ProviderConfig` |
| `spec.helm.namespace` | Namespace for the Helm release | `headlamp` |
| `spec.helm.name` | Name of the Helm release | `headlamp` |
| `spec.helm.values` | Helm values (merged with defaults) | `{}` |
| `spec.helm.overrideAllValues` | Helm values that replace all defaults | `{}` |

## Chart Info

- **Chart**: headlamp
- **Repository**: https://kubernetes-sigs.github.io/headlamp/
- **Version**: 0.27.0
