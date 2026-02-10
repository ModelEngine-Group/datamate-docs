---
title: Data Annotation
description: Perform data annotation with DataMate
weight: 4
---

{{% pageinfo %}}
Data annotation module integrates Label Studio to provide professional-grade data annotation capabilities.
{{% /pageinfo %}}

## Features Overview

Data annotation module provides:

- **Multiple Annotation Types**: Image, text, audio, etc.
- **Annotation Templates**: Rich annotation templates and configurations
- **Quality Control**: Annotation review and consistency checks
- **Team Collaboration**: Multi-person collaborative annotation
- **Annotation Export**: Export annotation results

## Annotation Types

### Image Annotation

| Type | Description | Use Cases |
|------|-------------|-----------|
| Image Classification | Classify entire image | Scene recognition |
| Object Detection | Annotate object locations | Object recognition |
| Semantic Segmentation | Pixel-level classification | Medical imaging |
| Key Point Annotation | Annotate key points | Pose estimation |

### Text Annotation

| Type | Description | Use Cases |
|------|-------------|-----------|
| Text Classification | Classify text | Sentiment analysis |
| Named Entity Recognition | Annotate entities | Information extraction |
| Text Summarization | Generate summaries | Document understanding |

## Quick Start

### 1. Deploy Label Studio

```bash
make install-label-studio
```

Access: http://localhost:30001

Default credentials:
- Username: admin@demo.com
- Password: demoadmin

### 2. Create Annotation Task

#### Step 1: Enter Data Annotation Page

Select **Data** → **Data Annotation**.

#### Step 2: Create Task

Click **Create Task**.

#### Step 3: Configure Basic Information

- **Task name**: e.g., `image_classification_task`
- **Source dataset**: Select dataset to annotate
- **Annotation type**: Select type

#### Step 4: Configure Annotation Template

**Image Classification Template**:
```xml
<View>
  <Image name="image" value="$image"/>
  <Choices name="choice" toName="image">
    <Choice value="cat"/>
    <Choice value="dog"/>
    <Choice value="bird"/>
  </Choices>
</View>
```

#### Step 5: Configure Annotation Rules

- **Annotation method**: Single label / Multi label
- **Minimum annotations**: Per sample (for consistency)
- **Review mechanism**: Enable/disable review

### 3. Start Annotation

1. Enter annotation interface
2. View sample to annotate
3. Perform annotation
4. Click **Submit**
5. Auto-load next sample

## Advanced Features

### Quality Control

#### Annotation Consistency

Check consistency between annotators:
- **Cohen's Kappa**: Evaluate consistency
- **Majority vote**: Use majority annotation results
- **Expert review**: Expert reviews disputed annotations

### Pre-annotation

Use models for pre-annotation:
1. Train or use existing model
2. Pre-annotate dataset
3. Annotators correct pre-annotations

## Best Practices

### 1. Annotation Guidelines

Create clear guidelines:
- **Define standards**: Clear annotation standards
- **Provide examples**: Positive and negative examples
- **Edge cases**: Handle edge cases
- **Train annotators**: Ensure understanding

## Common Questions

### Q: Poor annotation quality?

**A**: Improve:
1. Refine guidelines
2. Strengthen training
3. Increase reviews
4. Use pre-annotation

## Related Documentation

- [Data Management](/docs/user-guide/data-management/) - Manage data to annotate
- [Data Evaluation](/docs/user-guide/data-evaluation/) - Evaluate annotation quality
