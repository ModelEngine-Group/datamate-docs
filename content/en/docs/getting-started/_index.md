---
title: Getting Started
description: What does your user need to know to try your project?
categories: [Examples, Placeholders]
tags: [test, docs]
weight: 2
---

{{% pageinfo %}}
This is a placeholder page that shows you how to use this template site.
{{% /pageinfo %}}

# 前置条件

- Git (用于拉取源码)
- Make (用于构建和安装)
- Docker (用于构建镜像和部署服务)
- Docker-Compose (用于部署服务-docker方式)
- kubernetes (用于部署服务-k8s方式)
- Helm (用于部署服务-k8s方式)

# 拉取代码

```bash
git clone git@github.com:ModelEngine-Group/DataMate.git
cd DataMate
```

# 镜像构建

```bash
make build
```

# Docker安装

```bash
make install INSTALLER=docker
```

# kubernetes安装

```bash
make install INSTALLER=k8s
```