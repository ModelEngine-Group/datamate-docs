---
title: Data Evaluation
description: Evaluate data quality with DataMate
weight: 6
---

{{% pageinfo %}}
Data evaluation module provides multi-dimensional data quality evaluation capabilities.
{{% /pageinfo %}}

## Features Overview

Data evaluation module provides:

- **Quality Metrics**: Rich data quality evaluation metrics
- **Automatic Evaluation**: Auto-execute evaluation tasks
- **Manual Evaluation**: Manual sampling evaluation
- **Evaluation Reports**: Generate detailed reports
- **Quality Tracking**: Track data quality trends

## Evaluation Dimensions

### Data Completeness

| Metric | Description | Calculation |
|--------|-------------|-------------|
| Null Rate | Null value ratio | Null count / Total count |
| Missing Field Rate | Required field missing rate | Missing fields / Total fields |
| Record Complete Rate | Complete record ratio | Complete records / Total records |

### Data Accuracy

| Metric | Description | Calculation |
|--------|-------------|-------------|
| Format Correct Rate | Format compliance | Format correct / Total |
| Value Range Compliance | In valid range | In range / Total |
| Consistency Rate | Data consistency | Consistent records / Total |

## Quick Start

### 1. Create Evaluation Task

#### Step 1: Enter Data Evaluation Page

Select **Data Evaluation** in the left navigation.

#### Step 2: Create Task

Click **Create Task**.

#### Step 3: Configure Basic Information

- **Task name**: e.g., `data_quality_evaluation`
- **Evaluation dataset**: Select dataset to evaluate

#### Step 4: Configure Evaluation Dimensions

Select dimensions:
- ✅ Data completeness
- ✅ Data accuracy
- ✅ Data uniqueness
- ✅ Data timeliness

#### Step 5: Configure Evaluation Rules

**Completeness Rules**:
```
Required fields: name, email, phone
Null threshold: 5% (warn if exceeded)
```

### 2. Execute Evaluation

#### Automatic Evaluation

Auto-executes after creation, or click **Execute Now**.

#### Manual Evaluation

1. Click **Manual Evaluation** tab
2. View samples to evaluate
3. Manually evaluate quality
4. Submit results

### 3. View Evaluation Report

#### Overall Score

```
Overall Quality Score: 85 (Excellent)

Completeness: 90 ⭐⭐⭐⭐⭐
Accuracy: 82 ⭐⭐⭐⭐
Uniqueness: 95 ⭐⭐⭐⭐⭐
Timeliness: 75 ⭐⭐⭐⭐
```

#### Detailed Metrics

**Completeness**:
- Null rate: 3.2% ✅
- Missing field rate: 1.5% ✅
- Record complete rate: 96.8% ✅

## Advanced Features

### Custom Evaluation Rules

#### Regex Validation

```
Field: phone
Rule: ^1[3-9]\d{9}$
Description: China mobile phone number
```

#### Value Range Validation

```
Field: age
Min value: 0
Max value: 120
```

### Comparison Evaluation

Compare different datasets or versions.

## Best Practices

### 1. Regular Evaluation

Recommended schedule:
- **Daily**: Critical data
- **Weekly**: General data
- **Monthly**: All data

### 2. Establish Baseline

Create quality baseline for each dataset.

### 3. Continuous Improvement

Based on evaluation results:
- **Clean problem data**
- **Optimize collection process**
- **Update validation rules**

## Common Questions

### Q: Evaluation task failed?

**A**: Troubleshoot:
1. Check dataset exists
2. Check rule configuration
3. View execution logs
4. Test with small sample size

## API Reference

- [Data Evaluation API](/docs/api-reference/data-evaluation/)

## Related Documentation

- [Data Cleaning](/docs/user-guide/data-cleansing/) - Clean data
- [Data Management](/docs/user-guide/data-management/) - Manage data
