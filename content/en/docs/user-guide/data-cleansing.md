---
title: Data Cleaning
description: Clean and preprocess data with DataMate
weight: 3
---

{{% pageinfo %}}
Data cleaning module provides powerful data processing capabilities to help you clean, transform, and optimize data quality.
{{% /pageinfo %}}

## Features Overview

Data cleaning module provides:

- **Built-in Cleaning Operators**: Rich pre-cleaning operator library
- **Visual Configuration**: Drag-and-drop cleaning pipeline design
- **Template Management**: Save and reuse cleaning templates
- **Batch Processing**: Support large-scale data batch cleaning
- **Real-time Preview**: Preview cleaning results

## Cleaning Operator Types

### Data Quality Operators

| Operator | Function | Applicable Data Types |
|----------|----------|----------------------|
| Deduplication | Remove duplicates | All types |
| Null Handling | Handle null values | All types |
| Outlier Detection | Detect outliers | Numerical |
| Format Validation | Validate format | All types |

### Text Cleaning Operators

| Operator | Function |
|----------|----------|
| Remove Special Chars | Remove special characters |
| Case Conversion | Convert case |
| Remove Stopwords | Remove common stopwords |
| Text Segmentation | Chinese word segmentation |
| HTML Tag Cleaning | Clean HTML tags |

## Quick Start

### 1. Create Cleaning Task

#### Step 1: Enter Data Cleaning Page

Select **Data Processing** in the left navigation.

#### Step 2: Create Task

Click **Create Task** button.

#### Step 3: Configure Basic Information

- **Task name**: e.g., `user_data_cleansing`
- **Source dataset**: Select dataset to clean
- **Output dataset**: Select or create output dataset

#### Step 4: Configure Cleaning Pipeline

1. Drag operators from left library to canvas
2. Connect operators to form pipeline
3. Configure operator parameters
4. Preview cleaning results

Example pipeline:
```
Input Data → Deduplication → Null Handling → Format Validation → Output Data
```

### 2. Use Cleaning Templates

#### Create Template

1. Configure cleaning pipeline
2. Click **Save as Template**
3. Enter template name
4. Save

#### Use Template

1. Create cleaning task
2. Click **Use Template**
3. Select template
4. Adjust as needed

### 3. Monitor Cleaning Task

View task status, progress, and statistics in task list.

## Advanced Features

### Custom Operators

Develop custom operators. See:
- [Operator Market](/docs/user-guide/operator-market/) - Operator development guide

### Conditional Branching

Add conditional branches in pipeline:
```
Input Data → [Condition Check]
              ├── Satisfied → Pipeline A
              └── Not Satisfied → Pipeline B
```

## Best Practices

### 1. Pipeline Design

Recommended principles:
- **Modular**: Split complex pipelines
- **Reusable**: Use templates and parameters
- **Maintainable**: Add comments
- **Testable**: Test individually before combining

### 2. Performance Optimization

Optimize performance:
- **Parallelize**: Use parallel nodes
- **Reduce data transfer**: Process locally when possible
- **Batch operations**: Use batch operations
- **Cache results**: Cache intermediate results

## Common Questions

### Q: Task execution failed?

**A**: Troubleshooting:
1. Check data format
2. View execution logs
3. Check operator parameters
4. Test individual operators
5. Reduce data size for testing

### Q: Cleaning speed is slow?

**A**: Optimize:
1. Reduce operator count
2. Optimize operator order
3. Increase concurrency
4. Use incremental processing

## API Reference

- [Data Cleaning API](/docs/api-reference/data-cleaning/)

## Related Documentation

- [Data Management](/docs/user-guide/data-management/) - Manage cleaned data
- [Operator Market](/docs/user-guide/operator-market/) - Get more cleaning operators
