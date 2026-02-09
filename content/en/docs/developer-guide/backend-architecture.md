---
title: Backend Architecture
description: DataMate Java backend architecture design
weight: 1
---

{{% pageinfo %}}
DataMate backend adopts microservices architecture built on Spring Boot 3.x and Spring Cloud.
{{% /pageinfo %}}

## Architecture Overview

DataMate backend uses microservices architecture, splitting into multiple independent services:

```
┌─────────────────────────────────────────────┐
│              API Gateway                    │
│         (Spring Cloud Gateway)              │
│              Port: 8080                     │
└──────────────┬──────────────────────────────┘
               │
       ┌───────┴───────┬───────────────┐
       ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│   Main       │ │  Data        │ │  Data        │
│ Application  │ │  Management  │ │  Collection  │
└──────────────┘ └──────────────┘ └──────────────┘
       │               │               │
       └───────────────┴───────────────┘
                       │
                       ▼
              ┌────────────────┐
              │   PostgreSQL   │
              │   Port: 5432   │
              └────────────────┘
```

## Tech Stack

### Core Frameworks

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 21 | Programming language |
| Spring Boot | 3.5.6 | Application framework |
| Spring Cloud | 2023.x | Microservices framework |
| MyBatis Plus | 3.5.x | ORM framework |

### Support Components

| Technology | Version | Purpose |
|------------|---------|---------|
| Redis | 5.x | Cache and message queue |
| MinIO | 8.x | Object storage |
| Milvus SDK | 2.3.x | Vector database |

## Microservices List

### API Gateway

**Port**: 8080

**Functions**:
- Unified entry point
- Route forwarding
- Authentication and authorization
- Rate limiting and circuit breaking

**Tech**: Spring Cloud Gateway, JWT authentication

### Main Application

**Functions**:
- User management
- Permission management
- System configuration
- Task scheduling

### Data Management Service

**Port**: 8092

**Functions**:
- Dataset management
- File management
- Tag management
- Statistics

**API Endpoints**:
- `/data-management/datasets` - Dataset management
- `/data-management/datasets/{id}/files` - File management

### Runtime Service

**Port**: 8081

**Functions**:
- Operator execution
- Ray integration
- Task scheduling

**Tech**: Python + Ray, FastAPI

## Database Design

### Main Tables

#### users (User Table)

| Field | Type | Description |
|-------|------|-------------|
| id | BIGINT | Primary key |
| username | VARCHAR(50) | Username |
| password | VARCHAR(255) | Password (encrypted) |
| email | VARCHAR(100) | Email |
| role | VARCHAR(20) | Role |
| created_at | TIMESTAMP | Creation time |

#### datasets (Dataset Table)

| Field | Type | Description |
|-------|------|-------------|
| id | VARCHAR(50) | Primary key |
| name | VARCHAR(100) | Name |
| description | TEXT | Description |
| type | VARCHAR(20) | Type |
| status | VARCHAR(20) | Status |
| created_by | VARCHAR(50) | Creator |

## Service Communication

### Synchronous Communication

Services communicate via HTTP/REST:

```java
// Using Feign Client
@FeignClient(name = "data-management-service")
public interface DataManagementClient {
    @GetMapping("/data-management/datasets/{id}")
    DatasetResponse getDataset(@PathVariable String id);
}
```

### Asynchronous Communication

Using Redis for async messaging:

```java
// Send message
redisTemplate.convertAndSend("task.created", taskMessage);

// Receive message
@RedisListener(topic = "task.created")
public void handleTaskCreated(TaskMessage message) {
    // Handle task creation event
}
```

## Authentication & Authorization

### JWT Authentication

```java
@Configuration
public class JwtConfig {
    @Value("${datamate.jwt.secret}")
    private String secret;

    @Value("${datamate.jwt.expiration}")
    private Long expiration;
}
```

### RBAC

```java
@PreAuthorize("hasRole('ADMIN')")
public void adminOperation() {
    // Admin operations
}
```

## Performance Optimization

### Database Connection Pool

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
```

### Caching Strategy

```java
@Cacheable(value = "datasets", key = "#id")
public Dataset getDataset(String id) {
    return datasetRepository.findById(id);
}
```

## Related Documentation

- [Frontend Architecture](/docs/developer-guide/frontend-architecture/) - Frontend architecture
- [Database Schema](/docs/developer-guide/database-schema/) - Database tables
- [Development Setup](/docs/getting-started/development/) - Dev environment
