# Present Now Helm Chart

A Helm chart for deploying the Present Now application, which includes a Quarkus-based backend, React frontend, and PostgreSQL database.

## Components

- **Backend**: Quarkus application serving the API
- **Frontend**: React-based web interface
- **PostgreSQL**: Database for data persistence
- **Ingress**: Configured with TLS termination

## Prerequisites

- Kubernetes cluster
- Helm 3.x
- cert-manager (for TLS certificates)

## Installation

```bash
helm install present-now ./present-now-chart
```

## Upgrading

To upgrade an existing release:

```bash
helm upgrade present-now ./present-now-chart
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
| `ingress.host` | Ingress host | `presentnow.dev.ha1nz.de` |
| `ingress.tlsSecret` | TLS secret name | `presentnow-tls` |

## Values

For detailed configuration options, see `values.yaml`.