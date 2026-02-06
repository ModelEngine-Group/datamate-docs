---
title: 安装部署指南
description: 详细的 DataMate 安装和配置说明
weight: 1
---

{{% pageinfo %}}
本文档提供 DataMate 平台的详细安装和配置说明。
{{% /pageinfo %}}

## 系统要求

### 最低配置

| 组件 | 最低要求 | 推荐配置 |
|------|----------|----------|
| CPU | 4 核 | 8 核+ |
| 内存 | 8 GB | 16 GB+ |
| 磁盘 | 50 GB | 100 GB+ |
| 操作系统 | Linux/macOS/Windows | Linux (Ubuntu 20.04+) |

### 软件依赖

#### Docker Compose 部署
- Docker 20.10+
- Docker Compose 2.0+
- Git (可选，用于克隆代码)
- Make (可选，用于使用 Makefile)

#### Kubernetes 部署
- Kubernetes 1.20+
- Helm 3.0+
- kubectl (与集群版本匹配)
- Git (可选，用于克隆代码)
- Make (可选，使用 Makefile)

## 部署方式对比

| 特性 | Docker Compose | Kubernetes |
|------|----------------|------------|
| 部署难度 | ⭐ 简单 | ⭐⭐⭐ 复杂 |
| 资源利用 | ⭐⭐ 一般 | ⭐⭐⭐⭐ 高 |
| 高可用 | ❌ 不支持 | ✅ 支持 |
| 扩展性 | ⭐⭐ 一般 | ⭐⭐⭐⭐ 强 |
| 适用场景 | 开发测试、小规模部署 | 生产环境、大规模部署 |

## Docker Compose 部署

### 基础部署

#### 1. 准备工作

```bash
# 克隆代码仓库
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate

# 检查 Docker 和 Docker Compose 版本
docker --version
docker compose version
```

#### 2. 使用 Makefile 部署

```bash
# 一键部署（包括 Milvus）
make install
```

在提示中选择 `1. Docker/Docker-Compose`。

#### 3. 直接使用 Docker Compose

如果没有安装 Make，可以直接使用 Docker Compose：

```bash
# 设置镜像仓库（可选）
export REGISTRY=ghcr.io/modelengine-group/

# 启动基础服务
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus up -d
```

#### 4. 验证部署

```bash
# 检查容器状态
docker ps

# 查看服务日志
docker compose -f deployment/docker/datamate/docker-compose.yml logs -f

# 访问前端界面
open http://localhost:30000
```

### 可选组件部署

#### Milvus 向量数据库

Milvus 用于知识库的向量存储和检索。

```bash
# 使用 Makefile
make install-milvus

# 或使用 Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus up -d
```

组件包括：
- milvus-standalone (19530, 9091)
- milvus-minio (9000, 9001)
- milvus-etcd

#### Label Studio 标注工具

```bash
# 使用 Makefile
make install-label-studio

# 或使用 Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile label-studio up -d
```

访问地址：http://localhost:30001

默认账号：
- 用户名：admin@demo.com
- 密码：demoadmin

#### MinerU PDF 处理服务

MinerU 提供增强的 PDF 文档解析能力，支持 NPU 加速。

```bash
# 构建 MinerU 镜像
make build-mineru

# 部署 MinerU
make install-mineru
```

支持的平台：
- mineru (默认，910B)
- mineru-910C
- mineru-310P

#### DeerFlow 工作流服务

```bash
# 使用 Makefile
make install-deer-flow

# 或使用 Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile deer-flow up -d
```

#### Redis 缓存

```bash
# 使用 Makefile
make install-redis

# 或使用 Docker Compose
docker compose -f deployment/docker/datamate/docker-compose.yml --profile redis up -d
```

### 环境变量配置

可以在 `deployment/docker/datamate/docker-compose.yml` 中配置以下环境变量：

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `DB_PASSWORD` | `password` | 数据库密码 |
| `DATAMATE_JWT_ENABLE` | `false` | 是否启用 JWT 认证 |
| `REGISTRY` | `ghcr.io/modelengine-group/` | 镜像仓库地址 |
| `VERSION` | `latest` | 镜像版本 |
| `LABEL_STUDIO_HOST` | - | Label Studio 访问地址 |

### 数据卷管理

DataMate 使用以下 Docker 卷进行数据持久化：

```bash
# 查看所有卷
docker volume ls | grep datamate

# 查看卷详情
docker volume inspect datamate-dataset-volume

# 备份卷数据
docker run --rm -v datamate-dataset-volume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/dataset-backup.tar.gz /data

# 恢复卷数据
docker run --rm -v datamate-dataset-volume:/data -v $(pwd):/backup \
  ubuntu tar xzf /backup/dataset-backup.tar.gz -C /
```

## Kubernetes/Helm 部署

### 前置准备

```bash
# 检查集群连接
kubectl cluster-info
kubectl get nodes

# 检查 Helm 版本
helm version

# 创建命名空间（可选）
kubectl create namespace datamate
```

### 使用 Makefile 部署

```bash
# 部署 DataMate
make install INSTALLER=k8s

# 或直接部署到指定命名空间
make install NAMESPACE=datamate INSTALLER=k8s
```

### 使用 Helm 部署

#### 1. 部署基础服务

```bash
# 添加 Helm 仓库（如果需要）
# helm repo add datamate https://charts.datamate.io

# 部署 DataMate
helm upgrade datamate deployment/helm/datamate/ \
  --install \
  --namespace datamate \
  --create-namespace \
  --set global.image.repository=ghcr.io/modelengine-group/

# 查看部署状态
kubectl get pods -n datamate
```

#### 2. 配置 Ingress（可选）

```bash
# 编辑 values.yaml
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

# 重新部署
helm upgrade datamate deployment/helm/datamate/ \
  --namespace datamate \
  -f deployment/helm/datamate/values.yaml
```

#### 3. 部署可选组件

```bash
# 部署 Milvus
helm upgrade milvus deployment/helm/milvus \
  --install \
  --namespace datamate

# 部署 Label Studio
helm upgrade label-studio deployment/helm/label-studio/ \
  --install \
  --namespace datamate

# 部署 DeerFlow
helm upgrade deer-flow deployment/helm/deer-flow \
  --install \
  --namespace datamate \
  --set global.image.repository=ghcr.io/modelengine-group/
```

### Helm 配置选项

主要配置项（`deployment/helm/datamate/values.yaml`）：

```yaml
# 镜像配置
global:
  image:
    repository: ghcr.io/modelengine-group/
    pullPolicy: IfNotPresent
    tag: latest

# 服务配置
services:
  frontend:
    port: 30000
  gateway:
    port: 8080
  backend:
    port: 8092

# 资源配置
resources:
  requests:
    memory: "512Mi"
    cpu: "500m"
  limits:
    memory: "2Gi"
    cpu: "2000m"

# 存储配置
persistence:
  enabled: true
  storageClass: standard
  size: 50Gi
```

### 查看日志和调试

```bash
# 查看 Pod 状态
kubectl get pods -n datamate

# 查看服务日志
kubectl logs -f deployment/datamate-backend -n datamate

# 查看所有资源
kubectl get all -n datamate

# 端口转发进行本地调试
kubectl port-forward svc/datamate-gateway 8080:8080 -n datamate
```

## 离线环境部署

### 准备离线镜像包

#### 1. 下载镜像

```bash
# 下载所有镜像到本地
make download SAVE=true

# 下载指定版本
make download VERSION=v1.0.0 SAVE=true

# 指定平台下载
make download PLATFORM=linux/amd64 SAVE=true
```

镜像将保存在 `dist/` 目录。

#### 2. 打包传输

```bash
# 打包
tar czf datamate-images.tar.gz dist/

# 传输到目标服务器
scp datamate-images.tar.gz user@target-server:/tmp/
```

### 离线安装

#### 1. 加载镜像

```bash
# 在目标服务器上解压
tar xzf datamate-images.tar.gz

# 加载所有镜像
make load-images

# 或手动加载
for img in dist/*.tar; do
  docker load -i $img
done
```

#### 2. 修改配置

使用本地镜像时，设置 `REGISTRY` 为空：

```bash
# Docker Compose 部署
REGISTRY= docker compose -f deployment/docker/datamate/docker-compose.yml up -d

# 或使用 Makefile
make install dev=true
```

#### 3. 离线依赖

某些组件可能需要额外的离线资源：

- **Python 包**：预装在运行时镜像中
- **Node 包**：预装在前端镜像中
- **Java 包**：预装在后端镜像中

## 升级指南

### Docker Compose 升级

```bash
# 1. 备份数据
docker run --rm -v datamate-postgresql-volume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/postgres-backup.tar.gz /data

# 2. 拉取新镜像
docker pull ghcr.io/modelengine-group/datamate-backend:latest
docker pull ghcr.io/modelengine-group/datamate-frontend:latest
# ... 其他镜像

# 3. 停止服务
docker compose -f deployment/docker/datamate/docker-compose.yml down

# 4. 启动新版本
docker compose -f deployment/docker/datamate/docker-compose.yml up -d

# 5. 验证升级
docker ps
docker logs -f datamate-backend
```

或使用 Makefile：

```bash
make datamate-docker-upgrade
```

### Kubernetes 升级

```bash
# 1. 备份数据
kubectl exec -n datamate deployment/datamate-database -- \
  pg_dump -U postgres datamate > backup.sql

# 2. 更新 Helm Chart
helm upgrade datamate deployment/helm/datamate/ \
  --namespace datamate \
  --set global.image.tag=new-version

# 3. 监控升级状态
kubectl rollout status deployment/datamate-backend -n datamate

# 4. 查看新 Pod 状态
kubectl get pods -n datamate
```

## 卸载

### Docker Compose 完全卸载

```bash
# 使用 Makefile
make uninstall

# 选择删除数据卷以完全清理
```

或手动卸载：

```bash
# 停止并删除容器
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus --profile label-studio down -v

# 删除所有卷
docker volume rm datamate-dataset-volume \
  datamate-postgresql-volume \
  datamate-log-volume \
  # ... 其他卷

# 删除网络
docker network rm datamate-network
```

### Kubernetes 完全卸载

```bash
# 卸载所有组件
make uninstall INSTALLER=k8s

# 或使用 Helm
helm uninstall datamate -n datamate
helm uninstall milvus -n datamate
helm uninstall label-studio -n datamate
helm uninstall deer-flow -n datamate

# 删除命名空间
kubectl delete namespace datamate

# 删除 PV/PVC（根据存储类）
kubectl delete pvc -n datamate --all
```

## 故障排查

### 常见问题

#### 1. 服务无法启动

```bash
# 检查端口占用
netstat -tlnp | grep -E '30000|8080|5432'

# 检查磁盘空间
df -h

# 检查内存使用
free -h

# 查看详细日志
docker logs datamate-backend --tail 100
```

#### 2. 数据库连接失败

```bash
# 检查数据库容器
docker ps | grep database

# 测试数据库连接
docker exec -it datamate-database psql -U postgres -d datamate

# 检查数据库日志
docker logs datamate-database
```

#### 3. 前端无法访问后端

```bash
# 检查网关配置
docker logs datamate-gateway

# 检查网络连接
docker network inspect datamate-network

# 测试 API 连接
curl http://localhost:8080/api/v1/health
```

#### 4. Milvus 连接失败

```bash
# 检查 Milvus 状态
docker logs milvus-standalone

# 检查依赖服务
docker logs milvus-etcd
docker logs milvus-minio

# 测试 Milvus 连接
curl http://localhost:19530/healthz
```

### 日志位置

| 服务 | 日志位置 |
|------|----------|
| 前端 | `/var/log/datamate/frontend` |
| 后端 | `/var/log/datamate` |
| 数据库 | `/var/log/datamate/database` |
| Ray 运行时 | `/tmp/ray` |

### 获取帮助

如果遇到问题：
1. 查看 [故障排查文档](/docs/appendix/troubleshooting/)
2. 搜索 [GitHub Issues](https://github.com/ModelEngine-Group/DataMate/issues)
3. 提交新的 Issue 并附上详细的错误日志
