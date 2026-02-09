---
title: Pipeline Orchestration
description: Visual workflow orchestration with DataMate
weight: 9
---

{{% pageinfo %}}
Pipeline orchestration module provides drag-and-drop visual interface for designing and managing complex data processing workflows.
{{% /pageinfo %}}

## Features Overview

Pipeline orchestration provides:

- **Visual Designer**: Drag-and-drop workflow design
- **Rich Node Types**: Data processing, conditions, loops, etc.
- **Flow Execution**: Auto-execute and monitor workflows
- **Template Management**: Save and reuse flow templates
- **Version Management**: Flow version control

## Node Types

### Data Nodes

| Node | Function | Config |
|------|----------|--------|
| Input Dataset | Read from dataset | Select dataset |
| Output Dataset | Write to dataset | Select dataset |
| Data Collection | Execute collection task | Select task |
| Data Cleaning | Execute cleaning task | Select task |
| Data Synthesis | Execute synthesis task | Select task |

### Logic Nodes

| Node | Function | Config |
|------|----------|--------|
| Condition Branch | Execute different branches | Condition expression |
| Loop | Repeat execution | Loop count/condition |
| Parallel | Execute multiple branches in parallel | Branch count |
| Wait | Wait for specified time | Duration |

## Quick Start

### 1. Create Pipeline

#### Step 1: Enter Pipeline Orchestration Page

Select **Pipeline Orchestration** in left navigation.

#### Step 2: Create Pipeline

Click **Create Pipeline**.

#### Step 3: Fill Basic Information

- **Pipeline name**: e.g., `data_processing_pipeline`
- **Description**: Pipeline purpose (optional)

#### Step 4: Design Flow

1. Drag nodes from left library to canvas
2. Connect nodes
3. Configure node parameters
4. Save flow

Example:
```
Input Dataset → Data Cleaning → Condition Branch
                                    ├── Satisfied → Data Annotation → Output
                                    └── Not Satisfied → Data Synthesis → Output
```

### 2. Execute Pipeline

#### Step 1: Enter Execution Page

Click pipeline name to enter details.

#### Step 2: Execute Pipeline

Click **Execute Now**.

#### Step 3: Monitor Execution

View execution status, progress, and logs.

## Advanced Features

### Flow Templates

#### Save as Template

1. Design flow
2. Click **Save as Template**
3. Enter template name

#### Use Template

1. Create pipeline, click **Use Template**
2. Select template
3. Load to designer

### Parameterized Flow

Define parameters in pipeline:

```json
{
  "parameters": [
    {
      "name": "input_dataset",
      "type": "dataset",
      "required": true
    }
  ]
}
```

### Scheduled Execution

Configure scheduled execution:
- Cron expression: `0 0 2 * * ?` (Daily at 2 AM)
- Execution parameters

## Best Practices

### 1. Flow Design

Recommended principles:
- **Modular**: Split complex flows
- **Reusable**: Use templates
- **Maintainable**: Add comments
- **Testable**: Test individually

### 2. Performance Optimization

Optimize performance:
- **Parallelize**: Use parallel nodes
- **Reduce data transfer**: Process locally
- **Batch operations**: Use batch operations
- **Cache results**: Cache intermediate results

## Common Questions

### Q: Flow execution failed?

**A**: Troubleshoot:
1. View execution logs
2. Check node configuration
3. Check data format
4. Test nodes individually

## Related Documentation

- [Data Collection](/docs/user-guide/data-collection/) - Collection nodes
- [Data Cleaning](/docs/user-guide/data-cleansing/) - Cleaning nodes
- [Operator Market](/docs/user-guide/operator-market/) - Get more operators
