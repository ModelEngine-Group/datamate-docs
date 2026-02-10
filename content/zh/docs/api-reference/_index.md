---
title: API 参考
description: DataMate API 文档
weight: 4
---

{{% pageinfo %}}
DataMate 提供完整的 REST API，支持所有核心功能的编程访问。
{{% /pageinfo %}}

## API 概述

DataMate API 基于 REST 架构设计，提供以下服务：

- **数据管理 API**：数据集和文件管理
- **数据清洗 API**：数据清洗任务管理
- **数据采集 API**：数据采集任务管理
- **数据标注 API**：数据标注任务管理
- **数据合成 API**：数据合成任务管理
- **数据评估 API**：数据评估任务管理
- **算子市场 API**：算子管理
- **RAG 索引 API**：知识库和向量检索
- **流水线编排 API**：流程编排管理

## 认证方式

DataMate 支持两种认证方式：

### JWT 认证（推荐）

```http
GET /api/v1/data-management/datasets
Authorization: Bearer <your-jwt-token>
```

获取 JWT Token：

```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "username": "admin",
  "password": "password"
}
```

响应：

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 86400
}
```

### API Key 认证

```http
GET /api/v1/data-management/datasets
X-API-Key: <your-api-key>
```

## 通用响应格式

### 成功响应

```json
{
  "code": 200,
  "message": "success",
  "data": {
    // 响应数据
  }
}
```

### 错误响应

```json
{
  "code": 400,
  "message": "Bad Request",
  "error": "Invalid parameter: datasetId",
  "timestamp": "2024-01-15T10:30:00Z",
  "path": "/api/v1/data-management/datasets"
}
```

### 分页响应

```json
{
  "content": [],
  "page": 0,
  "size": 20,
  "totalElements": 100,
  "totalPages": 5,
  "first": true,
  "last": false
}
```

## API 端点

### 数据管理

| 端点 | 方法 | 描述 |
|------|------|------|
| `/data-management/datasets` | GET | 获取数据集列表 |
| `/data-management/datasets` | POST | 创建数据集 |
| `/data-management/datasets/{id}` | GET | 获取数据集详情 |
| `/data-management/datasets/{id}` | PUT | 更新数据集 |
| `/data-management/datasets/{id}` | DELETE | 删除数据集 |
| `/data-management/datasets/{id}/files` | GET | 获取文件列表 |
| `/data-management/datasets/{id}/files/upload` | POST | 上传文件 |

详细文档：[数据管理 API](/docs/api-reference/data-management/)

### 数据清洗

| 端点 | 方法 | 描述 |
|------|------|------|
| `/data-cleaning/tasks` | GET | 获取清洗任务列表 |
| `/data-cleaning/tasks` | POST | 创建清洗任务 |
| `/data-cleaning/tasks/{id}` | GET | 获取任务详情 |
| `/data-cleaning/tasks/{id}` | PUT | 更新任务 |
| `/data-cleaning/tasks/{id}` | DELETE | 删除任务 |
| `/data-cleaning/tasks/{id}/execute` | POST | 执行任务 |

详细文档：[数据清洗 API](/docs/api-reference/data-cleaning/)

### 数据采集

| 端点 | 方法 | 描述 |
|------|------|------|
| `/data-collection/tasks` | GET | 获取采集任务列表 |
| `/data-collection/tasks` | POST | 创建采集任务 |
| `/data-collection/tasks/{id}` | GET | 获取任务详情 |
| `/data-collection/tasks/{id}/execute` | POST | 执行采集任务 |

详细文档：[数据采集 API](/docs/api-reference/data-collection/)

### 数据合成

| 端点 | 方法 | 描述 |
|------|------|------|
| `/data-synthesis/tasks` | GET | 获取合成任务列表 |
| `/data-synthesis/tasks` | POST | 创建合成任务 |
| `/data-synthesis/templates` | GET | 获取指令模板列表 |
| `/data-synthesis/templates` | POST | 创建指令模板 |

详细文档：[数据合成 API](/docs/api-reference/data-synthesis/)

### 算子市场

| 端点 | 方法 | 描述 |
|------|------|------|
| `/operator-market/operators` | GET | 获取算子列表 |
| `/operator-market/operators` | POST | 发布算子 |
| `/operator-market/operators/{id}` | GET | 获取算子详情 |
| `/operator-market/operators/{id}/install` | POST | 安装算子 |

详细文档：[算子市场 API](/docs/api-reference/operator-market/)

### RAG 索引

| 端点 | 方法 | 描述 |
|------|------|------|
| `/rag/knowledge-bases` | GET | 获取知识库列表 |
| `/rag/knowledge-bases` | POST | 创建知识库 |
| `/rag/knowledge-bases/{id}/documents` | POST | 上传文档 |
| `/rag/knowledge-bases/{id}/search` | POST | 向量检索 |

详细文档：[RAG 索引 API](/docs/api-reference/rag-indexer/)

## SDK 和客户端

DataMate 提供多种语言的 SDK：

### Python SDK

```python
from datamate import DataMateClient

# 初始化客户端
client = DataMateClient(
    base_url="http://localhost:8080",
    api_key="your-api-key"
)

# 获取数据集列表
datasets = client.data_management.get_datasets()

# 创建数据集
dataset = client.data_management.create_dataset(
    name="my_dataset",
    type="text",
    description="My dataset"
)

# 上传文件
client.data_management.upload_file(
    dataset_id=dataset.id,
    file_path="/path/to/file.txt"
)
```

### JavaScript SDK

```javascript
import { DataMateClient } from '@datamate/sdk';

// 初始化客户端
const client = new DataMateClient({
  baseURL: 'http://localhost:8080',
  apiKey: 'your-api-key'
});

// 获取数据集列表
const datasets = await client.dataManagement.getDatasets();

// 创建数据集
const dataset = await client.dataManagement.createDataset({
  name: 'my_dataset',
  type: 'text',
  description: 'My dataset'
});

// 上传文件
await client.dataManagement.uploadFile(
  dataset.id,
  '/path/to/file.txt'
);
```

## 错误码

| 错误码 | 说明 |
|--------|------|
| 200 | 成功 |
| 201 | 创建成功 |
| 400 | 请求参数错误 |
| 401 | 未认证 |
| 403 | 无权限 |
| 404 | 资源不存在 |
| 409 | 资源冲突 |
| 500 | 服务器内部错误 |

## 速率限制

API 调用速率限制：

- **默认限制**：1000 次/小时
- **突发限制**：100 次/分钟

超过限制返回 `429 Too Many Requests`。

响应头包含速率限制信息：

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1642252800
```

## 版本管理

API 版本通过 URL 路径指定：

- 当前版本：`/api/v1/`
- 未来版本：`/api/v2/`

## 相关文档

- [开发者指南](/docs/developer-guide/) - 架构和开发指南
- [OpenAPI 规范](https://github.com/ModelEngine-Group/DataMate/tree/main/backend/openapi/specs) - 完整的 OpenAPI 规范
