---
title: System Architecture
description: DataMate system architecture design documentation
weight: 2
---

{{% pageinfo %}}
This document details DataMate's system architecture, tech stack, and design philosophy.
{{% /pageinfo %}}

## Overall Architecture

DataMate adopts a microservices architecture, splitting the system into multiple independent services, each responsible for specific business functions. This architecture provides good scalability, maintainability, and fault tolerance.

```
┌─────────────────────────────────────────────────────────────────┐
│                           Frontend Layer                        │
│                    (React + TypeScript)                         │
│                      Ant Design + Tailwind                      │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                        API Gateway Layer                        │
│                    (Spring Cloud Gateway)                       │
│                      Port: 8080                                 │
└────────────────────────┬────────────────────────────────────────┘
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│  Java Backend│ │ Python Backend│ │  Runtime     │
│   Services   │ │    Service    │ │   Service    │
├──────────────┤ ├──────────────┤ ├──────────────┤
│· Main App    │ │· RAG Service  │ │· Operator    │
│· Data Mgmt   │ │· LangChain    │ │  Execution   │
│· Collection  │ │· FastAPI      │ │              │
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

## Tech Stack

### Frontend Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| React | 18.x | UI framework |
| TypeScript | 5.x | Type safety |
| Ant Design | 5.x | UI component library |
| Tailwind CSS | 3.x | Styling framework |
| Redux Toolkit | 2.x | State management |
| React Router | 6.x | Routing management |
| Vite | 5.x | Build tool |

### Backend Tech Stack (Java)

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 21 | Runtime environment |
| Spring Boot | 3.5.6 | Application framework |
| Spring Cloud | 2023.x | Microservices framework |
| MyBatis Plus | 3.x | ORM framework |
| PostgreSQL Driver | 42.x | Database driver |
| Redis | 5.x | Cache client |
| MinIO | 8.x | Object storage client |

### Backend Tech Stack (Python)

| Technology | Version | Purpose |
|------------|---------|---------|
| Python | 3.11+ | Runtime environment |
| FastAPI | 0.100+ | Web framework |
| LangChain | 0.1+ | LLM application framework |
| Ray | 2.x | Distributed computing |
| Pydantic | 2.x | Data validation |

### Data Storage

| Technology | Version | Purpose |
|------------|---------|---------|
| PostgreSQL | 15+ | Main database |
| Redis | 8.x | Cache and message queue |
| Milvus | 2.6.5 | Vector database |
| MinIO | RELEASE.2024+ | Object storage |

## Microservices Architecture

### Service List

| Service Name | Port | Tech Stack | Description |
|-------------|------|------------|-------------|
| API Gateway | 8080 | Spring Cloud Gateway | Unified entry, routing, auth |
| Frontend | 30000 | React | Frontend UI |
| Main Application | - | Spring Boot | Core business logic |
| Data Management Service | 8092 | Spring Boot | Dataset management |
| Data Collection Service | - | Spring Boot | Data collection tasks |
| Data Cleaning Service | - | Spring Boot | Data cleaning tasks |
| Data Annotation Service | - | Spring Boot | Data annotation tasks |
| Data Synthesis Service | - | Spring Boot | Data synthesis tasks |
| Data Evaluation Service | - | Spring Boot | Data evaluation tasks |
| Operator Market Service | - | Spring Boot | Operator marketplace |
| RAG Indexer Service | - | Spring Boot | Knowledge base indexing |
| Runtime Service | 8081 | Python + Ray | Operator execution engine |
| Backend Python Service | 18000 | FastAPI | Python backend service |
| Database | 5432 | PostgreSQL | Database |

### Service Communication

#### Synchronous Communication
- **API Gateway → Backend Services**: HTTP/REST
- **Frontend → API Gateway**: HTTP/REST
- **Backend Services ↔**: HTTP/REST (Feign Client)

#### Asynchronous Communication
- **Task Execution**: Database task queue
- **Event Notification**: Redis Pub/Sub

## Data Architecture

### Data Flow

```
┌─────────────┐
│  Data       │ Collection task config
│  Collection │ → DataX → Raw data
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Data       │ Dataset management, file upload
│  Management │ → Structured storage
└──────┬──────┘
       │
       ├──────────────┐
       ▼              ▼
┌─────────────┐  ┌─────────────┐
│  Data       │  │ Knowledge   │
│  Cleaning   │  │ Base        │
│             │  │             │
└──────┬──────┘  └──────┬──────┘
       │                │
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│  Data       │  │ Vector      │
│  Annotation │  │ Index       │
└──────┬──────┘  └──────┬──────┘
       │                │
       ▼                │
┌─────────────┐          │
│  Data       │          │
│  Synthesis  │          │
└──────┬──────┘          │
       │                │
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│  Data       │  │  RAG        │
│  Evaluation │  │ Retrieval   │
└─────────────┘  └─────────────┘
```

## Deployment Architecture

### Docker Compose Deployment

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

### Kubernetes Deployment

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

## Security Architecture

### Authentication & Authorization

#### JWT Authentication (Optional)
```yaml
datamate:
  jwt:
    enable: true  # Enable JWT authentication
    secret: your-secret-key
    expiration: 86400  # 24 hours
```

#### API Key Authentication
```yaml
datamate:
  api-key:
    enable: false
```

### Data Security

#### Transport Encryption
- API Gateway supports HTTPS/TLS
- Internal service communication can be encrypted

#### Storage Encryption
- Database: Transparent data encryption (TDE)
- MinIO: Server-side encryption
- Milvus: Encryption at rest

## Next Steps

- [Development Setup](/docs/getting-started/development/) - Local development configuration
- [Backend Architecture](/docs/developer-guide/backend-architecture/) - Backend architecture details
- [Frontend Architecture](/docs/developer-guide/frontend-architecture/) - Frontend architecture details
