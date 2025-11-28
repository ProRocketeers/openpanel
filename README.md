# OpenPanel Helm Chart

A Helm chart for deploying OpenPanel analytics platform on Kubernetes.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- kubectl configured to access your cluster

## Installation

### Quick Start

1. **Clone or navigate to the chart directory:**
   ```bash
   cd openpanel
   ```

2. **Review and update `values.yaml`** with your configuration:
   - Update domain names
   - Set PostgreSQL configuration (self-hosted or external)
   - Configure secrets (cookie secret, API keys, etc.)

3. **Install the chart:**
   ```bash
   helm install openpanel . --namespace openpanel --create-namespace
   ```

### Using External PostgreSQL

To use an external PostgreSQL database:

1. Set `postgresql.enabled: false` in `values.yaml`
2. Configure `externalPostgresql` section:
   ```yaml
   externalPostgresql:
     host: "your-postgres-host"
     port: 5432
     user: postgres
     password: "your-password"
     database: openpanel
     schema: public
   ```

### Using Self-Hosted PostgreSQL

To deploy PostgreSQL in Kubernetes:

1. Set `postgresql.enabled: true` in `values.yaml`
2. Configure PostgreSQL settings:
   ```yaml
   postgresql:
     enabled: true
     user: postgres
     password: "your-secure-password"
     database: postgres
     persistence:
       size: 20Gi
   ```

## Configuration

### Key Configuration Options

#### PostgreSQL
- `postgresql.enabled`: Enable/disable self-hosted PostgreSQL
- `externalPostgresql.*`: Configuration for external PostgreSQL

#### Redis
- `redis.enabled`: Enable/disable self-hosted Redis
- `externalRedis.*`: Configuration for external Redis

#### ClickHouse
- `clickhouse.enabled`: Enable/disable self-hosted ClickHouse
- `externalClickhouse.*`: Configuration for external ClickHouse

#### Ingress
- `ingress.enabled`: Enable/disable ingress
- `ingress.type`: Choose `httpproxy` (Contour) or `standard` (nginx/traefik)
- `ingress.fqdn`: Your domain name

#### Application Components
- `api.enabled`: Enable/disable API service
- `dashboard.enabled`: Enable/disable Dashboard service
- `worker.enabled`: Enable/disable Worker service

### Required Secrets

Before installation, make sure to update these in `values.yaml`:

1. **COOKIE_SECRET**: Generate with `openssl rand -base64 32`
2. **Database passwords**: Update PostgreSQL credentials
3. **API keys**: Configure email, AI, and OAuth keys as needed

## Upgrading

```bash
helm upgrade openpanel . --namespace openpanel
```

## Uninstallation

```bash
helm uninstall openpanel --namespace openpanel
```

**Note:** This will delete all resources including persistent volumes. Make sure to backup your data before uninstalling.

## Troubleshooting

### Check Pod Status
```bash
kubectl get pods -n openpanel
```

### View Logs
```bash
# API logs
kubectl logs -f deployment/op-api -n openpanel

# Dashboard logs
kubectl logs -f deployment/op-dashboard -n openpanel

# Worker logs
kubectl logs -f deployment/op-worker -n openpanel
```

### Check Services
```bash
kubectl get svc -n openpanel
```

### Check ConfigMap and Secrets
```bash
kubectl get configmap openpanel-config -n openpanel -o yaml
kubectl get secret openpanel-secrets -n openpanel -o yaml
```

## Values Reference

See `values.yaml` for all available configuration options with detailed comments.

## Support

For issues and questions, please refer to the OpenPanel documentation or create an issue in the repository.
