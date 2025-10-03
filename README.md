# Present Now Helm Chart

A Helm chart for deploying the Present Now application, which includes a Quarkus-based backend, React frontend, and PostgreSQL database.

## Components

- **Backend**: Quarkus application serving the API
- **Frontend**: React-based web interface
- **PostgreSQL**: Database for data persistence
- **Ingress**: Configured with TLS termination

**Note**: Database credentials are managed via an external Kubernetes secret named `present-now-secrets`.

## Prerequisites

- Kubernetes cluster
- Helm 3.x
- cert-manager (for TLS certificates)

## Secrets

The chart requires a Kubernetes secret containing database credentials. Create it before installing the chart:

```bash
kubectl create secret generic present-now-secrets \
  --from-literal=postgres-user=your-username \
  --from-literal=postgres-password=your-password
```

Replace `your-username` and `your-password` with your actual database credentials.

## Installation

```bash
helm install present-now . -n presentnow-dev
```

## Upgrading

To upgrade an existing release:

```bash
helm upgrade present-now . -n presentnow-dev
```

## Configuration

The following table lists the configurable parameters of the Present Now chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `backend.image` | Backend container image | `ghcr.io/workaround-org/presentnow-backend:latest` |
| `backend.replicas` | Number of backend replicas | `2` |
| `frontend.image` | Frontend container image | `code.mymiggi.de/miggi/presentnow-frontend-v2` |
| `frontend.replicas` | Number of frontend replicas | `2` |
| `postgres.image` | PostgreSQL container image | `postgres:16-alpine` |
| `postgres.storage` | PostgreSQL storage size | `1Gi` |
| `postgres.persistence.enabled` | Enable PostgreSQL PVC | `true` |
| `ingress.hosts` | List of ingress hosts | `["presentnow.dev.ha1nz.de"]` |
| `ingress.tlsSecret` | TLS secret name | `presentnow-tls` |

## Values

For detailed configuration options, see `values.yaml`.