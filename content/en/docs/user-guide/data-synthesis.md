---
title: Data Synthesis
description: Use large models for data augmentation and synthesis
weight: 5
---

{{% pageinfo %}}
Data synthesis module leverages large model capabilities to automatically generate high-quality training data, reducing data collection costs.
{{% /pageinfo %}}

## Features Overview

Data synthesis module provides:

- **Instruction template management**: Create and manage synthesis instruction templates
- **Single task synthesis**: Create individual synthesis tasks
- **Proportional synthesis task**: Synthesize multi-category balanced data by specified ratios
- **Large model integration**: Support for multiple LLM APIs
- **Quality evaluation**: Automatic evaluation of synthesized data quality

## Quick Start

### 1. Create Instruction Template

#### Step 1: Enter Data Synthesis Page

In left navigation, select **Data** → **Data Synthesis** → **Tasks**.

#### Step 2: Create Instruction Template

1. Click **Instruction Templates** tab
2. Click **Create Template** button

#### Step 3: Configure Template

**Basic Information**:
- **Template name**: e.g., `qa_generation_template`
- **Template description**: Describe template purpose (optional)
- **Template type**: Select template type (Q&A, dialogue, summary, etc.)

**Prompt Configuration**:

Example prompt:
```
You are a professional data generation assistant. Generate data based on the following requirements:

Task: Generate Q&A pairs
Topic: {topic}
Count: {count}
Difficulty: {difficulty}

Requirements:
1. Questions should be clear and specific
2. Answers should be accurate and complete
3. Cover different difficulty levels

Output format: JSON
[
  {
    "question": "...",
    "answer": "..."
  }
]
```

**Parameter Configuration**:
- **Model**: Select LLM to use (GPT-4, Claude, local model, etc.)
- **Temperature**: Control generation randomness (0-1)
- **Max tokens**: Limit generation length
- **Other parameters**: Configure according to model

#### Step 4: Save Template

Click **Save** button to save template.

### 2. Create Synthesis Task

#### Step 1: Create Task

1. Return to **Data Synthesis** page
2. Click **Create Task** button

#### Step 2: Configure Basic Information

- **Task name**: e.g., `medical_qa_synthesis`
- **Task description**: Describe task purpose (optional)
- **Select template**: Select created instruction template

#### Step 3: Configure Parameters

Configure according to template parameters:

| Parameter | Value | Description |
|-----------|-------|-------------|
| topic | Medical health | Synthesis topic |
| count | 100 | Generation count |
| difficulty | Medium | Difficulty level |

#### Step 4: Configure Output

- **Output dataset**: Select or create output dataset
- **Output format**: Select output format (JSON, CSV, JSONL, etc.)

#### Step 5: Create and Execute

Click **Create** button. Task will automatically start executing.

### 3. Monitor Synthesis Task

#### View Task List

In data synthesis page, you can see all synthesis tasks:

| Task Name | Template | Status | Progress | Generated Count | Actions |
|-----------|----------|--------|----------|-----------------|---------|
| Medical QA Synthesis | qa_template | Running | 50% | 50/100 | View Details |
| Sentiment Data Synthesis | sentiment_template | Completed | 100% | 1000/1000 | View Details |

## Advanced Features

### Template Variables

Use variables in prompts for dynamic configuration:

**Variable syntax**: `{variable_name}`

**Example**:
```
Generate {count} {difficulty} level {type} about {topic}.
```

**Built-in variables**:
- `{current_date}`: Current date
- `{current_time}`: Current time
- `{random_id}`: Random ID

### Model Selection

DataMate supports multiple LLMs:

| Model | Type | Description |
|-------|------|-------------|
| GPT-4 | OpenAI | High-quality generation |
| GPT-3.5-Turbo | OpenAI | Fast generation |
| Claude 3 | Anthropic | Long-text generation |
| Wenxin Yiyan | Baidu | Chinese optimized |
| Tongyi Qianwen | Alibaba | Chinese optimized |
| Local Model | Deployed locally | Private deployment |

## Best Practices

### 1. Prompt Design

Good prompts should:

- **Define task clearly**: Clearly describe generation task
- **Specify format**: Clearly define output format requirements
- **Provide examples**: Give expected output examples
- **Control quality**: Set quality requirements

**Example prompt**:
```
You are a professional educational content creator.

Task: Generate educational Q&A pairs
Subject: {subject}
Grade: {grade}
Count: {count}

Requirements:
1. Questions should be appropriate for the grade level
2. Answers should be accurate, detailed, and easy to understand
3. Each answer should include explanation process
4. Do not generate sensitive or inappropriate content

Output format (JSON):
[
  {
    "id": 1,
    "question": "Question content",
    "answer": "Answer content",
    "explanation": "Explanation content",
    "difficulty": "easy/medium/hard",
    "knowledge_points": ["point1", "point2"]
  }
]

Start generating:
```

### 2. Parameter Tuning

Adjust model parameters according to needs:

| Parameter | High Quality | Fast Generation | Creative Generation |
|-----------|--------------|------------------|-------------------|
| Temperature | 0.3-0.5 | 0.1-0.3 | 0.7-1.0 |
| Max tokens | As needed | Shorter | Longer |
| Top P | 0.9-0.95 | 0.9 | 0.95-1.0 |

## Common Questions

### Q: Generated data quality is not ideal?

**A**: Optimization suggestions:

1. **Improve prompt**: More detailed and clear instructions
2. **Adjust parameters**: Lower temperature, increase max tokens
3. **Provide examples**: Give examples in prompt
4. **Change model**: Try other LLMs
5. **Manual review**: Manual review and filtering

### Q: Generation speed is slow?

**A**: Acceleration suggestions:

1. **Reduce count**: Generate in smaller batches
2. **Adjust concurrency**: Increase concurrency appropriately
3. **Use faster model**: Like GPT-3.5-Turbo
4. **Shorten output**: Reduce max tokens
5. **Use local model**: Deploy local model for acceleration

## API Reference

For detailed API documentation, see:
- [Data Synthesis API](/docs/api-reference/data-synthesis/)

## Related Documentation

- [Data Evaluation](/docs/user-guide/data-evaluation/) - Evaluate synthesized data quality
- [Data Management](/docs/user-guide/data-management/) - Manage synthesized data
- [Pipeline Orchestration](/docs/user-guide/orchestration/) - Integrate synthesis tasks into pipelines
