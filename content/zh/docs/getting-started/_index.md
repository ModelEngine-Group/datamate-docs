---
title: 快速开始
description: 5 分钟快速部署 DataMate
categories: [快速开始, 部署]
tags: [kubernetes, docker]
weight: 2
---

{{% pageinfo %}}
本指南将帮助您在 5 分钟内快速部署 DataMate 平台。
{{% /pageinfo %}}

DataMate 支持两种主要的部署方式：
- **Docker Compose**：适合快速体验、开发测试环境
- **Kubernetes/Helm**：适合生产环境部署

## 前置条件

### Docker Compose 部署

- Docker 20.10+
- Docker Compose 2.0+
- 至少 4GB 内存
- 至少 10GB 磁盘空间

### Kubernetes 部署

- Kubernetes 1.20+
- Helm 3.0+
- kubectl 配置好集群连接
- 至少 8GB 内存
- 至少 20GB 磁盘空间

## 5 分钟快速部署（Docker Compose）

### 1. 拉取代码

```bash
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate
```

### 2. 启动服务

使用提供的 Makefile 进行一键部署：

```bash
make install
```

执行命令后，系统会提示选择部署方式：

```shell
Choose a deployment method:
1. Docker/Docker-Compose
2. Kubernetes/Helm
Enter choice:
```

输入 `1` 选择 Docker Compose 部署。

### 3. 验证部署

服务启动后，可以通过以下方式访问：

- **前端界面**：http://localhost:30000
- **API 网关**：http://localhost:8080
- **数据库**：localhost:5432

### 4. 查看服务状态

```bash
docker ps
```

您应该能看到以下容器在运行：

- datamate-frontend (前端服务)
- datamate-backend (后端服务)
- datamate-backend-python (Python 后端服务)
- datamate-gateway (API 网关)
- datamate-database (PostgreSQL 数据库)
- datamate-runtime (算子运行时)

## 可选组件安装

### 安装 Milvus 向量数据库

Milvus 用于知识库的向量存储和检索：

```bash
make install-milvus
```

选择 Docker Compose 部署方式。

### 安装 Label Studio 标注工具

Label Studio 用于数据标注功能：

```bash
make install-label-studio
```

访问地址：http://localhost:30001

默认账号：
- 用户名：admin@demo.com
- 密码：demoadmin

### 安装 MinerU PDF 处理服务

MinerU 用于增强 PDF 文档处理能力：

```bash
make build-mineru
make install-mineru
```

### 安装 DeerFlow 服务

DeerFlow 用于工作流编排增强功能：

```bash
make install-deer-flow
```

## 使用本地镜像开发

如果您修改了本地代码，可以使用本地镜像进行部署：

```bash
make build
make install dev=true
```

## 离线环境部署

对于离线环境，可以预先下载所有镜像：

```bash
make download SAVE=true
```

镜像将保存在 `dist/` 目录下。在目标机器上加载镜像：

```bash
make load-images
```

## 卸载

### 卸载 DataMate

```bash
make uninstall
```

系统会提示是否删除数据卷：
- 选择 `1`：删除所有数据（包括数据集、配置等）
- 选择 `2`：保留数据卷

### 仅卸载特定组件

```bash
# 卸载 Label Studio
make uninstall-label-studio

# 卸载 Milvus
make uninstall-milvus

# 卸载 DeerFlow
make uninstall-deer-flow
```

## 下一步

- [安装部署指南](/docs/getting-started/installation/) - 详细的安装和配置说明
- [系统架构](/docs/getting-started/architecture/) - 了解 DataMate 的技术架构
- [开发环境搭建](/docs/getting-started/development/) - 本地开发环境配置

## 常见问题

### Q: 服务启动失败怎么办？

A: 首先检查端口是否被占用：

```bash
# 检查端口占用
lsof -i :30000
lsof -i :8080
```

如果端口被占用，可以修改 `deployment/docker/datamate/docker-compose.yml` 中的端口映射。

### Q: 如何查看服务日志？

```bash
# 查看所有服务日志
docker compose -f deployment/docker/datamate/docker-compose.yml logs

# 查看特定服务日志
docker compose -f deployment/docker/datamate/docker-compose.yml logs -f datamate-backend
```

### Q: 数据存储在哪里？

数据通过 Docker 卷进行持久化存储：

- `datamate-dataset-volume`：数据集文件
- `datamate-postgresql-volume`：数据库数据
- `datamate-log-volume`：日志文件

可以使用以下命令查看所有卷：

```bash
docker volume ls | grep datamate
```
