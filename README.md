# Present Now Helm Chart

A Helm chart for deploying the Present Now application, which includes a Quarkus-based backend, React frontend, and PostgreSQL database.

## Components

- **Backend**: Quarkus application serving the API
- **Frontend**: React-based web interface
- **PostgreSQL**: Database for data persistence
- **Ingress**: Configured with TLS termination

**Note**: Database credentials are read from an external Kubernetes secret. Configure it with `backend.database.secretName` (default fallback: `unstable-postgres-quarkus`). The JDBC URL is read from that secret using `backend.database.jdbcUrlSecretKey` (default `jdbc-uri`) and falls back to `backend.env.QUARKUS_DATASOURCE_JDBC_URL` when no secret name is configured. OIDC values are configured via `backend.oidc.*`, and PresentNow settings via `backend.presentnow.*`.

## Prerequisites

- Kubernetes cluster
- Helm 3.x
- cert-manager (for TLS certificates)

## Secrets

The chart requires a Kubernetes secret containing database credentials. Create it before installing the chart:

```bash
kubectl create secret generic presentnow-postgres-app \
  --from-literal=username=your-username \
  --from-literal=password=your-password \
  --from-literal=jdbc-uri='jdbc:postgresql://your-postgres-rw:5432/presentnow'
```

Replace values with your actual database credentials and JDBC URL, and set `backend.database.secretName` to that secret name.

## Installation (OCI from GitHub Container Registry)

```bash
helm install present-now oci://ghcr.io/workaround-org/charts/present-now \
  --version 0.1.1 \
  -n presentnow-dev -f values-dev.yaml
```

You can also pull and inspect the packaged chart:

```bash
helm pull oci://ghcr.io/workaround-org/charts/present-now --version 0.1.1
```

## Local Installation (from this repository)

```bash
helm install present-now . -n presentnow-dev -f values-dev.yaml
```

## Upgrading

To upgrade an existing release:

```bash
helm upgrade present-now oci://ghcr.io/workaround-org/charts/present-now \
  --version 0.1.1 \
  -n presentnow-dev -f values-dev.yaml
```

## Configuration

The following table lists the configurable parameters of the Present Now chart and their default values.

| Parameter                                 | Description | Default |
|-------------------------------------------|-------------|---------|
| `backend.image`                           | Backend container image | `ghcr.io/workaround-org/presentnow-backend:latest` |
| `backend.replicas`                        | Number of backend replicas | `2` |
| `backend.database.secretName`             | Secret name containing DB `username`/`password` keys | `unstable-postgres-quarkus` |
| `backend.database.jdbcUrlSecretKey`       | Secret key holding JDBC URL | `jdbc-uri` |
| `backend.searchEngine`                    | Search engine URL prefix for backend | `https://www.google.com/search?q=` |
| `backend.oidc.audience`                   | Audience value exposed as backend env var | `""` |
| `backend.oidc.authServerUrl`              | OIDC auth server URL | `""` |
| `backend.oidc.clientId`                   | OIDC client ID | `""` |
| `backend.extraEnv`                        | Additional backend env entries (Kubernetes env list format) | `[]` |
| `backend.env.QUARKUS_DATASOURCE_JDBC_URL` | Optional direct JDBC URL override | `""` |
| `frontend.image`                          | Frontend container image | `ghcr.io/workaround-org/presentnow-frontend:latest` |
| `frontend.replicas`                       | Number of frontend replicas | `2` |
| `postgres.image`                          | PostgreSQL container image | `postgres:16-alpine` |
| `postgres.storage`                        | PostgreSQL storage size | `1Gi` |
| `postgres.persistence.enabled`            | Enable PostgreSQL PVC | `true` |
| `ingress.hosts`                           | List of ingress hosts | `["presentnow.dev.ha1nz.de"]` |
| `ingress.tlsSecret`                       | TLS secret name | `presentnow-tls` |

## Values

For detailed configuration options, see `values.yaml`.
