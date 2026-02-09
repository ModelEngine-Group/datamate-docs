---
title: Installation Guide
description: Detailed installation and configuration instructions for DataMate
weight: 1
---

{{% pageinfo %}}
This document provides detailed installation and configuration instructions for the DataMate platform.
{{% /pageinfo %}}

## System Requirements

### Minimum Configuration

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| CPU | 4 cores | 8 cores+ |
| RAM | 8 GB | 16 GB+ |
| Disk | 50 GB | 100 GB+ |
| OS | Linux/macOS/Windows | Linux (Ubuntu 20.04+) |

### Software Dependencies

#### Docker Compose Deployment
- Docker 20.10+
- Docker Compose 2.0+
- Git (optional, for cloning code)
- Make (optional, for using Makefile)

#### Kubernetes Deployment
- Kubernetes 1.20+
- Helm 3.0+
- kubectl (matching cluster version)
- Git (optional, for cloning code)
- Make (optional, for using Makefile)

## Deployment Method Comparison

| Feature | Docker Compose | Kubernetes |
|---------|----------------|------------|
| Deployment Difficulty | ⭐ Simple | ⭐⭐⭐ Complex |
| Resource Utilization | ⭐⭐ Fair | ⭐⭐⭐⭐ High |
| High Availability | ❌ Not supported | ✅ Supported |
| Scalability | ⭐⭐ Fair | ⭐⭐⭐⭐ Strong |
| Use Case | Dev/test, small scale | Production, large scale |

## Docker Compose Deployment

### Basic Deployment

#### 1. Prerequisites

```bash
# Clone code repository
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate

# Check Docker and Docker Compose versions
docker --version
docker compose version
```

#### 2. Deploy Using Makefile

```bash
# One-click deployment (including Milvus)
make install
```

Select `1. Docker/Docker-Compose` when prompted.

#### 3. Use Docker Compose Directly

If Make is not installed:

```bash
# Set image registry (optional)
export REGISTRY=ghcr.io/modelengine-group/

# Start basic services
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus up -d
```

#### 4. Verify Deployment

```bash
# Check container status
docker ps

# View service logs
docker compose -f deployment/docker/datamate/docker-compose.yml logs -f

# Access frontend
open http://localhost:30000
```

### Optional Components

#### Milvus Vector Database

```bash
# Using Makefile
make install-milvus

# Or Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus up -d
```

Components:
- milvus-standalone (19530, 9091)
- milvus-minio (9000, 9001)
- milvus-etcd

#### Label Studio Annotation Tool

```bash
# Using Makefile
make install-label-studio

# Or Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile label-studio up -d
```

Access: http://localhost:30001

Default credentials:
- Username: admin@demo.com
- Password: demoadmin

#### MinerU PDF Processing

```bash
# Build MinerU image
make build-mineru

# Deploy MinerU
make install-mineru
```

#### DeerFlow Workflow Service

```bash
# Using Makefile
make install-deer-flow

# Or Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile deer-flow up -d
```

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `DB_PASSWORD` | `password` | Database password |
| `DATAMATE_JWT_ENABLE` | `false` | Enable JWT authentication |
| `REGISTRY` | `ghcr.io/modelengine-group/` | Image registry |
| `VERSION` | `latest` | Image version |
| `LABEL_STUDIO_HOST` | - | Label Studio access URL |

### Data Volume Management

DataMate uses Docker volumes for persistence:

```bash
# View all volumes
docker volume ls | grep datamate

# View volume details
docker volume inspect datamate-dataset-volume

# Backup volume data
docker run --rm -v datamate-dataset-volume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/dataset-backup.tar.gz /data
```

## Kubernetes/Helm Deployment

### Prerequisites

```bash
# Check cluster connection
kubectl cluster-info
kubectl get nodes

# Check Helm version
helm version

# Create namespace (optional)
kubectl create namespace datamate
```

### Using Makefile

```bash
# Deploy DataMate
make install INSTALLER=k8s

# Or deploy to specific namespace
make install NAMESPACE=datamate INSTALLER=k8s
```

### Using Helm

#### 1. Deploy Basic Services

```bash
# Deploy DataMate
helm upgrade datamate deployment/helm/datamate/ \
  --install \
  --namespace datamate \
  --create-namespace \
  --set global.image.repository=ghcr.io/modelengine-group/

# Check deployment status
kubectl get pods -n datamate
```

#### 2. Configure Ingress (Optional)

```bash
# Edit values.yaml
cat >> deployment/helm/datamate/values.yaml << EOF
ingress:
  enabled: true
  className: nginx
  hosts:
    - host: datamate.example.com
      paths:
        - path: /
          pathType: Prefix
EOF

# Redeploy
helm upgrade datamate deployment/helm/datamate/ \
  --namespace datamate \
  -f deployment/helm/datamate/values.yaml
```

#### 3. Deploy Optional Components

```bash
# Deploy Milvus
helm upgrade milvus deployment/helm/milvus \
  --install \
  --namespace datamate

# Deploy Label Studio
helm upgrade label-studio deployment/helm/label-studio/ \
  --install \
  --namespace datamate
```

## Offline Deployment

### Prepare Offline Images

#### 1. Download Images

```bash
# Download all images locally
make download SAVE=true

# Download specific version
make download VERSION=v1.0.0 SAVE=true
```

Images saved in `dist/` directory.

#### 2. Package and Transfer

```bash
# Package
tar czf datamate-images.tar.gz dist/

# Transfer to target server
scp datamate-images.tar.gz user@target-server:/tmp/
```

### Offline Installation

#### 1. Load Images

```bash
# Extract on target server
tar xzf datamate-images.tar.gz

# Load all images
make load-images
```

#### 2. Modify Configuration

Use empty REGISTRY for local images:

```bash
REGISTRY= docker compose -f deployment/docker/datamate/docker-compose.yml up -d
```

## Upgrade Guide

### Docker Compose Upgrade

```bash
# 1. Backup data
docker run --rm -v datamate-postgresql-volume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/postgres-backup.tar.gz /data

# 2. Pull new images
docker pull ghcr.io/modelengine-group/datamate-backend:latest

# 3. Stop services
docker compose -f deployment/docker/datamate/docker-compose.yml down

# 4. Start new version
docker compose -f deployment/docker/datamate/docker-compose.yml up -d

# 5. Verify upgrade
docker ps
docker logs -f datamate-backend
```

Or use Makefile:

```bash
make datamate-docker-upgrade
```

### Kubernetes Upgrade

```bash
# 1. Backup data
kubectl exec -n datamate deployment/datamate-database -- \
  pg_dump -U postgres datamate > backup.sql

# 2. Update Helm Chart
helm upgrade datamate deployment/helm/datamate/ \
  --namespace datamate \
  --set global.image.tag=new-version
```

## Uninstall

### Docker Compose Complete Uninstall

```bash
# Using Makefile
make uninstall

# Choose to delete volumes for complete cleanup
```

Or manual uninstall:

```bash
# Stop and remove containers
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus --profile label-studio down -v

# Remove all volumes
docker volume rm datamate-dataset-volume \
  datamate-postgresql-volume \
  datamate-log-volume

# Remove network
docker network rm datamate-network
```

### Kubernetes Complete Uninstall

```bash
# Uninstall all components
make uninstall INSTALLER=k8s

# Or use Helm
helm uninstall datamate -n datamate
helm uninstall milvus -n datamate
helm uninstall label-studio -n datamate

# Delete namespace
kubectl delete namespace datamate
```

## Troubleshooting

### Common Issues

#### 1. Service Won't Start

```bash
# Check port conflicts
netstat -tlnp | grep -E '30000|8080|5432'

# Check disk space
df -h

# Check memory
free -h

# View detailed logs
docker logs datamate-backend --tail 100
```

#### 2. Database Connection Failed

```bash
# Check database container
docker ps | grep database

# Test connection
docker exec -it datamate-database psql -U postgres -d datamate
```

## Related Documentation

- [Quick Start](/docs/getting-started/) - Quick deployment guide
- [System Architecture](/docs/getting-started/architecture/) - Architecture details
- [Development Setup](/docs/getting-started/development/) - Development environment
