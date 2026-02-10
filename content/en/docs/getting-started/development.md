---
title: Development Environment Setup
description: Local development environment configuration guide for DataMate
weight: 3
---

{{% pageinfo %}}
This document describes how to set up a local development environment for DataMate.
{{% /pageinfo %}}

## Prerequisites

### Required Software

| Software | Version | Purpose |
|----------|---------|---------|
| Node.js | 18.x+ | Frontend development |
| pnpm | 8.x+ | Frontend package management |
| Java | 21 | Backend development |
| Maven | 3.9+ | Backend build |
| Python | 3.11+ | Python service development |
| Docker | 20.10+ | Containerized deployment |
| Docker Compose | 2.0+ | Service orchestration |
| Git | 2.x+ | Version control |
| Make | 4.x+ | Build automation |

### Recommended Software

- **IDE**: IntelliJ IDEA (backend) + VS Code (frontend/Python)
- **Database Client**: DBeaver, pgAdmin
- **API Testing**: Postman, curl
- **Git Client**: GitKraken, SourceTree

## Code Structure

```
DataMate/
├── backend/                 # Java backend
│   ├── services/           # Microservice modules
│   │   ├── main-application/
│   │   ├── data-management-service/
│   │   ├── data-cleaning-service/
│   │   └── ...
│   ├── openapi/            # OpenAPI specs
│   └── scripts/            # Build scripts
├── frontend/               # React frontend
│   ├── src/
│   │   ├── components/    # Common components
│   │   ├── pages/         # Page components
│   │   ├── services/      # API services
│   │   ├── store/         # Redux store
│   │   └── routes/        # Routes config
│   └── package.json
├── runtime/                # Python runtime
│   └── datamate/          # DataMate runtime
└── deployment/             # Deployment configs
    ├── docker/            # Docker configs
    └── helm/              # Helm charts
```

## Backend Development

### 1. Install Java 21

```bash
# macOS (Homebrew)
brew install openjdk@21

# Linux (Ubuntu/Debian)
sudo apt update
sudo apt install openjdk-21-jdk

# Verify
java -version
```

### 2. Install Maven

```bash
# macOS
brew install maven

# Linux
sudo apt install maven

# Verify
mvn -version
```

### 3. Configure IDE (IntelliJ IDEA)

#### Install Plugins
- Lombok Plugin
- MyBatis Plugin
- Rainbow Brackets
- GitToolBox

#### Import Project
1. Open IntelliJ IDEA
2. File → Open
3. Select `backend` directory
4. Wait for Maven dependency download

### 4. Configure Database

#### Start Local Database (Docker)

```bash
# Start database only
docker compose -f deployment/docker/datamate/docker-compose.yml up -d datamate-database
```

Connection info:
- Host: localhost
- Port: 5432
- Database: datamate
- Username: postgres
- Password: password

### 5. Run Backend Service

#### Using Maven

```bash
cd backend/services/main-application
mvn spring-boot:run
```

#### Using IDE

1. Find Application class
2. Right-click → Run
3. Access http://localhost:8080

## Frontend Development

### 1. Install Node.js

```bash
# macOS
brew install node@18

# Linux
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 2. Install pnpm

```bash
npm install -g pnpm
```

### 3. Install Dependencies

```bash
cd frontend
pnpm install
```

### 4. Configure Dev Environment

Create `.env.development`:

```bash
VITE_API_BASE_URL=http://localhost:8080
VITE_API_TIMEOUT=30000
```

### 5. Start Dev Server

```bash
pnpm dev
```

Access http://localhost:3000

## Python Service Development

### 1. Install Python 3.11

```bash
# macOS
brew install python@3.11

# Linux
sudo apt install python3.11 python3.11-venv
```

### 2. Create Virtual Environment

```bash
cd runtime/datamate
python3.11 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Python Service

```bash
python operator_runtime.py --port 8081
```

## Local Debugging

### Start All Services

#### Using Docker Compose

```bash
# Start base services (database, Redis, etc.)
docker compose -f deployment/docker/datamate/docker-compose.yml up -d \
  datamate-database \
  datamate-redis

# Start Milvus (optional)
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus up -d
```

#### Start Backend Services

```bash
# Terminal 1: Main Application
cd backend/services/main-application
mvn spring-boot:run

# Terminal 2: Data Management Service
cd backend/services/data-management-service
mvn spring-boot:run
```

#### Start Frontend

```bash
cd frontend
pnpm dev
```

#### Start Python Services

```bash
# Runtime Service
cd runtime/datamate
python operator_runtime.py --port 8081

# Backend Python Service
cd backend-python
uvicorn main:app --reload --port 18000
```

## Code Standards

### Java Code Standards

#### Naming Conventions
- Class name: PascalCase `UserService`
- Method name: camelCase `getUserById`
- Constants: UPPER_CASE `MAX_SIZE`
- Variables: camelCase `userName`

### TypeScript Code Standards

#### Naming Conventions
- Components: PascalCase `UserProfile`
- Types/Interfaces: PascalCase `UserData`
- Functions: camelCase `getUserData`
- Constants: UPPER_CASE `API_BASE_URL`

### Python Code Standards

Follow PEP 8:

```python
def get_user(user_id: int) -> dict:
    """Get user information

    Args:
        user_id: User ID

    Returns:
        User information dictionary
    """
    # ...
```

## Common Issues

### Backend Won't Start

1. Check Java version: `java -version`
2. Check port conflicts: `lsof -i :8080`
3. View logs
4. Clean and rebuild: `mvn clean install`

### Frontend Won't Start

1. Check Node version: `node -v`
2. Delete node_modules: `rm -rf node_modules && pnpm install`
3. Check port: `lsof -i :3000`

## Next Steps

- [Backend Architecture](/docs/developer-guide/backend-architecture/) - Backend architecture details
- [Frontend Architecture](/docs/developer-guide/frontend-architecture/) - Frontend architecture details
- [Contribution Guide](/docs/developer-guide/contribute/) - How to contribute
