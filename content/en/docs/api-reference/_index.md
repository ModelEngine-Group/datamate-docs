---
title: API Reference
description: DataMate API documentation
weight: 4
---

{{% pageinfo %}}
DataMate provides complete REST APIs supporting programmatic access to all core features.
{{% /pageinfo %}}

## API Overview

DataMate API is based on REST architecture design, providing the following services:

- **Data Management API**: Dataset and file management
- **Data Cleaning API**: Data cleaning task management
- **Data Collection API**: Data collection task management
- **Data Annotation API**: Data annotation task management
- **Data Synthesis API**: Data synthesis task management
- **Data Evaluation API**: Data evaluation task management
- **Operator Market API**: Operator management
- **RAG Indexer API**: Knowledge base and vector retrieval
- **Pipeline Orchestration API**: Pipeline orchestration management

## Authentication

DataMate supports two authentication methods:

### JWT Authentication (Recommended)

```http
GET /api/v1/data-management/datasets
Authorization: Bearer <your-jwt-token>
```

Get JWT Token:

```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "username": "admin",
  "password": "password"
}
```

Response:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 86400
}
```

### API Key Authentication

```http
GET /api/v1/data-management/datasets
X-API-Key: <your-api-key>
```

## Common Response Format

### Success Response

```json
{
  "code": 200,
  "message": "success",
  "data": {
    // Response data
  }
}
```

### Error Response

```json
{
  "code": 400,
  "message": "Bad Request",
  "error": "Invalid parameter: datasetId",
  "timestamp": "2024-01-15T10:30:00Z",
  "path": "/api/v1/data-management/datasets"
}
```

### Paged Response

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

## API Endpoints

### Data Management

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/data-management/datasets` | GET | Get dataset list |
| `/data-management/datasets` | POST | Create dataset |
| `/data-management/datasets/{id}` | GET | Get dataset details |
| `/data-management/datasets/{id}` | PUT | Update dataset |
| `/data-management/datasets/{id}` | DELETE | Delete dataset |
| `/data-management/datasets/{id}/files` | GET | Get file list |
| `/data-management/datasets/{id}/files/upload` | POST | Upload files |

### Data Cleaning

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/data-cleaning/tasks` | GET | Get cleaning task list |
| `/data-cleaning/tasks` | POST | Create cleaning task |
| `/data-cleaning/tasks/{id}` | GET | Get task details |
| `/data-cleaning/tasks/{id}` | PUT | Update task |
| `/data-cleaning/tasks/{id}` | DELETE | Delete task |
| `/data-cleaning/tasks/{id}/execute` | POST | Execute task |

### Data Collection

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/data-collection/tasks` | GET | Get collection task list |
| `/data-collection/tasks` | POST | Create collection task |
| `/data-collection/tasks/{id}` | GET | Get task details |
| `/data-collection/tasks/{id}/execute` | POST | Execute collection task |

### Data Synthesis

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/data-synthesis/tasks` | GET | Get synthesis task list |
| `/data-synthesis/tasks` | POST | Create synthesis task |
| `/data-synthesis/templates` | GET | Get instruction template list |
| `/data-synthesis/templates` | POST | Create instruction template |

### Operator Market

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/operator-market/operators` | GET | Get operator list |
| `/operator-market/operators` | POST | Publish operator |
| `/operator-market/operators/{id}` | GET | Get operator details |
| `/operator-market/operators/{id}/install` | POST | Install operator |

### RAG Indexer

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/rag/knowledge-bases` | GET | Get knowledge base list |
| `/rag/knowledge-bases` | POST | Create knowledge base |
| `/rag/knowledge-bases/{id}/documents` | POST | Upload documents |
| `/rag/knowledge-bases/{id}/search` | POST | Vector search |

## Error Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 500 | Internal Server Error |

## Rate Limiting

API call rate limits:

- **Default limit**: 1000 requests/hour
- **Burst limit**: 100 requests/minute

Exceeding the limit returns `429 Too Many Requests`.

Response headers contain rate limiting information:

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1642252800
```

## Version Management

API versions are specified through URL paths:

- Current version: `/api/v1/`
- Future versions: `/api/v2/`

## Related Documentation

- [Developer Guide](/docs/developer-guide/) - Architecture and development guide
- [OpenAPI Specifications](https://github.com/ModelEngine-Group/DataMate/tree/main/backend/openapi/specs) - Complete OpenAPI specs
