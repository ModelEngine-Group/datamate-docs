---
title: Data Management API
description: Dataset and file management API
weight: 1
---

{{% pageinfo %}}
Data management API provides capabilities for dataset and file creation, query, update, and deletion.
{{% /pageinfo %}}

## Basic Information

- **Base URL**: `http://localhost:8092/api/v1/data-management`
- **Authentication**: JWT / API Key
- **Content-Type**: `application/json`

## Dataset Management

### Get Dataset List

```http
GET /data-management/datasets?page=0&size=20&type=text
```

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number, starts from 0 |
| size | integer | No | Page size, default 20 |
| type | string | No | Dataset type filter |
| tags | string | No | Tag filter, comma-separated |
| keyword | string | No | Keyword search |
| status | string | No | Status filter |

**Response Example**:
```json
{
  "content": [
    {
      "id": "dataset-001",
      "name": "text_dataset",
      "description": "Text dataset",
      "type": {
        "code": "TEXT",
        "name": "Text"
      },
      "status": "ACTIVE",
      "fileCount": 1000,
      "totalSize": 1073741824,
      "createdAt": "2024-01-15T10:00:00Z"
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 1
}
```

### Create Dataset

```http
POST /data-management/datasets
Content-Type: application/json

{
  "name": "my_dataset",
  "description": "My dataset",
  "type": "TEXT",
  "tags": ["training", "nlp"]
}
```

### Get Dataset Details

```http
GET /data-management/datasets/{datasetId}
```

### Update Dataset

```http
PUT /data-management/datasets/{datasetId}
Content-Type: application/json

{
  "name": "updated_dataset",
  "description": "Updated description"
}
```

### Delete Dataset

```http
DELETE /data-management/datasets/{datasetId}
```

## File Management

### Get File List

```http
GET /data-management/datasets/{datasetId}/files?page=0&size=20
```

### Upload File

```http
POST /data-management/datasets/{datasetId}/files/upload/chunk
Content-Type: multipart/form-data
```

### Download File

```http
GET /data-management/datasets/{datasetId}/files/{fileId}/download
```

### Delete File

```http
DELETE /data-management/datasets/{datasetId}/files/{fileId}
```

## Error Response

```json
{
  "code": 400,
  "message": "Bad Request",
  "error": "Invalid parameter: datasetId",
  "timestamp": "2024-01-15T10:30:00Z",
  "path": "/api/v1/data-management/datasets"
}
```

## SDK Usage

### Python

```python
from datamate import DataMateClient

client = DataMateClient(
    base_url="http://localhost:8080",
    api_key="your-api-key"
)

# Get datasets
datasets = client.data_management.get_datasets()

# Create dataset
dataset = client.data_management.create_dataset(
    name="my_dataset",
    type="TEXT"
)
```

### cURL

```bash
# Get datasets
curl -X GET "http://localhost:8092/api/v1/data-management/datasets" \
  -H "Authorization: Bearer your-jwt-token"

# Create dataset
curl -X POST "http://localhost:8092/api/v1/data-management/datasets" \
  -H "Authorization: Bearer your-jwt-token" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my_dataset",
    "type": "TEXT"
  }'
```

## Related Documentation

- [Data Management](/docs/user-guide/data-management/) - User guide
- [OpenAPI Specs](https://github.com/ModelEngine-Group/DataMate/blob/main/backend/openapi/specs/data-management.yaml) - Complete specs
