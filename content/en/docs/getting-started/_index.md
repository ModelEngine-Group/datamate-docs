---
title: Quick Start
description: Deploy DataMate in 5 minutes
categories: [Quick Start, Deployment]
tags: [kubernetes, docker]
weight: 2
---

{{% pageinfo %}}
This guide will help you deploy DataMate platform in 5 minutes.
{{% /pageinfo %}}

DataMate supports two main deployment methods:
- **Docker Compose**: Suitable for quick experience and development testing
- **Kubernetes/Helm**: Suitable for production deployment

## Prerequisites

### Docker Compose Deployment

- Docker 20.10+
- Docker Compose 2.0+
- At least 4GB RAM
- At least 10GB disk space

### Kubernetes Deployment

- Kubernetes 1.20+
- Helm 3.0+
- kubectl configured with cluster connection
- At least 8GB RAM
- At least 20GB disk space

## 5-Minute Quick Deployment (Docker Compose)

### 1. Clone the Code

```bash
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate
```

### 2. Start Services

Use the provided Makefile for one-click deployment:

```bash
make install
```

After running the command, the system will prompt you to select a deployment method:

```shell
Choose a deployment method:
1. Docker/Docker-Compose
2. Kubernetes/Helm
Enter choice:
```

Enter `1` to select Docker Compose deployment.

### 3. Verify Deployment

After services start, you can access them at:

- **Frontend**: http://localhost:30000
- **API Gateway**: http://localhost:8080
- **Database**: localhost:5432

### 4. Check Service Status

```bash
docker ps
```

You should see the following containers running:

- datamate-frontend (Frontend service)
- datamate-backend (Backend service)
- datamate-backend-python (Python backend service)
- datamate-gateway (API gateway)
- datamate-database (PostgreSQL database)
- datamate-runtime (Operator runtime)

## Optional Components Installation

### Install Milvus Vector Database

Milvus is used for vector storage and retrieval in knowledge bases:

```bash
make install-milvus
```

Select Docker Compose deployment method when prompted.

### Install Label Studio Annotation Tool

Label Studio is used for data annotation:

```bash
make install-label-studio
```

Access: http://localhost:30001

Default credentials:
- Username: admin@demo.com
- Password: demoadmin

### Install MinerU PDF Processing Service

MinerU provides enhanced PDF document processing:

```bash
make build-mineru
make install-mineru
```

### Install DeerFlow Service

DeerFlow is used for enhanced workflow orchestration:

```bash
make install-deer-flow
```

## Using Local Images for Development

If you've modified local code, use local images for deployment:

```bash
make build
make install dev=true
```

## Offline Environment Deployment

For offline environments, download all images first:

```bash
make download SAVE=true
```

Images will be saved in the `dist/` directory. Load images on the target machine:

```bash
make load-images
```

## Uninstall

### Uninstall DataMate

```bash
make uninstall
```

The system will prompt whether to delete volumes:
- Select `1`: Delete all data (including datasets, configurations, etc.)
- Select `2`: Keep volumes

### Uninstall Specific Components

```bash
# Uninstall Label Studio
make uninstall-label-studio

# Uninstall Milvus
make uninstall-milvus

# Uninstall DeerFlow
make uninstall-deer-flow
```

## Next Steps

- [Installation Guide](/docs/getting-started/installation/) - Detailed installation and configuration
- [System Architecture](/docs/getting-started/architecture/) - Understand DataMate architecture
- [Development Setup](/docs/getting-started/development/) - Local development environment

## Common Questions

### Q: What if service startup fails?

First check if ports are occupied:

```bash
# Check port usage
lsof -i :30000
lsof -i :8080
```

If ports are occupied, modify port mappings in `deployment/docker/datamate/docker-compose.yml`.

### Q: How to view service logs?

```bash
# View all service logs
docker compose -f deployment/docker/datamate/docker-compose.yml logs

# View specific service logs
docker compose -f deployment/docker/datamate/docker-compose.yml logs -f datamate-backend
```

### Q: Where is data stored?

Data is persisted through Docker volumes:

- `datamate-dataset-volume`: Dataset files
- `datamate-postgresql-volume`: Database data
- `datamate-log-volume`: Log files

View all volumes:

```bash
docker volume ls | grep datamate
```