# Pulse Official Helm Chart (`pulse-helm`)

Official production Kubernetes Helm Chart for deploying the **Pulse Distributed Messaging Platform**.

## 🏛️ Architecture Overview

The chart packages two decoupled operational components:
1. **Stateless Gateway Deployment**:
   - WebSocket connection termination and JWT authentication.
   - Horizontal Pod Autoscaler (HPA) scaling with CPU and active connections.
   - PodDisruptionBudget (PDB) guaranteeing high availability during node maintenance.
2. **Domain Cell StatefulSet**:
   - Sharded domain engine executing monotonic gapless sequence counters (`convSeq`).
   - Persistent storage for local caching and audit projections.

## 🚀 Installation

```bash
# Add Pulse Helm repo (or install directly from charts/pulse)
helm repo add pulse https://charts.pulse.dev
helm repo update

# Install with production values
helm install pulse-cluster ./charts/pulse \
  --namespace pulse \
  --create-namespace \
  --values production-values.yaml
```

## ⚙️ Configuration Parameters

| Parameter | Description | Default |
| :--- | :--- | :--- |
| `gateway.replicaCount` | Initial replica count for Gateway nodes | `3` |
| `gateway.autoscaling.enabled` | Enable Horizontal Pod Autoscaler | `true` |
| `gateway.autoscaling.maxReplicas` | Max pod count for Gateway | `10` |
| `cell.replicas` | Replica count for sharded domain cells | `2` |
| `cell.persistence.size` | PVC storage size per Cell replica | `20Gi` |
| `ingress.enabled` | Ingress resource with WebSocket upgrade | `true` |
| `metrics.serviceMonitor.enabled` | Prometheus Operator ServiceMonitor | `true` |
| `metrics.grafanaDashboard.enabled` | ConfigMap containing Grafana telemetry | `true` |

