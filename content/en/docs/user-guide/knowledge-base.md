---
title: Knowledge Base Management
description: Build and manage RAG knowledge bases with DataMate
weight: 7
---

{{% pageinfo %}}
Knowledge base management module helps you build enterprise knowledge bases for efficient vector retrieval and RAG applications.
{{% /pageinfo %}}

## Features Overview

Knowledge base management module provides:

- **Document upload**: Support multiple document formats
- **Text chunking**: Intelligent text splitting strategies
- **Vectorization**: Automatic text-to-vector conversion
- **Vector search**: Semantic similarity-based retrieval
- **Knowledge base Q&A**: RAG-intelligent Q&A

## Supported Document Formats

| Format | Description | Recommended For |
|--------|-------------|-----------------|
| TXT | Plain text | General text |
| PDF | PDF documents | Documents, reports |
| Markdown | Markdown files | Technical docs |
| JSON | JSON data | Structured data |
| CSV | CSV tables | Tabular data |
| DOCX | Word documents | Office documents |

## Quick Start

### 1. Create Knowledge Base

#### Step 1: Enter Knowledge Base Page

In the left navigation, select **Knowledge Generation**.

#### Step 2: Create Knowledge Base

Click **Create Knowledge Base** button in upper right.

#### Step 3: Configure Basic Information

- **Knowledge base name**: e.g., `company_docs_kb`
- **Knowledge base description**: Describe purpose (optional)
- **Knowledge base type**: General / Professional domain

#### Step 4: Configure Vector Parameters

- **Embedding model**: Select embedding model
  - OpenAI text-embedding-ada-002
  - BGE-M3
  - Custom model

- **Vector dimension**: Auto-set based on model
- **Index type**: IVF_FLAT / HNSW / IVF_PQ

#### Step 5: Configure Chunking Strategy

- **Chunking method**:
  - By character count
  - By paragraph
  - By semantic

- **Chunk size**: Size of each text chunk (character count)
- **Overlap size**: Overlap between adjacent chunks

### 2. Upload Documents

#### Step 1: Enter Knowledge Base Details

Click knowledge base name to enter details.

#### Step 2: Upload Documents

1. Click **Upload Document** button
2. Select local files
3. Wait for upload completion

System will automatically:
1. Parse document content
2. Chunk text
3. Generate vectors
4. Build index

### 3. Vector Search

#### Step 1: Enter Search Page

In knowledge base details page, click **Vector Search** tab.

#### Step 2: Enter Query

Enter query in search box, e.g.:

```
How to use DataMate for data cleaning?
```

#### Step 3: View Search Results

System returns most relevant text chunks with similarity scores:

| Rank | Text Chunk | Similarity | Source Doc | Actions |
|------|------------|------------|------------|---------|
| 1 | DataMate's data cleaning module... | 0.92 | user_guide.pdf | View |
| 2 | Configure cleaning task... | 0.87 | tutorial.md | View |
| 3 | Cleaning operator list... | 0.81 | reference.txt | View |

### 4. Knowledge Base Q&A (RAG)

#### Step 1: Enable RAG

In knowledge base details page, click **RAG Q&A** tab.

#### Step 2: Configure RAG Parameters

- **LLM**: Select LLM to use
- **Retrieval count**: Number of text chunks to retrieve
- **Temperature**: Control generation randomness
- **Prompt template**: Custom Q&A template

#### Step 3: Q&A

Enter question in dialog box, e.g.:

```
User: What data cleaning operators does DataMate support?

Assistant: DataMate supports rich data cleaning operators, including:
1. Data quality operators: deduplication, null handling, outlier detection...
2. Text cleaning operators: remove special chars, case conversion...
3. Image cleaning operators: format conversion, quality detection...
[Source: user_guide.pdf, tutorial.md]
```

## Best Practices

### 1. Document Preparation

Before uploading documents:

- **Unify format**: Convert to unified format (PDF, Markdown)
- **Clean content**: Remove irrelevant content (headers, ads)
- **Maintain structure**: Keep good document structure
- **Add metadata**: Add document metadata (author, date, tags)

### 2. Chunking Strategy Selection

Choose based on document type:

| Document Type | Recommended Strategy | Chunk Size |
|---------------|---------------------|------------|
| Technical docs | Paragraph chunking | - |
| Long reports | Semantic chunking | - |
| Short text | Character chunking | 500 |
| Code | Character chunking | 300 |

## Common Questions

### Q: Document stuck in "Processing"?

**A**: Check:

1. **Document format**: Ensure format is supported
2. **Document size**: Single document under 100MB
3. **Vector service**: Check if vector service is running
4. **View logs**: Check detailed error messages

### Q: Inaccurate search results?

**A**: Optimization suggestions:

1. **Adjust chunking**: Try different chunking methods
2. **Increase chunk size**: Add more context
3. **Use reranking**: Enable reranking model
4. **Optimize query**: Use clearer query statements
5. **Change embedding model**: Try other models

## API Reference

For detailed API documentation, see:
- [RAG Indexer API](/docs/api-reference/rag-indexer/)

## Related Documentation

- [Agent Chat](/docs/user-guide/agent/) - Q&A with knowledge base
- [Data Management](/docs/user-guide/data-management/) - Manage knowledge base documents
- [Pipeline Orchestration](/docs/user-guide/orchestration/) - Integrate knowledge base into pipelines
