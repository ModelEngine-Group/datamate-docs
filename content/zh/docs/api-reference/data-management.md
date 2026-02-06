---
title: 数据管理 API
description: 数据集和文件管理 API
weight: 1
---

{{% pageinfo %}}
数据管理 API 提供数据集和文件的创建、查询、更新和删除功能。
{{% /pageinfo %}}

## 基础信息

- **Base URL**: `http://localhost:8092/api/v1/data-management`
- **认证方式**: JWT / API Key
- **Content-Type**: `application/json`

## 数据集管理

### 获取数据集列表

```http
GET /data-management/datasets?page=0&size=20&type=text&status=ACTIVE
```

**查询参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| page | integer | 否 | 页码，从 0 开始，默认 0 |
| size | integer | 否 | 每页大小，默认 20 |
| type | string | 否 | 数据集类型过滤 |
| tags | string | 否 | 标签过滤，逗号分隔 |
| keyword | string | 否 | 关键词搜索 |
| status | string | 否 | 状态过滤 |

**响应示例**:

```json
{
  "content": [
    {
      "id": "dataset-001",
      "name": "text_dataset",
      "description": "文本数据集",
      "type": {
        "code": "TEXT",
        "name": "文本",
        "supportedFormats": ["txt", "md", "json"]
      },
      "status": "ACTIVE",
      "tags": [
        {"id": "tag-001", "name": "training", "color": "#FF0000"}
      ],
      "fileCount": 1000,
      "totalSize": 1073741824,
      "completionRate": 85.5,
      "createdAt": "2024-01-15T10:00:00Z",
      "createdBy": "admin"
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 1,
  "totalPages": 1,
  "first": true,
  "last": true
}
```

### 创建数据集

```http
POST /data-management/datasets
Content-Type: application/json

{
  "name": "my_dataset",
  "description": "My dataset description",
  "type": "TEXT",
  "tags": ["training", "nlp"],
  "dataSource": "/data/input",
  "targetLocation": "/data/output"
}
```

**请求体参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| name | string | 是 | 数据集名称，1-100 字符 |
| description | string | 否 | 数据集描述，最多 500 字符 |
| type | string | 是 | 数据集类型代码 |
| tags | array | 否 | 标签列表 |
| dataSource | string | 否 | 数据源 |
| targetLocation | string | 否 | 目标位置 |

**响应示例**:

```json
{
  "id": "dataset-002",
  "name": "my_dataset",
  "description": "My dataset description",
  "type": {
    "code": "TEXT",
    "name": "文本"
  },
  "status": "ACTIVE",
  "tags": [],
  "fileCount": 0,
  "totalSize": 0,
  "completionRate": 0,
  "createdAt": "2024-01-15T11:00:00Z",
  "createdBy": "admin"
}
```

### 获取数据集详情

```http
GET /data-management/datasets/{datasetId}
```

**路径参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| datasetId | string | 是 | 数据集 ID |

**响应**: 同创建数据集响应

### 更新数据集

```http
PUT /data-management/datasets/{datasetId}
Content-Type: application/json

{
  "name": "updated_dataset",
  "description": "Updated description",
  "tags": ["training", "updated"]
}
```

**请求体参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| name | string | 否 | 数据集名称 |
| description | string | 否 | 数据集描述 |
| tags | array | 否 | 标签列表 |
| status | string | 否 | 数据集状态 |

### 删除数据集

```http
DELETE /data-management/datasets/{datasetId}
```

**响应**: 204 No Content

## 文件管理

### 获取文件列表

```http
GET /data-management/datasets/{datasetId}/files?page=0&size=20
```

**查询参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| page | integer | 否 | 页码，从 0 开始 |
| size | integer | 否 | 每页大小 |
| fileType | string | 否 | 文件类型过滤 |
| status | string | 否 | 状态过滤 |

**响应示例**:

```json
{
  "content": [
    {
      "id": "file-001",
      "fileName": "document.txt",
      "originalName": "document.txt",
      "fileType": "txt",
      "fileSize": 1024,
      "status": "COMPLETED",
      "filePath": "/dataset-001/document.txt",
      "uploadTime": "2024-01-15T10:30:00Z",
      "uploadedBy": "admin"
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 1,
  "totalPages": 1
}
```

### 上传文件（预上传）

```http
POST /data-management/datasets/{datasetId}/files/upload/pre-upload
Content-Type: application/json

{
  "hasArchive": false,
  "totalFileNum": 1,
  "totalSize": 1024,
  "prefix": "documents/"
}
```

**请求体参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| hasArchive | boolean | 否 | 是否为压缩包 |
| totalFileNum | integer | 是 | 总文件数 |
| totalSize | integer | 是 | 总文件大小（字节） |
| prefix | string | 否 | 目标子目录前缀 |

**响应**: 请求 ID (string)

### 上传文件（分片上传）

```http
POST /data-management/datasets/{datasetId}/files/upload/chunk
Content-Type: multipart/form-data

reqId: <pre-upload-request-id>
fileNo: 1
fileName: document.txt
totalChunkNum: 3
chunkNo: 1
file: <binary-data>
checkSumHex: <checksum>
```

**表单参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| reqId | string | 是 | 预上传请求 ID |
| fileNo | integer | 是 | 文件编号 |
| fileName | string | 是 | 文件名 |
| totalChunkNum | integer | 是 | 总分片数 |
| chunkNo | integer | 是 | 当前分片编号（从 1 开始） |
| file | file | 是 | 分片二进制数据 |
| checkSumHex | string | 否 | 分片校验和 |

### 下载文件

```http
GET /data-management/datasets/{datasetId}/files/{fileId}/download
```

**响应**: 文件二进制内容

### 批量下载文件（ZIP）

```http
GET /data-management/datasets/{datasetId}/files/download
```

**响应**: ZIP 压缩包

### 删除文件

```http
DELETE /data-management/datasets/{datasetId}/files/{fileId}
```

**响应**: 204 No Content

### 创建目录

```http
POST /data-management/datasets/{datasetId}/files/directories
Content-Type: application/json

{
  "parentPrefix": "documents/",
  "directoryName": "subfolder"
}
```

## 数据集类型

### 获取数据集类型列表

```http
GET /data-management/dataset-types
```

**响应示例**:

```json
[
  {
    "code": "TEXT",
    "name": "文本",
    "description": "文本数据集",
    "supportedFormats": ["txt", "md", "json", "csv"],
    "icon": "file-text"
  },
  {
    "code": "IMAGE",
    "name": "图像",
    "description": "图像数据集",
    "supportedFormats": ["jpg", "png", "gif", "bmp"],
    "icon": "file-image"
  }
]
```

## 标签管理

### 获取标签列表

```http
GET /data-management/tags?keyword=training
```

**查询参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| keyword | string | 否 | 标签名称关键词 |

**响应示例**:

```json
[
  {
    "id": "tag-001",
    "name": "training",
    "color": "#FF0000",
    "description": "训练数据",
    "usageCount": 15
  }
]
```

### 创建标签

```http
POST /data-management/tags
Content-Type: application/json

{
  "name": "validation",
  "color": "#00FF00",
  "description": "验证数据"
}
```

**请求体参数**:

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| name | string | 是 | 标签名称，1-50 字符 |
| color | string | 否 | 标签颜色，十六进制 |
| description | string | 否 | 标签描述，最多 200 字符 |

## 统计信息

### 获取数据集统计

```http
GET /data-management/datasets/{datasetId}/statistics
```

**响应示例**:

```json
{
  "totalFiles": 1000,
  "completedFiles": 850,
  "totalSize": 1073741824,
  "completionRate": 85.0,
  "fileTypeDistribution": {
    "txt": 600,
    "md": 250,
    "json": 150
  },
  "statusDistribution": {
    "COMPLETED": 850,
    "PROCESSING": 100,
    "ERROR": 50
  }
}
```

## 错误响应

### 400 Bad Request

```json
{
  "error": "INVALID_PARAMETER",
  "message": "Invalid parameter: datasetId",
  "timestamp": "2024-01-15T10:30:00Z",
  "path": "/api/v1/data-management/datasets"
}
```

### 404 Not Found

```json
{
  "error": "DATASET_NOT_FOUND",
  "message": "Dataset not found: dataset-001",
  "timestamp": "2024-01-15T10:30:00Z",
  "path": "/api/v1/data-management/datasets/dataset-001"
}
```

## SDK 使用示例

### Python

```python
from datamate import DataMateClient

client = DataMateClient(
    base_url="http://localhost:8080",
    api_key="your-api-key"
)

# 获取数据集列表
datasets = client.data_management.get_datasets(
    type="TEXT",
    status="ACTIVE"
)

# 创建数据集
dataset = client.data_management.create_dataset(
    name="my_dataset",
    type="TEXT",
    description="My dataset"
)

# 上传文件
with open("document.txt", "rb") as f:
    client.data_management.upload_file(
        dataset_id=dataset.id,
        file=f,
        file_name="document.txt"
    )
```

### JavaScript

```javascript
import { DataMateClient } from '@datamate/sdk';

const client = new DataMateClient({
  baseURL: 'http://localhost:8080',
  apiKey: 'your-api-key'
});

// 获取数据集列表
const datasets = await client.dataManagement.getDatasets({
  type: 'TEXT',
  status: 'ACTIVE'
});

// 创建数据集
const dataset = await client.dataManagement.createDataset({
  name: 'my_dataset',
  type: 'TEXT',
  description: 'My dataset'
});

// 上传文件
await client.dataManagement.uploadFile(
  dataset.id,
  'document.txt',
  fileData
);
```

### cURL

```bash
# 获取数据集列表
curl -X GET "http://localhost:8092/api/v1/data-management/datasets?page=0&size=20" \
  -H "Authorization: Bearer your-jwt-token"

# 创建数据集
curl -X POST "http://localhost:8092/api/v1/data-management/datasets" \
  -H "Authorization: Bearer your-jwt-token" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my_dataset",
    "type": "TEXT",
    "description": "My dataset"
  }'
```

## 相关文档

- [数据管理](/docs/user-guide/data-management/) - 用户指南
- [OpenAPI 规范](https://github.com/ModelEngine-Group/DataMate/blob/main/backend/openapi/specs/data-management.yaml) - 完整规范
