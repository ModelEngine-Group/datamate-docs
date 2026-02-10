---
title: Configuration
description: DataMate system configuration parameters
weight: 1
---

{{% pageinfo %}}
This document details various configuration parameters of the DataMate system.
{{% /pageinfo %}}

## Environment Variables

### Common Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `DB_PASSWORD` | `password` | Database password |
| `DATAMATE_JWT_ENABLE` | `false` | Enable JWT authentication |
| `REGISTRY` | `ghcr.io/modelengine-group/` | Image registry |
| `VERSION` | `latest` | Image version tag |

### Database Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `DB_HOST` | `datamate-database` | Database host |
| `DB_PORT` | `5432` | Database port |
| `DB_NAME` | `datamate` | Database name |
| `DB_USER` | `postgres` | Database username |
| `DB_PASSWORD` | `password` | Database password |

### Redis Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `REDIS_HOST` | `datamate-redis` | Redis host |
| `REDIS_PORT` | `6379` | Redis port |
| `REDIS_PASSWORD` | - | Redis password (optional) |
| `REDIS_DB` | `0` | Redis database number |

### Milvus Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `MILVUS_HOST` | `milvus` | Milvus host |
| `MILVUS_PORT` | `19530` | Milvus port |
| `MILVUS_INDEX_TYPE` | `IVF_FLAT` | Vector index type |
| `MILVUS_EMBEDDING_DIM` | `768` | Vector dimension |

### MinIO Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `MINIO_ENDPOINT` | `minio:9000` | MinIO endpoint |
| `MINIO_ACCESS_KEY` | `minioadmin` | Access key |
| `MINIO_SECRET_KEY` | `minioadmin` | Secret key |
| `MINIO_BUCKET` | `datamate` | Bucket name |

### LLM Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `OPENAI_API_KEY` | - | OpenAI API key |
| `OPENAI_BASE_URL` | `https://api.openai.com/v1` | API base URL |
| `OPENAI_MODEL` | `gpt-4` | Model to use |

### JWT Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `JWT_SECRET` | `default-insecure-key` | JWT secret (CHANGE IN PRODUCTION) |
| `JWT_EXPIRATION` | `86400` | Token expiration (seconds) |

### Logging Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `LOG_LEVEL` | `INFO` | Log level |
| `LOG_PATH` | `/var/log/datamate` | Log path |

## application.yml Configuration

### Main Config

```yaml
datamate:
  jwt:
    enable: ${DATAMATE_JWT_ENABLE:false}
    secret: ${JWT_SECRET:default-insecure-key}
    expiration: ${JWT_EXPIRATION:86400}

  storage:
    type: minio
    endpoint: ${MINIO_ENDPOINT:minio:9000}
    access-key: ${MINIO_ACCESS_KEY:minioadmin}
    secret-key: ${MINIO_SECRET_KEY:minioadmin}
```

### Spring Boot Config

```yaml
spring:
  datasource:
    url: jdbc:postgresql://${DB_HOST:datamate-database}:${DB_PORT:5432}/${DB_NAME:datamate}
    username: ${DB_USER:postgres}
    password: ${DB_PASSWORD:password}

  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false

server:
  port: 8092
```

## Docker Compose Configuration

### Environment Variables

```yaml
services:
  datamate-backend:
    environment:
      - DB_PASSWORD=${DB_PASSWORD:-password}
      - LOG_LEVEL=${LOG_LEVEL:-INFO}
```

### Resource Limits

```yaml
services:
  datamate-backend:
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 4G
```

## Kubernetes Configuration

### ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: datamate-config
data:
  LOG_LEVEL: "INFO"
```

### Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: datamate-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=  # base64 encoded
```

## Performance Tuning

### Database Connection Pool

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
```

### JVM Parameters

```bash
JAVA_OPTS="-Xms2g -Xmx4g -XX:+UseG1GC"
```

## Related Documentation

- [Installation Guide](/docs/getting-started/installation/) - Deployment configuration
- [Troubleshooting](/docs/appendix/troubleshooting/) - Common issues
