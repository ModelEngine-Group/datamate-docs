---
title: Agent Chat
description: Use DataMate Agent for intelligent conversation
weight: 10
---

{{% pageinfo %}}
Agent chat module integrates large language models to provide intelligent Q&A and knowledge base retrieval capabilities.
{{% /pageinfo %}}

## Features Overview

Agent chat module provides:

- **Intelligent Chat**: Natural language conversation based on LLMs
- **Knowledge Base Q&A**: RAG-integrated knowledge base Q&A
- **Multi-turn Dialogue**: Support context-aware multi-turn conversation
- **Dialogue Management**: Conversation history and management
- **Quick Commands**: Quick commands for common operations

## Quick Start

### 1. Access Agent

Select **Agent Chat** in left navigation, or visit:
- Local: http://localhost:30000/chat
- Production: https://your-domain/chat

### 2. Start Conversation

Enter question in dialog box, e.g.:

```
User: What data formats does DataMate support?

Assistant: DataMate supports multiple data formats:

Images: JPG, PNG, GIF, BMP, WebP
Text: TXT, MD, JSON, CSV
Audio: MP3, WAV, FLAC, AAC
Video: MP4, AVI, MOV, MKV

These formats can be uploaded and managed in the data management module.
```

### 3. Use Knowledge Base Q&A

#### Enable Knowledge Base

1. Click **Settings** button
2. In **Knowledge Base** settings, select knowledge base to use
3. Save settings

#### Knowledge Base Q&A

```
User: How to create a data cleaning task?

Assistant: According to the knowledge base documentation:

1. Enter data processing page
2. Click create task button
3. Configure basic information
4. Configure cleaning pipeline (drag operators to canvas)
5. Configure execution parameters
6. Create and execute task

[Source: user_guide.md, data_cleansing.md]
```

## Advanced Features

### Conversation Modes

#### General Chat

Use LLM for general conversation without knowledge base.

#### Knowledge Base Q&A

Answer questions based on knowledge base content.

#### Mixed Mode

Combine general chat and knowledge base Q&A.

### Quick Commands

| Command | Function | Example |
|---------|----------|---------|
| `/dataset` | Query datasets | `/dataset list` |
| `/task` | Query tasks | `/task status` |
| `/help` | Show help | `/help` |
| `/clear` | Clear conversation | `/clear` |

### Conversation History

#### View History

1. Click **History** tab on left
2. Select historical conversation
3. View conversation content

#### Continue Conversation

Click historical conversation to continue.

#### Export Conversation

Export conversation records:
- **Markdown**: Export as Markdown file
- **JSON**: Export as JSON
- **PDF**: Export as PDF

## Best Practices

### 1. Effective Questioning

Get better answers:
- **Be specific**: Clear and specific questions
- **Provide context**: Include background information
- **Break down**: Split complex questions

### 2. Knowledge Base Usage

Make the most of knowledge base:
- **Select appropriate knowledge base**: Choose based on question
- **View sources**: Check answer source documents
- **Verify information**: Verify with source documents

## Common Questions

### Q: Inaccurate Agent answers?

**A**: Improve:
1. Optimize question: More specific
2. Check knowledge base: Ensure relevant content exists
3. Change model: Try more powerful model
4. Provide context: More background info

## Related Documentation

- [Knowledge Base Management](/docs/user-guide/knowledge-base/) - Create and manage knowledge bases
- [Data Management](/docs/user-guide/data-management/) - Manage knowledge base documents
