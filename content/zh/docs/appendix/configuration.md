---
title: 配置参数
description: DataMate 系统配置参数说明
weight: 1
---

{{% pageinfo %}}
本文档详细说明 DataMate 的各种配置参数。
{{% /pageinfo %}}

## 环境变量

### 通用配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `DB_PASSWORD` | `password` | 数据库密码 |
| `DATAMATE_JWT_ENABLE` | `false` | 是否启用 JWT 认证 |
| `REGISTRY` | `ghcr.io/modelengine-group/` | 镜像仓库地址 |
| `VERSION` | `latest` | 镜像版本标签 |

### 数据库配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `DB_HOST` | `datamate-database` | 数据库主机 |
| `DB_PORT` | `5432` | 数据库端口 |
| `DB_NAME` | `datamate` | 数据库名称 |
| `DB_USER` | `postgres` | 数据库用户名 |
| `DB_PASSWORD` | `password` | 数据库密码 |

### Redis 配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `REDIS_HOST` | `datamate-redis` | Redis 主机 |
| `REDIS_PORT` | `6379` | Redis 端口 |
| `REDIS_PASSWORD` | - | Redis 密码（可选） |
| `REDIS_DB` | `0` | Redis 数据库编号 |

### Milvus 配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `MILVUS_HOST` | `milvus` | Milvus 主机 |
| `MILVUS_PORT` | `19530` | Milvus 端口 |
| `MILVUS_INDEX_TYPE` | `IVF_FLAT` | 向量索引类型 |
| `MILVUS_EMBEDDING_DIM` | `768` | 向量维度 |

### MinIO 配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `MINIO_ENDPOINT` | `minio:9000` | MinIO 端点 |
| `MINIO_ACCESS_KEY` | `minioadmin` | 访问密钥 |
| `MINIO_SECRET_KEY` | `minioadmin` | 秘密密钥 |
| `MINIO_BUCKET` | `datamate` | 存储桶名称 |
| `MINIO_USE_SSL` | `false` | 是否使用 SSL |

### LLM 配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `OPENAI_API_KEY` | - | OpenAI API 密钥 |
| `OPENAI_BASE_URL` | `https://api.openai.com/v1` | API 基础 URL |
| `OPENAI_MODEL` | `gpt-4` | 使用的模型 |

### JWT 配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `JWT_SECRET` | `default-insecure-key` | JWT 密钥（生产环境必须修改） |
| `JWT_EXPIRATION` | `86400` | Token 过期时间（秒） |

### 日志配置

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `LOG_LEVEL` | `INFO` | 日志级别 |
| `LOG_PATH` | `/var/log/datamate` | 日志路径 |

## application.yml 配置

### 主配置文件

```yaml
# application.yml
datamate:
  jwt:
    enable: ${DATAMATE_JWT_ENABLE:false}
    secret: ${JWT_SECRET:default-insecure-key}
    expiration: ${JWT_EXPIRATION:86400}

  storage:
    type: minio  # minio, local, s3
    endpoint: ${MINIO_ENDPOINT:minio:9000}
    access-key: ${MINIO_ACCESS_KEY:minioadmin}
    secret-key: ${MINIO_SECRET_KEY:minioadmin}
    bucket: ${MINIO_BUCKET:datamate}

  llm:
    provider: openai
    api-key: ${OPENAI_API_KEY:}
    base-url: ${OPENAI_BASE_URL:https://api.openai.com/v1}
    model: ${OPENAI_MODEL:gpt-4}
    temperature: 0.7
    max-tokens: 2000
```

### Spring Boot 配置

```yaml
spring:
  application:
    name: data-management-service

  datasource:
    url: jdbc:postgresql://${DB_HOST:datamate-database}:${DB_PORT:5432}/${DB_NAME:datamate}
    username: ${DB_USER:postgres}
    password: ${DB_PASSWORD:password}
    driver-class-name: org.postgresql.Driver
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000

  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
        format_sql: true

  redis:
    host: ${REDIS_HOST:datamate-redis}
    port: ${REDIS_PORT:6379}
    password: ${REDIS_PASSWORD:}
    database: ${REDIS_DB:0}
    timeout: 3000
    lettuce:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
        max-wait: -1ms

server:
  port: 8092
  servlet:
    context-path: /api/v1
  compression:
    enabled: true
  tomcat:
    max-threads: 200
    accept-count: 100
```

### 日志配置

```xml
<!-- logback-spring.xml -->
<configuration>
    <springProperty scope="context" name="LOG_LEVEL" source="log.level" defaultValue="INFO"/>
    <springProperty scope="context" name="LOG_PATH" source="log.path" defaultValue="/var/log/datamate"/>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/backend/app.log</file>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/backend/app-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
            <totalSizeCap>10GB</totalSizeCap>
        </rollingPolicy>
    </appender>

    <logger name="com.datamate" level="${LOG_LEVEL}"/>
    <logger name="org.springframework" level="INFO"/>
    <logger name="org.hibernate" level="INFO"/>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

## Docker Compose 配置

### 环境变量配置

```yaml
# docker-compose.yml
services:
  datamate-backend:
    environment:
      - DB_PASSWORD=${DB_PASSWORD:-password}
      - DATAMATE_JWT_ENABLE=${DATAMATE_JWT_ENABLE:-false}
      - LOG_LEVEL=${LOG_LEVEL:-INFO}
    volumes:
      - dataset_volume:/dataset
      - log_volume:/var/log/datamate
```

### 资源限制配置

```yaml
services:
  datamate-backend:
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 4G
        reservations:
          cpus: '1'
          memory: 2G
```

## Kubernetes 配置

### ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: datamate-config
data:
  DB_HOST: "datamate-database"
  DB_PORT: "5432"
  REDIS_HOST: "datamate-redis"
  REDIS_PORT: "6379"
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
  JWT_SECRET: eW91ci1zZWNyZXQta2V5
  OPENAI_API_KEY: eW91ci1hcGkta2V5
```

### 环境变量引用

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: datamate-backend
spec:
  template:
    spec:
      containers:
        - name: backend
          env:
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: datamate-secret
                  key: DB_PASSWORD
            - name: LOG_LEVEL
              valueFrom:
                configMapKeyRef:
                  name: datamate-config
                  key: LOG_LEVEL
```

## 前端配置

### 环境变量

```bash
# .env.development
VITE_API_BASE_URL=http://localhost:8080
VITE_API_TIMEOUT=30000
VITE_APP_TITLE=DataMate Dev
```

```bash
# .env.production
VITE_API_BASE_URL=https://api.datamate.com
VITE_API_TIMEOUT=30000
VITE_APP_TITLE=DataMate
```

### Vite 配置

```typescript
// vite.config.ts
export default defineConfig({
  server: {
    port: 3000,
    proxy: {
      '/api': {
        target: env.VITE_API_BASE_URL,
        changeOrigin: true,
      },
    },
  },
  build: {
    outDir: 'dist',
    sourcemap: false,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom', 'react-router-dom'],
          antd: ['antd'],
        },
      },
    },
  },
});
```

## Python 运行时配置

### 环境变量

```bash
# Python 服务配置
PG_HOST=datamate-database
PG_PORT=5432
PG_USER=postgres
PG_PASSWORD=password
PG_DATABASE=datamate

log_level=DEBUG
datamate_jwt_enable=false
```

### 配置文件

```python
# config.py
import os

class Config:
    # PostgreSQL
    PG_HOST = os.getenv('PG_HOST', 'datamate-database')
    PG_PORT = int(os.getenv('PG_PORT', 5432))
    PG_USER = os.getenv('PG_USER', 'postgres')
    PG_PASSWORD = os.getenv('PG_PASSWORD', 'password')
    PG_DATABASE = os.getenv('PG_DATABASE', 'datamate')

    # 日志
    LOG_LEVEL = os.getenv('log_level', 'INFO')

    # JWT
    JWT_ENABLE = os.getenv('datamate_jwt_enable', 'false').lower() == 'true'

    # Milvus
    MILVUS_HOST = os.getenv('MILVUS_HOST', 'milvus')
    MILVUS_PORT = int(os.getenv('MILVUS_PORT', 19530))
```

## 性能调优参数

### 数据库连接池

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20      # 最大连接数
      minimum-idle: 5            # 最小空闲连接数
      connection-timeout: 30000  # 连接超时（毫秒）
      idle-timeout: 600000       # 空闲超时（毫秒）
      max-lifetime: 1800000      # 连接最大生命周期（毫秒）
```

### Redis 连接池

```yaml
spring:
  redis:
    lettuce:
      pool:
        max-active: 8      # 最大活跃连接数
        max-idle: 8        # 最大空闲连接数
        min-idle: 0        # 最小空闲连接数
        max-wait: -1ms     # 最大等待时间
```

### JVM 参数

```bash
# JVM 调优参数
JAVA_OPTS="-Xms2g -Xmx4g -XX:+UseG1GC -XX:MaxGCPauseMillis=200"
```

## 相关文档

- [安装部署指南](/docs/getting-started/installation/) - 部署配置说明
- [故障排查](/docs/appendix/troubleshooting/) - 常见问题解决
- [后端架构](/docs/developer-guide/backend-architecture/) - 后端架构设计
