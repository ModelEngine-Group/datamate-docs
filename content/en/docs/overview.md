---
title: Overview
description: DataMate - Enterprise-level Large Model Data Processing Platform
weight: 1
---

{{% pageinfo %}}

DataMate is an enterprise-level data processing platform designed for model fine-tuning and RAG retrieval. It provides comprehensive data processing capabilities including data collection, management, cleaning, annotation, synthesis, evaluation, and knowledge base management.

{{% /pageinfo %}}

## Product Positioning

DataMate is dedicated to solving data pain points in large model implementation, providing a one-stop data governance solution:

- **Full Lifecycle Coverage**: From data collection to evaluation, covering the entire data processing lifecycle
- **Enterprise-grade Capabilities**: Supports million-scale concurrent data processing with private deployment options
- **Flexible Extension**: Rich built-in data processing operators with support for custom operator development
- **Visual Orchestration**: Drag-and-drop pipeline design without coding for complex data processing workflows

## Core Features

### Data Collection
- Heterogeneous data source collection capabilities based on DataX
- Supports relational databases, NoSQL, file systems, and other data sources
- Flexible task configuration and monitoring

### Data Management
- Unified dataset management supporting image, text, audio, video, and multimodal data types
- Complete data operations: upload, download, preview
- Tag and metadata management for easy data organization and retrieval

### Data Cleaning
- Rich built-in data cleaning operators
- Visual cleaning template configuration
- Supports both batch and stream processing modes

### Data Annotation
- Integrated Label Studio for professional annotation capabilities
- Supports image classification, object detection, text classification, and other annotation types
- Annotation review and quality control mechanisms

### Data Synthesis
- Data augmentation and synthesis capabilities based on large models
- Instruction template management and customization
- Proportional synthesis tasks for diverse data needs

### Data Evaluation
- Multi-dimensional data quality evaluation metrics
- Supports both automatic and manual evaluation
- Detailed evaluation reports

### Knowledge Base Management (RAG)
- Supports multiple document formats for knowledge base construction
- Automated text chunking and vectorization
- Integrated vector retrieval for RAG applications

### Operator Marketplace
- Rich built-in data processing operators
- Support for operator publishing and sharing
- Custom operator development capabilities

### Pipeline Orchestration
- Visual drag-and-drop workflow design
- Multiple node types and configurations
- Pipeline execution monitoring and debugging

### Agent Chat
- Integrated large language model chat capabilities
- Knowledge base Q&A
- Conversation history management

## Technical Architecture

### Overall Architecture

DataMate adopts a microservices architecture with core components including:

- **Frontend**: React 18 + TypeScript + Ant Design + Tailwind CSS
- **Backend**: Java 21 + Spring Boot 3.5.6 + Spring Cloud + MyBatis Plus
- **Runtime**: Python FastAPI + LangChain + Ray
- **Database**: PostgreSQL + Redis + Milvus + MinIO

### Microservice Components

- **API Gateway** (8080): Unified entry point for routing and authentication
- **Main Application**: Core business logic
- **Data Management Service** (8092): Dataset management
- **Data Collection Service**: Data collection task management
- **Data Cleaning Service**: Data cleaning task management
- **Data Annotation Service**: Data annotation task management
- **Data Synthesis Service**: Data synthesis task management
- **Data Evaluation Service**: Data evaluation task management
- **Operator Market Service**: Operator marketplace management
- **RAG Indexer Service**: Knowledge base indexing
- **Runtime Service** (8081): Operator execution engine
- **Backend Python Service** (18000): Python backend service

## Use Cases

### Model Fine-tuning
- Training data cleaning and quality improvement
- Data augmentation and synthesis
- Training data evaluation

### RAG Applications
- Enterprise knowledge base construction
- Document vectorization and indexing
- Semantic retrieval and Q&A

### Data Governance
- Unified management of multi-source data
- Data lineage tracking
- Data quality monitoring

## Deployment Options

DataMate supports multiple deployment methods:

- **Docker Compose**: Quick experience and development testing
- **Kubernetes/Helm**: Production environment deployment
- **Offline Deployment**: Supports air-gapped environment deployment

## Comparison with Similar Products

| Feature | DataMate | Label Studio | DocArray |
|---------|----------|--------------|----------|
| Data Management | ✅ Complete dataset management | ❌ Annotation data only | ❌ Document data only |
| Data Collection | ✅ DataX support | ❌ Not supported | ❌ Not supported |
| Data Cleaning | ✅ Rich built-in operators | ❌ Not supported | ❌ Not supported |
| Data Annotation | ✅ Label Studio integration | ✅ Professional tool | ❌ Not supported |
| Data Synthesis | ✅ LLM-based | ❌ Not supported | ❌ Not supported |
| Data Evaluation | ✅ Multi-dimensional | ⚠️ Basic | ❌ Not supported |
| Knowledge Base | ✅ RAG integration | ❌ Not supported | ⚠️ Requires development |
| Pipeline Orchestration | ✅ Visual orchestration | ❌ Not supported | ❌ Not supported |
| Operator Extension | ✅ Custom operators | ⚠️ Limited | ⚠️ Requires coding |
| License | ✅ MIT | ✅ Apache 2.0 | ✅ MIT |

## Next Steps

- [Quick Start](/docs/getting-started/) - Deploy DataMate in 5 minutes
- [User Guide](/docs/user-guide/) - Detailed feature usage documentation
- [API Reference](/docs/api-reference/) - Complete API documentation
- [Developer Guide](/docs/developer-guide/) - Architecture and development guide