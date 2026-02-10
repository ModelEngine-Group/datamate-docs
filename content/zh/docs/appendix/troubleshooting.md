---
title: 故障排查
description: DataMate 常见问题和解决方案
weight: 2
---

{{% pageinfo %}}
本文档提供 DataMate 常见问题的排查步骤和解决方案。
{{% /pageinfo %}}

## 服务启动问题

### 服务无法启动

#### 症状

执行 `make install` 后，服务无法启动或立即退出。

#### 排查步骤

1. **检查端口占用**

```bash
# 检查端口是否被占用
lsof -i :8080  # API Gateway
lsof -i :30000 # Frontend
lsof -i :5432  # PostgreSQL
```

如果端口被占用：
```bash
# 查找占用进程
ps aux | grep <port>

# 终止进程
kill -9 <PID>
```

2. **查看容器日志**

```bash
# 查看所有容器状态
docker ps -a

# 查看特定容器日志
docker logs datamate-backend
docker logs datamate-frontend
docker logs datamate-database
```

3. **检查 Docker 资源**

```bash
# 查看 Docker 系统信息
docker system df

# 清理未使用的资源
docker system prune -a
```

#### 常见原因和解决方案

| 原因 | 解决方案 |
|------|----------|
| 端口被占用 | 终止占用进程或修改端口映射 |
| 内存不足 | 增加 Docker 内存限制 |
| 镜像未拉取 | 执行 `docker pull` 拉取镜像 |
| 网络问题 | 检查防火墙和网络配置 |

### 容器启动后立即退出

#### 排查步骤

```bash
# 查看容器退出码
docker ps -a

# 查看详细日志
docker logs <container-name> --tail 100

# 检查容器健康状态
docker inspect <container-name> | grep -A 10 Health
```

#### 常见原因

- **配置错误**：检查环境变量和配置文件
- **依赖服务未启动**：确保数据库等依赖服务已启动
- **资源不足**：检查内存和磁盘空间

## 数据库连接问题

### 无法连接到数据库

#### 症状

后端服务日志显示数据库连接错误。

#### 排查步骤

1. **检查数据库容器**

```bash
# 检查数据库容器状态
docker ps | grep datamate-database

# 查看数据库日志
docker logs datamate-database
```

2. **测试数据库连接**

```bash
# 进入数据库容器
docker exec -it datamate-database psql -U postgres -d datamate

# 或从本地连接
psql -h localhost -U postgres -d datamate
```

3. **检查数据库配置**

```bash
# 检查环境变量
docker exec datamate-backend env | grep DB_

# 检查配置文件
docker exec datamate-backend cat /app/application.yml | grep datasource
```

#### 常见解决方案

| 问题 | 解决方案 |
|------|----------|
| 密码错误 | 检查 `DB_PASSWORD` 环境变量 |
| 数据库未就绪 | 等待数据库完全启动 |
| 网络不通 | 检查 Docker 网络 |
| 连接池耗尽 | 增加连接池大小 |

### 数据库性能问题

#### 症状

查询缓慢，数据库响应时间长。

#### 排查步骤

1. **查看慢查询**

```sql
-- 查看慢查询
SELECT query, mean_exec_time, calls
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 10;
```

2. **检查连接数**

```sql
-- 查看当前连接数
SELECT count(*) FROM pg_stat_activity;

-- 查看最大连接数
SHOW max_connections;
```

3. **分析表大小**

```sql
-- 查看表大小
SELECT
    relname AS table_name,
    pg_size_pretty(pg_total_relation_size(relid)) AS size
FROM pg_catalog.pg_statio_user_tables
ORDER BY pg_total_relation_size(relid) DESC;
```

#### 优化方案

- 添加索引
- 优化查询语句
- 增加连接池大小
- 定期清理和分析表

## 前端问题

### 前端无法访问

#### 症状

浏览器无法访问 http://localhost:30000

#### 排查步骤

1. **检查前端容器**

```bash
docker ps | grep datamate-frontend
docker logs datamate-frontend
```

2. **检查端口映射**

```bash
# 查看端口映射
docker port datamate-frontend
```

3. **测试网络连接**

```bash
# 测试容器内部服务
docker exec datamate-frontend wget -O- http://localhost:3000
```

#### 常见解决方案

- 检查端口是否正确映射
- 检查防火墙设置
- 查看浏览器控制台错误信息
- 清除浏览器缓存

### API 请求失败

#### 症状

前端页面显示，但 API 请求失败。

#### 排查步骤

1. **检查浏览器控制台**

```javascript
// 查看网络请求
// 开发者工具 -> Network 标签
```

2. **检查 API Gateway**

```bash
# 检查 API Gateway 容器
docker ps | grep datamate-gateway
docker logs datamate-gateway
```

3. **测试 API**

```bash
# 测试 API 连接
curl http://localhost:8080/actuator/health

# 测试 API 端点
curl http://localhost:8080/api/v1/data-management/datasets
```

#### 常见解决方案

| 错误 | 解决方案 |
|------|----------|
| CORS 错误 | 配置 API Gateway CORS |
| 401 Unauthorized | 检查认证配置 |
| 404 Not Found | 检查 API 路径配置 |
| 503 Service Unavailable | 检查后端服务状态 |

## 任务执行问题

### 任务卡住不动

#### 症状

任务状态一直为"运行中"，但没有任何进展。

#### 排查步骤

1. **查看任务日志**

```bash
# 查看后端服务日志
docker logs datamate-backend --tail 100 | grep <task-id>

# 查看运行时日志
docker logs datamate-runtime --tail 100
```

2. **检查任务状态**

```bash
# 通过 API 查询任务状态
curl http://localhost:8080/api/v1/data-cleaning/tasks/<task-id>
```

3. **检查系统资源**

```bash
# 检查 CPU 和内存使用
docker stats

# 检查磁盘空间
df -h
```

#### 常见原因和解决方案

| 原因 | 解决方案 |
|------|----------|
| 算子执行失败 | 查看运行时日志，修复算子代码 |
| 数据量过大 | 分批处理 |
| 资源不足 | 增加容器资源限制 |
| 死锁 | 重启服务 |

### 任务执行失败

#### 排查步骤

1. **查看详细错误日志**

```bash
# 查看任务执行日志
docker logs datamate-backend | grep ERROR
```

2. **检查任务配置**

```bash
# 查看任务配置
curl http://localhost:8080/api/v1/data-cleaning/tasks/<task-id>
```

3. **验证输入数据**

检查输入数据格式和内容是否正确。

## 性能问题

### 系统响应缓慢

#### 排查步骤

1. **检查系统资源**

```bash
# 查看 CPU、内存使用
docker stats

# 查看磁盘 I/O
docker exec datamate-backend iostat -x 1
```

2. **检查数据库性能**

```sql
-- 查看活跃查询
SELECT * FROM pg_stat_activity WHERE state = 'active';

-- 查看锁等待
SELECT * FROM pg_stat_activity WHERE wait_event_type = 'Lock';
```

3. **检查 Redis 状态**

```bash
# 查看 Redis 信息
docker exec datamate-redis redis-cli INFO

# 查看慢查询
docker exec datamate-redis redis-cli SLOWLOG GET
```

#### 优化方案

- 增加容器资源限制
- 优化数据库查询
- 启用缓存
- 使用负载均衡

### 内存溢出

#### 症状

容器因内存不足被 OOM Killer 终止。

#### 排查步骤

```bash
# 查看容器退出原因
docker inspect <container> | grep OOMKilled

# 查看内存使用历史
docker stats --no-stream
```

#### 解决方案

```yaml
# 增加内存限制
services:
  datamate-backend:
    deploy:
      resources:
        limits:
          memory: 8G  # 增加内存限制
```

## 日志查看

### 查看应用日志

```bash
# 后端日志
docker logs datamate-backend --tail 100 -f

# 前端日志
docker logs datamate-frontend --tail 100 -f

# 数据库日志
docker logs datamate-database --tail 100 -f

# 运行时日志
docker logs datamate-runtime --tail 100 -f
```

### 查看特定级别日志

```bash
# 只查看错误日志
docker logs datamate-backend 2>&1 | grep ERROR

# 查看警告日志
docker logs datamate-backend 2>&1 | grep WARN
```

### 日志文件位置

| 服务 | 日志路径 |
|------|----------|
| 后端 | `/var/log/datamate/backend/app.log` |
| 前端 | `/var/log/datamate/frontend/` |
| 数据库 | `/var/log/datamate/database/` |
| 运行时 | `/var/log/datamate/runtime/` |

## 获取帮助

如果以上方法都无法解决问题：

1. **收集信息**
   - 错误信息
   - 日志文件
   - 系统环境
   - 复现步骤

2. **搜索已有问题**

访问 [GitHub Issues](https://github.com/ModelEngine-Group/DataMate/issues) 搜索类似问题。

3. **提交新 Issue**

在 GitHub 提交 Issue 时，请提供：
- DataMate 版本
- 操作系统版本
- Docker 版本
- 详细的错误信息
- 复现步骤

## 相关文档

- [安装部署指南](/docs/getting-started/installation/) - 部署说明
- [配置参数](/docs/appendix/configuration/) - 配置说明
- [API 参考](/docs/api-reference/) - API 文档
