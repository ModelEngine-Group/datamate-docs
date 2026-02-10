---
title: 系统架构
description: DataMate 系统架构设计说明
weight: 2
---

{{% pageinfo %}}
本文档详细介绍 DataMate 的系统架构、技术栈和设计理念。
{{% /pageinfo %}}

## 整体架构

DataMate 采用微服务架构，将系统拆分为多个独立的服务，每个服务负责特定的业务功能。这种架构提供了良好的可扩展性、可维护性和容错性。

```
┌─────────────────────────────────────────────────────────────────┐
│                           前端层                                │
│                    (React + TypeScript)                         │
│                      Ant Design + Tailwind                      │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                        API 网关层                               │
│                    (Spring Cloud Gateway)                       │
│                      端口: 8080                                  │
└────────────────────────┬────────────────────────────────────────┘
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│  Java 后端   │ │ Python 后端  │ │  运行时服务  │
│   服务组     │ │    服务      │ │   (Ray)      │
├──────────────┤ ├──────────────┤ ├──────────────┤
│· Main App    │ │· RAG Service │ │· Operator    │
│· Data Mgmt   │ │· LangChain   │ │  Execution  │
│· Collection  │ │· FastAPI     │ │              │
│· Cleaning    │ │              │ │              │
│· Annotation  │ │              │ │              │
│· Synthesis   │ │              │ │              │
│· Evaluation  │ │              │ │              │
│· Operator    │ │              │ │              │
│· Pipeline    │ │              │ │              │
└──────┬───────┘ └──────┬───────┘ └──────┬───────┘
       │                │                │
       └────────────────┼────────────────┘
                        ▼
         ┌──────────────┴──────────────┐
         │                              │
    ┌────▼────┐    ┌─────────┐   ┌─────▼────┐
    │PostgreSQL│    │  Redis  │   │  Milvus  │
    │  (5432)  │    │ (6379)  │   │ (19530)  │
    └──────────┘    └─────────┘   └──────────┘
                                              │
                                        ┌─────▼─────┐
                                        │   MinIO   │
                                        │  (9000)   │
                                        └───────────┘
```

## 技术栈

### 前端技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| React | 18.x | UI 框架 |
| TypeScript | 5.x | 类型安全 |
| Ant Design | 5.x | UI 组件库 |
| Tailwind CSS | 3.x | 样式框架 |
| Redux Toolkit | 2.x | 状态管理 |
| React Router | 6.x | 路由管理 |
| Vite | 5.x | 构建工具 |

### 后端技术栈（Java）

| 技术 | 版本 | 用途 |
|------|------|------|
| Java | 21 | 运行时环境 |
| Spring Boot | 3.5.6 | 应用框架 |
| Spring Cloud | 2023.x | 微服务框架 |
| MyBatis Plus | 3.x | ORM 框架 |
| PostgreSQL Driver | 42.x | 数据库驱动 |
| Redis | 5.x | 缓存客户端 |
| MinIO | 8.x | 对象存储客户端 |

### 后端技术栈（Python）

| 技术 | 版本 | 用途 |
|------|------|------|
| Python | 3.11+ | 运行时环境 |
| FastAPI | 0.100+ | Web 框架 |
| LangChain | 0.1+ | LLM 应用框架 |
| Ray | 2.x | 分布式计算 |
| Pydantic | 2.x | 数据验证 |

### 数据存储

| 技术 | 版本 | 用途 |
|------|------|------|
| PostgreSQL | 15+ | 主数据库 |
| Redis | 8.x | 缓存和消息队列 |
| Milvus | 2.6.5 | 向量数据库 |
| MinIO | RELEASE.2024+ | 对象存储 |

## 微服务架构

### 服务列表

| 服务名称 | 端口 | 技术栈 | 功能描述 |
|----------|------|--------|----------|
| API Gateway | 8080 | Spring Cloud Gateway | 统一入口、路由、认证 |
| Frontend | 30000 | React | 前端界面 |
| Main Application | - | Spring Boot | 核心业务逻辑 |
| Data Management Service | 8092 | Spring Boot | 数据集管理 |
| Data Collection Service | - | Spring Boot | 数据采集任务 |
| Data Cleaning Service | - | Spring Boot | 数据清洗任务 |
| Data Annotation Service | - | Spring Boot | 数据标注任务 |
| Data Synthesis Service | - | Spring Boot | 数据合成任务 |
| Data Evaluation Service | - | Spring Boot | 数据评估任务 |
| Operator Market Service | - | Spring Boot | 算子市场 |
| RAG Indexer Service | - | Spring Boot | 知识库索引 |
| Runtime Service | 8081 | Python + Ray | 算子执行引擎 |
| Backend Python Service | 18000 | FastAPI | Python 后端服务 |
| Database | 5432 | PostgreSQL | 数据库 |

### 服务通信

#### 同步通信
- **API Gateway → 后端服务**：HTTP/REST
- **前端 → API Gateway**：HTTP/REST
- **后端服务之间**：HTTP/REST (Feign Client)

#### 异步通信
- **任务执行**：通过数据库任务队列
- **事件通知**：Redis Pub/Sub

### 服务发现与注册

DataMate 使用简单的服务注册机制：
- 服务启动时向数据库注册
- API Gateway 从数据库获取服务列表
- 支持服务的动态扩缩容

## 数据架构

### 数据流转

```
┌─────────────┐
│  数据采集   │ 采集任务配置
│  Collection │ → DataX → 原始数据
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  数据管理   │ 数据集管理、文件上传
│ Management  │ → 结构化存储
└──────┬──────┘
       │
       ├──────────────┐
       ▼              ▼
┌─────────────┐  ┌─────────────┐
│  数据清洗   │  │  知识库     │
│  Cleaning   │  │  Knowledge  │
│             │  │  Base       │
└──────┬──────┘  └──────┬──────┘
       │                │
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│  数据标注   │  │  向量索引    │
│  Annotation │  │  Milvus      │
└──────┬──────┘  └──────┬──────┘
       │                │
       ▼                │
┌─────────────┐          │
│  数据合成   │          │
│  Synthesis  │          │
└──────┬──────┘          │
       │                │
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│  数据评估   │  │  RAG 检索   │
│  Evaluation │  │  Retrieval  │
└─────────────┘  └─────────────┘
```

### 数据存储模型

#### PostgreSQL
- **业务数据**：用户、数据集、任务、配置
- **关系数据**：服务间的关联关系
- **事务数据**：需要强一致性的数据

#### Redis
- **缓存数据**：热点数据缓存
- **会话数据**：用户会话信息
- **任务队列**：异步任务队列

#### Milvus
- **向量数据**：文档向量 embeddings
- **向量索引**：高效的相似度搜索

#### MinIO
- **文件存储**：原始文件、处理结果
- **大数据文件**：不适合数据库存储的文件

## 部署架构

### Docker Compose 部署

```
┌────────────────────────────────────────────────┐
│              Docker Network                    │
│            datamate-network                    │
│                                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │Frontend  │  │ Gateway  │  │ Backend  │   │
│  │ :30000   │  │  :8080   │  │          │   │
│  └──────────┘  └──────────┘  └──────────┘   │
│                                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │Backend   │  │ Runtime  │  │Database  │   │
│  │  Python  │  │  :8081   │  │  :5432   │   │
│  └──────────┘  └──────────┘  └──────────┘   │
│                                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Milvus  │  │  MinIO   │  │  etcd    │   │
│  │  :19530  │  │  :9000   │  │          │   │
│  └──────────┘  └──────────┘  └──────────┘   │
└────────────────────────────────────────────────┘
```

### Kubernetes 部署

```
┌────────────────────────────────────────────────┐
│           Kubernetes Cluster                   │
│                                                │
│  Namespace: datamate                           │
│                                                │
│  ┌────────────┐  ┌────────────┐              │
│  │ Deployment │  │ Deployment │              │
│  │  Frontend  │  │  Gateway   │              │
│  │   (3 Pods) │  │  (2 Pods)  │              │
│  └─────┬──────┘  └─────┬──────┘              │
│        │                │                     │
│  ┌─────▼────────────────▼──────┐              │
│  │       Service (LoadBalancer) │              │
│  └──────────────────────────────┘              │
│                                                │
│  ┌────────────┐  ┌────────────┐              │
│  │ StatefulSet│  │ Deployment │              │
│  │  Database  │  │  Backend   │              │
│  └────────────┘  └────────────┘              │
└────────────────────────────────────────────────┘
```

## 安全架构

### 认证与授权

#### JWT 认证（可选）
```yaml
datamate:
  jwt:
    enable: true  # 启用 JWT 认证
    secret: your-secret-key
    expiration: 86400  # 24 小时
```

#### API Key 认证
```yaml
datamate:
  api-key:
    enable: false
```

### 数据安全

#### 传输加密
- API Gateway 支持 HTTPS/TLS
- 内部服务通信可配置加密

#### 存储加密
- 数据库支持透明数据加密 (TDE)
- MinIO 支持服务端加密
- Milvus 支持加密存储

#### 网络隔离
- Docker Network 隔离
- Kubernetes Network Policy
- VPC 隔离（云环境）

### 访问控制

#### 基于角色的访问控制 (RBAC)
- 管理员：所有权限
- 数据管理员：数据管理权限
- 标注员：标注权限
- 查看者：只读权限

#### 数据权限
- 数据集级权限控制
- 文件级权限控制
- 任务级权限控制

## 扩展性设计

### 水平扩展

#### 无状态服务
- 前端：支持多副本
- Gateway：支持多副本
- Backend 服务：支持多副本

#### 有状态服务
- 数据库：主从复制、分库分表
- Milvus：分布式部署
- MinIO：分布式纠删码

### 垂直扩展

#### 资源配置
```yaml
resources:
  requests:
    memory: "1Gi"
    cpu: "500m"
  limits:
    memory: "4Gi"
    cpu: "2000m"
```

### 缓存策略

#### 多级缓存
1. **浏览器缓存**：静态资源
2. **CDN 缓存**：公共资源
3. **Redis 缓存**：热点数据
4. **数据库缓存**：查询缓存

## 监控与可观测性

### 日志管理

#### 日志级别
- DEBUG：详细调试信息
- INFO：一般信息
- WARN：警告信息
- ERROR：错误信息

#### 日志存储
```bash
/var/log/datamate/
├── frontend/
├── backend/
├── database/
└── runtime/
```

### 性能监控

#### 指标收集
- JVM 指标
- HTTP 指标
- 数据库指标
- 自定义业务指标

#### 监控工具（可选）
- Prometheus + Grafana
- ELK Stack (Elasticsearch + Logstash + Kibana)

## 高可用设计

### 服务高可用

#### 健康检查
```yaml
livenessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

#### 自动重启
- Docker restart policy
- Kubernetes restart policy

### 数据高可用

#### 数据库
- 主从复制
- 自动故障转移
- 定期备份

#### 对象存储
- 纠删码
- 多区域复制

#### 向量数据库
- 主从架构
- 灾备恢复

## 性能优化

### 前端优化
- 代码分割
- 懒加载
- CDN 加速
- 资源压缩

### 后端优化
- 连接池优化
- 查询优化
- 缓存策略
- 异步处理

### 数据库优化
- 索引优化
- 分区表
- 查询优化
- 连接池配置

## 下一步

- [开发环境搭建](/docs/getting-started/development/) - 本地开发环境配置
- [后端架构](/docs/developer-guide/backend-architecture/) - 详细的后端架构说明
- [前端架构](/docs/developer-guide/frontend-architecture/) - 详细的前端架构说明
