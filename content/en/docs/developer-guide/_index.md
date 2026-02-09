---
title: Developer Guide
description: DataMate architecture and development guide
weight: 5
---

{{% pageinfo %}}
Developer guide introduces DataMate's technical architecture, development environment, and contribution process.
{{% /pageinfo %}}

DataMate is an enterprise-level data processing platform using microservices architecture, supporting large-scale data processing and custom extensions.

## Architecture Documentation

- [Backend Architecture](/docs/developer-guide/backend-architecture/) - Java microservices architecture design
- [Frontend Architecture](/docs/developer-guide/frontend-architecture/) - React frontend architecture design

## Development Guide

- [Development Setup](/docs/getting-started/development/) - Local development environment configuration
- [Contribution Guide](/docs/developer-guide/contribute/) - Code contribution process
- [Deployment Guide](/docs/developer-guide/deployment/) - Production environment deployment

## Tech Stack

### Frontend

| Technology | Version | Description |
|------------|---------|-------------|
| React | 18.x | UI framework |
| TypeScript | 5.x | Type safety |
| Ant Design | 5.x | UI component library |
| Redux Toolkit | 2.x | State management |
| Vite | 5.x | Build tool |

### Backend (Java)

| Technology | Version | Description |
|------------|---------|-------------|
| Java | 21 | Runtime environment |
| Spring Boot | 3.5.6 | Application framework |
| Spring Cloud | 2023.x | Microservices framework |
| MyBatis Plus | 3.x | ORM framework |

### Backend (Python)

| Technology | Version | Description |
|------------|---------|-------------|
| Python | 3.11+ | Runtime environment |
| FastAPI | 0.100+ | Web framework |
| LangChain | 0.1+ | LLM framework |
| Ray | 2.x | Distributed computing |

## Project Structure

```
DataMate/
├── backend/                 # Java backend
│   ├── services/           # Microservice modules
│   ├── openapi/            # OpenAPI specs
│   └── scripts/            # Build scripts
├── frontend/               # React frontend
│   ├── src/
│   │   ├── components/    # Common components
│   │   ├── pages/         # Page components
│   │   ├── services/      # API services
│   │   └── store/         # Redux store
│   └── package.json
├── runtime/                # Python runtime
│   └── datamate/          # DataMate runtime
└── deployment/             # Deployment config
    ├── docker/            # Docker config
    └── helm/              # Helm Charts
```

## Quick Start

### 1. Clone Code

```bash
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate
```

### 2. Start Services

```bash
# Start basic services
make install

# Access frontend
open http://localhost:30000
```

### 3. Development Mode

```bash
# Backend development
cd backend/services/main-application
mvn spring-boot:run

# Frontend development
cd frontend
pnpm dev

# Python service development
cd runtime/datamate
python operator_runtime.py --port 8081
```

## Core Concepts

### Microservices Architecture

DataMate uses microservices architecture, each service handles specific business functions:

- **API Gateway**: Unified entry, routing, authentication
- **Main Application**: Core business logic
- **Data Management Service**: Dataset management
- **Data Cleaning Service**: Data cleaning
- **Data Synthesis Service**: Data synthesis
- **Runtime Service**: Operator execution

### Operator System

Operators are basic units of data processing:

- **Built-in operators**: Common operators provided by platform
- **Custom operators**: User-developed custom operators
- **Operator execution**: Executed by Runtime Service

### Pipeline Orchestration

Pipelines are implemented through visual orchestration:

- **Nodes**: Basic units of data processing
- **Connections**: Data flow between nodes
- **Execution**: Automatic execution according to workflow

## Extension Development

### Develop Custom Operators

Operator development guide:

1. [Operator Market](/docs/user-guide/operator-market/) - Operator usage guide
2. Python operator development examples
3. Operator testing and debugging

### Integrate External Systems

- **API integration**: Integration via REST API
- **Webhook**: Event notifications
- **Plugin system**: (Coming soon)

## Testing

### Unit Tests

```bash
# Backend tests
cd backend
mvn test

# Frontend tests
cd frontend
pnpm test

# Python tests
cd runtime
pytest
```

### Integration Tests

```bash
# Start test environment
make test-env-up

# Run integration tests
make integration-test

# Clean test environment
make test-env-down
```

## Performance Optimization

### Backend Optimization

- Database connection pool configuration
- Query optimization
- Caching strategies
- Asynchronous processing

### Frontend Optimization

- Code splitting
- Lazy loading
- Caching strategies

## Security

### Authentication and Authorization

- JWT authentication
- RBAC permission control
- API Key authentication

### Data Security

- Transport encryption (HTTPS/TLS)
- Storage encryption
- Sensitive data masking

## Related Resources

- [GitHub Repository](https://github.com/ModelEngine-Group/DataMate)
- [Issue Tracking](https://github.com/ModelEngine-Group/DataMate/issues)
- [OpenAPI Specs](https://github.com/ModelEngine-Group/DataMate/tree/main/backend/openapi/specs)
- [API Reference](/docs/api-reference/) - Complete API documentation
