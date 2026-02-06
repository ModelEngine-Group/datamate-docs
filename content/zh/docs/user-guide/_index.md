---
title: 用户指南
description: DataMate 功能使用指南
weight: 3
---

{{% pageinfo %}}
本指南介绍 DataMate 各个功能模块的使用方法。
{{% /pageinfo %}}

DataMate 提供完整的大模型数据处理解决方案，涵盖数据采集、管理、清洗、标注、合成、评估等全流程。

## 功能模块

- [数据采集](/docs/user-guide/data-collection/) - 从多种数据源采集数据
- [数据管理](/docs/user-guide/data-management/) - 管理数据集和文件
- [数据清洗](/docs/user-guide/data-cleansing/) - 清洗和预处理数据
- [数据标注](/docs/user-guide/data-annotation/) - 数据标注和质量控制
- [数据合成](/docs/user-guide/data-synthesis/) - 基于大模型的数据增强
- [数据评估](/docs/user-guide/data-evaluation/) - 数据质量评估
- [知识库管理](/docs/user-guide/knowledge-base/) - RAG 知识库构建
- [算子市场](/docs/user-guide/operator-market/) - 数据处理算子管理
- [流水线编排](/docs/user-guide/orchestration/) - 可视化流程编排
- [Agent 对话](/docs/user-guide/agent/) - AI 智能助手

## 典型使用场景

### 模型微调场景

```
1. 数据采集 → 2. 数据管理 → 3. 数据清洗 → 4. 数据标注
↓
5. 数据评估 → 6. 导出训练数据
```

### RAG 应用场景

```
1. 上传文档 → 2. 向量化索引 → 3. 知识库管理
↓
4. Agent 对话（知识库问答）
```

### 数据增强场景

```
1. 准备原始数据 → 2. 创建指令模板 → 3. 数据合成
↓
4. 质量评估 → 5. 导出增强数据
```

## 快速链接

- [快速开始](/docs/getting-started/) - 部署和安装
- [API 参考](/docs/api-reference/) - API 文档
- [开发者指南](/docs/developer-guide/) - 架构和开发
