---
title: Operator Market
description: Manage and use DataMate operators
weight: 8
---

{{% pageinfo %}}
Operator marketplace provides rich data processing operators and supports custom operator development.
{{% /pageinfo %}}

## Features Overview

Operator marketplace provides:

- **Built-in Operators**: Rich built-in data processing operators
- **Operator Publishing**: Publish and share custom operators
- **Operator Installation**: Install third-party operators
- **Custom Development**: Develop custom operators

## Built-in Operators

### Data Cleaning Operators

| Operator | Function | Input | Output |
|----------|----------|-------|--------|
| Deduplication | Remove duplicates | Dataset | Deduplicated data |
| Null Handler | Handle nulls | Dataset | Filled data |
| Format Converter | Convert format | Original format | New format |

### Text Processing Operators

| Operator | Function |
|----------|----------|
| Text Segmentation | Chinese word segmentation |
| Remove Stopwords | Remove common stopwords |
| Text Cleaning | Clean special characters |

## Quick Start

### 1. Browse Operators

#### Step 1: Enter Operator Market

Select **Data** → **Operator Market**.

#### Step 2: Browse Operators

View all available operators with ratings and installation counts.

### 2. Install Operator

#### Install Built-in Operator

Built-in operators are installed by default.

#### Install Third-party Operator

1. In operator details page, click **Install**
2. Wait for installation completion

### 3. Use Operator

After installation, use in:
- **Data Cleaning**: Add operator node to cleaning pipeline
- **Pipeline Orchestration**: Add operator node to workflow

## Advanced Features

### Develop Custom Operator

#### Create Operator

1. In operator market page, click **Create Operator**
2. Fill operator information
3. Write operator code (Python)
4. Package and publish

**Python Operator Example**:
```python
class MyTextCleaner:
    def __init__(self, config):
        self.remove_special_chars = config.get('remove_special_chars', True)

    def process(self, data):
        if isinstance(data, str):
            result = data
            if self.remove_special_chars:
                import re
                result = re.sub(r'[^\w\s]', '', result)
            return result
        return data
```

## Best Practices

### 1. Operator Design

Good operator design:
- **Single responsibility**: One operator does one thing
- **Configurable**: Rich configuration options
- **Error handling**: Comprehensive error handling
- **Performance**: Consider large-scale data

## Common Questions

### Q: Operator execution failed?

**A**: Troubleshoot:
1. View logs
2. Check configuration
3. Check data format
4. Test locally

## Related Documentation

- [Data Cleaning](/docs/user-guide/data-cleansing/) - Use operators for cleaning
- [Pipeline Orchestration](/docs/user-guide/orchestration/) - Use operators in pipelines
