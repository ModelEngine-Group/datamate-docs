---
title: 开发者指南
description: DataMate 架构和开发指南
weight: 5
---

{{% pageinfo %}}
开发者指南介绍 DataMate 的技术架构、开发环境和贡献流程。
{{% /pageinfo %}}

DataMate 是一个企业级的数据处理平台，采用微服务架构，支持大规模数据处理和自定义扩展。

## 架构文档

- [后端架构](/docs/developer-guide/backend-architecture/) - Java 微服务架构设计
- [前端架构](/docs/developer-guide/frontend-architecture/) - React 前端架构设计
- [数据库设计](/docs/developer-guide/database-schema/) - 数据库表结构设计

## 开发指南

- [开发环境搭建](/docs/getting-started/development/) - 本地开发环境配置
- [贡献指南](/docs/developer-guide/contribute/) - 代码贡献流程
- [部署指南](/docs/developer-guide/deployment/) - 生产环境部署

## 技术栈

### 前端

| 技术 | 版本 | 说明 |
|------|------|------|
| React | 18.x | UI 框架 |
| TypeScript | 5.x | 类型安全 |
| Ant Design | 5.x | UI 组件库 |
| Redux Toolkit | 2.x | 状态管理 |
| Vite | 5.x | 构建工具 |

### 后端 (Java)

| 技术 | 版本 | 说明 |
|------|------|------|
| Java | 21 | 运行时环境 |
| Spring Boot | 3.5.6 | 应用框架 |
| Spring Cloud | 2023.x | 微服务框架 |
| MyBatis Plus | 3.x | ORM 框架 |

### 后端 (Python)

| 技术 | 版本 | 说明 |
|------|------|------|
| Python | 3.11+ | 运行时环境 |
| FastAPI | 0.100+ | Web 框架 |
| LangChain | 0.1+ | LLM 框架 |
| Ray | 2.x | 分布式计算 |

## 项目结构

```
DataMate/
├── backend/                 # Java 后端
│   ├── services/           # 微服务模块
│   ├── openapi/            # OpenAPI 规范
│   └── scripts/            # 构建脚本
├── frontend/               # React 前端
│   ├── src/
│   │   ├── components/    # 公共组件
│   │   ├── pages/         # 页面组件
│   │   ├── services/      # API 服务
│   │   └── store/         # Redux store
│   └── package.json
├── runtime/                # Python 运行时
│   └── datamate/          # DataMate 运行时
└── deployment/             # 部署配置
    ├── docker/            # Docker 配置
    └── helm/              # Helm Charts
```

## 快速开始

### 1. 克隆代码

```bash
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate
```

### 2. 启动服务

```bash
# 启动基础服务
make install

# 访问前端
open http://localhost:30000
```

### 3. 开发模式

```bash
# 后端开发
cd backend/services/main-application
mvn spring-boot:run

# 前端开发
cd frontend
pnpm dev

# Python 服务开发
cd runtime/datamate
python operator_runtime.py --port 8081
```

## 核心概念

### 微服务架构

DataMate 采用微服务架构，每个服务负责特定的业务功能：

- **API Gateway**：统一入口、路由、认证
- **Main Application**：核心业务逻辑
- **Data Management Service**：数据集管理
- **Data Cleaning Service**：数据清洗
- **Data Synthesis Service**：数据合成
- **Runtime Service**：算子执行

### 算子系统

算子是数据处理的基本单元：

- **内置算子**：平台提供的常用算子
- **自定义算子**：用户开发的自定义算子
- **算子执行**：由 Runtime Service 执行

### 流水线编排

流水线通过可视化编排实现：

- **节点**：数据处理的基本单元
- **连接**：节点间的数据流
- **执行**：按流程自动执行

## 扩展开发

### 开发自定义算子

算子开发指南：

1. [算子市场](/docs/user-guide/operator-market/) - 算子使用指南
2. Python 算子开发示例
3. 算子测试和调试

### 集成外部系统

- **API 集成**：通过 REST API 集成
- **Webhook**：事件通知
- **插件系统**：（即将推出）

## 测试

### 单元测试

```bash
# 后端测试
cd backend
mvn test

# 前端测试
cd frontend
pnpm test

# Python 测试
cd runtime
pytest
```

### 集成测试

```bash
# 启动测试环境
make test-env-up

# 运行集成测试
make integration-test

# 清理测试环境
make test-env-down
```

## 性能优化

### 后端优化

- 数据库连接池配置
- 查询优化
- 缓存策略
- 异步处理

### 前端优化

- 代码分割
- 懒加载
- 缓存策略

## 安全

### 认证授权

- JWT 认证
- RBAC 权限控制
- API Key 认证

### 数据安全

- 传输加密（HTTPS/TLS）
- 存储加密
- 敏感数据脱敏

## 相关资源

- [GitHub 仓库](https://github.com/ModelEngine-Group/DataMate)
- [Issue 追踪](https://github.com/ModelEngine-Group/DataMate/issues)
- [OpenAPI 规范](https://github.com/ModelEngine-Group/DataMate/tree/main/backend/openapi/specs)
- [API 参考](/docs/api-reference/) - 完整的 API 文档
