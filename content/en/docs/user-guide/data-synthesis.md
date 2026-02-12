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

In the left navigation, select **Data Synthesis** → **Synthesis Tasks**.

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

#### Step 1: Fill Basic Information

1. Return to **Data Synthesis** page
2. Click **Create Task** button
3. Fill basic information:
   - **Task name**: e.g., `medical_qa_synthesis`
   - **Task description**: Describe task purpose (optional)

#### Step 2: Select Dataset and Files

Select required data from existing datasets:

- **Select dataset**: Choose the dataset to use from the list
- **Select files**:
  - Can select all files from a dataset
  - Can also select specific files from a dataset
  - Support selecting multiple files

#### Step 3: Select Synthesis Instruction Template

Select an existing template or create a new one:

- **Select from template library**: Choose from created templates
- **Template type**: Q&A generation, dialogue generation, summary generation, etc.
- **Preview template**: View template prompt content

#### Step 4: Fill Synthesis Configuration

The synthesis configuration consists of four parts:

**1. Set Total Synthesis Count**

Set the maximum limit for the entire task:

| Parameter | Description | Default Value | Range |
|-----------|-------------|----------------|--------|
| Maximum QA Pairs | Maximum number of QA pairs to generate for entire task | 5000 | 1-100,000 |

This setting is optional, used for total volume control in large-scale synthesis tasks.

**2. Configure Text Chunking Strategy**

Chunk the input text files, supporting multiple chunking methods:

| Parameter | Description | Default Value |
|-----------|-------------|---------------|
| Chunking Method | Select chunking strategy | Default chunking |
| Chunk Size | Character count per chunk | 3000 |
| Overlap Size | Overlap characters between adjacent chunks | 100 |

**Chunking Method Options**:
- **Default Chunking (默认分块)**: Use system default intelligent chunking strategy
- **Chapter-based Chunking (按章节分块)**: Split by chapter structure
- **Paragraph-based Chunking (按段落分块)**: Split by paragraph boundaries
- **Fixed Length Chunking (固定长度分块)**: Split by fixed character length
- **Custom Separator Chunking (自定义分隔符分块)**: Split by custom delimiter

**3. Configure Question Synthesis Parameters**

Set parameters for question generation:

| Parameter | Description | Default Value | Range |
|-----------|-------------|----------------|--------|
| Question Count | Number of questions generated per chunk | 1 | 1-20 |
| Temperature | Control randomness and diversity of question generation | 0.7 | 0-2 |
| Model | Select CHAT model for question generation | - | Select from model list |

**Parameter Notes**:
- **Question Count**: Number of questions generated per text chunk. Higher value generates more questions.
- **Temperature**: Higher values produce more diverse questions, lower values produce more stable questions.

**4. Configure Answer Synthesis Parameters**

Set parameters for answer generation:

| Parameter | Description | Default Value | Range |
|-----------|-------------|----------------|--------|
| Temperature | Control stability of answer generation | 0.7 | 0-2 |
| Model | Select CHAT model for answer generation | - | Select from model list |

**Parameter Notes**:
- **Temperature**: Lower values produce more conservative and accurate answers, higher values produce more diverse and creative answers.

**Synthesis Types**:
The system supports two synthesis types:
- **SFT Q&A Synthesis (SFT 问答数据合成)**: Generate Q&A pairs for supervised fine-tuning
- **COT Chain-of-Thought Synthesis (COT 链式推理合成)**: Generate data with reasoning process

#### Step 5: Start Task

Click **Start Task** button, task will automatically start executing.

### 3. Create Ratio Synthesis Task

Ratio synthesis tasks are used to synthesize multi-category balanced data in specified proportions.

#### Step 1: Create Ratio Task

1. In the left navigation, select **Data Synthesis** → **Ratio Tasks**
2. Click **Create Task** button

#### Step 2: Fill Basic Information

| Parameter | Description | Required |
|-----------|-------------|-----------|
| Task Name | Unique identifier for the task | Yes |
| Total Target Count | Target total count for entire ratio task | Yes |
| Task Description | Describe purpose and requirements of ratio task | No |

**Example**:
- Task name: `balanced_dataset_synthesis`
- Total target count: 10000
- Task description: Generate balanced data for training and validation sets

#### Step 3: Select Datasets

Select datasets to participate in the ratio synthesis from existing datasets:

**Dataset Selection Features**:
- **Search Datasets**: Search datasets by keyword
- **Multi-select Support**: Can select multiple datasets simultaneously
- **Dataset Information**: Display detailed information for each dataset
  - Dataset name and type
  - Dataset description
  - File count
  - Dataset size
  - Label distribution preview (up to 8 labels)

After selecting datasets, the system automatically loads label distribution information for each dataset.

#### Step 4: Fill Ratio Configuration

Configure specific synthesis rules for each selected dataset:

**Ratio Configuration Items**:

| Parameter | Description | Range |
|-----------|-------------|--------|
| Label | Select label from dataset's label distribution | Based on dataset labels |
| Label Value | Specific value under selected label | Based on label value list |
| Label Update Time | Select label update date range (optional) | Date picker |
| Quantity | Data count to generate for this config | 0 to total target count |

**Feature Notes**:
- **Auto Distribute**: Click "Auto Distribute" button, system automatically distributes total count evenly across datasets
- **Quantity Limit**: Each configuration item's quantity cannot exceed the dataset's total file count
- **Percentage Calculation**: System automatically calculates percentage of each configuration item
- **Delete Configuration**: Can delete unwanted configuration items
- **Add Configuration**: Each dataset can have multiple different label configurations

**Example Configuration**:

| Dataset | Label | Label Value | Label Update Time | Quantity |
|----------|-------|--------------|-------------------|----------|
| Training Dataset | Category | Training | - | 6000 |
| Training Dataset | Category | Validation | - | 2000 |
| Test Dataset | Category | Test | 2024-01-01 to 2024-12-31 | 2000 |

#### Step 5: Execute Task

Click **Start Task** button, the system will create and execute the task according to ratio configuration.

### 4. Monitor Synthesis Task

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
