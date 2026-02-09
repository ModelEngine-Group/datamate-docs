---
title: User Guide
description: DataMate feature usage guides
weight: 3
---

{{% pageinfo %}}
This guide introduces how to use each feature module of DataMate.
{{% /pageinfo %}}

DataMate provides comprehensive data processing solutions for large models, covering data collection, management, cleaning, annotation, synthesis, evaluation, and the full process.

## Feature Modules

- [Data Collection](/docs/user-guide/data-collection/) - Collect data from multiple data sources
- [Data Management](/docs/user-guide/data-management/) - Manage datasets and files
- [Data Cleaning](/docs/user-guide/data-cleansing/) - Clean and preprocess data
- [Data Annotation](/docs/user-guide/data-annotation/) - Data annotation and quality control
- [Data Synthesis](/docs/user-guide/data-synthesis/) - Data augmentation based on large models
- [Data Evaluation](/docs/user-guide/data-evaluation/) - Data quality evaluation
- [Knowledge Base](/docs/user-guide/knowledge-base/) - RAG knowledge base construction
- [Operator Market](/docs/user-guide/operator-market/) - Data processing operator management
- [Pipeline Orchestration](/docs/user-guide/orchestration/) - Visual workflow orchestration
- [Agent Chat](/docs/user-guide/agent/) - AI intelligent assistant

## Typical Use Cases

### Model Fine-tuning Scenario

```
1. Data Collection → 2. Data Management → 3. Data Cleaning → 4. Data Annotation
↓
5. Data Evaluation → 6. Export Training Data
```

### RAG Application Scenario

```
1. Upload Documents → 2. Vectorization Index → 3. Knowledge Base Management
↓
4. Agent Chat (Knowledge Base Q&A)
```

### Data Augmentation Scenario

```
1. Prepare Raw Data → 2. Create Instruction Template → 3. Data Synthesis
↓
4. Quality Evaluation → 5. Export Augmented Data
```

## Quick Links

- [Quick Start](/docs/getting-started/) - Deployment and installation
- [API Reference](/docs/api-reference/) - API documentation
- [Developer Guide](/docs/developer-guide/) - Architecture and development
