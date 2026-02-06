---
title: 后端架构
description: DataMate Java 后端架构设计
weight: 1
---

{{% pageinfo %}}
DataMate 后端采用微服务架构，基于 Spring Boot 3.x 和 Spring Cloud 构建。
{{% /pageinfo %}}

## 架构概览

DataMate 后端采用微服务架构，将系统拆分为多个独立的服务：

```
┌─────────────────────────────────────────────┐
│              API Gateway                    │
│         (Spring Cloud Gateway)              │
│              Port: 8080                     │
└──────────────┬──────────────────────────────┘
               │
       ┌───────┴───────┬───────────────┐
       ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│   Main       │ │  Data        │ │  Data        │
│ Application  │ │  Management  │ │  Collection  │
└──────────────┘ └──────────────┘ └──────────────┘
       │               │               │
       └───────────────┴───────────────┘
                       │
                       ▼
              ┌────────────────┐
              │   PostgreSQL   │
              │   Port: 5432   │
              └────────────────┘
```

## 技术栈

### 核心框架

| 技术 | 版本 | 用途 |
|------|------|------|
| Java | 21 | 编程语言 |
| Spring Boot | 3.5.6 | 应用框架 |
| Spring Cloud | 2023.x | 微服务框架 |
| MyBatis Plus | 3.5.x | ORM 框架 |
| PostgreSQL Driver | 42.7.x | 数据库驱动 |

### 支持组件

| 技术 | 版本 | 用途 |
|------|------|------|
| Redis | 5.x | 缓存和消息队列 |
| MinIO | 8.x | 对象存储 |
| Milvus SDK | 2.3.x | 向量数据库 |
| OpenAI API | - | LLM 集成 |

## 微服务列表

### API Gateway

**端口**: 8080

**功能**:
- 统一入口
- 路由转发
- 认证授权
- 限流熔断

**技术**:
- Spring Cloud Gateway
- JWT 认证
- 限流：Redis + Lua

### Main Application

**端口**: 无（内部服务）

**功能**:
- 用户管理
- 权限管理
- 系统配置
- 任务调度

**核心模块**:
```
com.datamate.main
├── auth            # 认证授权
├── user            # 用户管理
├── permission      # 权限管理
├── config          # 配置管理
└── scheduler       # 任务调度
```

### Data Management Service

**端口**: 8092

**功能**:
- 数据集管理
- 文件管理
- 标签管理
- 统计分析

**API 端点**:
- `/data-management/datasets` - 数据集管理
- `/data-management/datasets/{id}/files` - 文件管理
- `/data-management/tags` - 标签管理

**核心模块**:
```
com.datamate.data.management
├── dataset         # 数据集管理
├── file            # 文件管理
├── tag             # 标签管理
└── statistics      # 统计分析
```

### Data Collection Service

**端口**: 无（内部服务）

**功能**:
- 数据采集任务管理
- DataX 集成
- 任务执行监控

**核心模块**:
```
com.datamate.data.collection
├── task            # 任务管理
├── execution       # 执行管理
└── datax           # DataX 集成
```

### Data Cleaning Service

**端口**: 无（内部服务）

**功能**:
- 数据清洗任务管理
- 清洗模板管理
- 算子调用

**核心模块**:
```
com.datamate.data.cleaning
├── task            # 任务管理
├── template        # 模板管理
└── operator        # 算子调用
```

### Data Synthesis Service

**端口**: 无（内部服务）

**功能**:
- 数据合成任务管理
- 指令模板管理
- LLM 集成

**核心模块**:
```
com.datamate.data.synthesis
├── task            # 任务管理
├── template        # 模板管理
└── llm             # LLM 集成
```

### Runtime Service

**端口**: 8081

**功能**:
- 算子执行
- Ray 集成
- 任务调度

**技术**:
- Python + Ray
- FastAPI
- 算子插件系统

## 数据库设计

### 主要数据表

#### 用户表 (users)

| 字段 | 类型 | 说明 |
|------|------|------|
| id | BIGINT | 主键 |
| username | VARCHAR(50) | 用户名 |
| password | VARCHAR(255) | 密码（加密） |
| email | VARCHAR(100) | 邮箱 |
| role | VARCHAR(20) | 角色 |
| created_at | TIMESTAMP | 创建时间 |

#### 数据集表 (datasets)

| 字段 | 类型 | 说明 |
|------|------|------|
| id | VARCHAR(50) | 主键 |
| name | VARCHAR(100) | 名称 |
| description | TEXT | 描述 |
| type | VARCHAR(20) | 类型 |
| status | VARCHAR(20) | 状态 |
| created_by | VARCHAR(50) | 创建者 |
| created_at | TIMESTAMP | 创建时间 |

#### 任务表 (tasks)

| 字段 | 类型 | 说明 |
|------|------|------|
| id | VARCHAR(50) | 主键 |
| name | VARCHAR(100) | 名称 |
| type | VARCHAR(20) | 任务类型 |
| status | VARCHAR(20) | 状态 |
| config | JSON | 配置 |
| created_at | TIMESTAMP | 创建时间 |

### 数据库连接配置

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/datamate
    username: postgres
    password: ${DB_PASSWORD:password}
    driver-class-name: org.postgresql.Driver
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
```

## 服务通信

### 同步通信

服务间使用 HTTP/REST 进行同步通信：

```java
// 使用 Feign Client
@FeignClient(name = "data-management-service")
public interface DataManagementClient {
    @GetMapping("/data-management/datasets/{id}")
    DatasetResponse getDataset(@PathVariable String id);
}
```

### 异步通信

使用 Redis 实现异步消息队列：

```java
// 发送消息
redisTemplate.convertAndSend("task.created", taskMessage);

// 接收消息
@RedisListener(topic = "task.created")
public void handleTaskCreated(TaskMessage message) {
    // 处理任务创建事件
}
```

## 认证授权

### JWT 认证

```java
// JWT 配置
@Configuration
public class JwtConfig {
    @Value("${datamate.jwt.secret}")
    private String secret;

    @Value("${datamate.jwt.expiration}")
    private Long expiration;
}

// JWT 工具类
public class JwtUtil {
    public String generateToken(User user) {
        // 生成 JWT Token
    }

    public Claims parseToken(String token) {
        // 解析 JWT Token
    }
}
```

### 权限控制

```java
// RBAC 权限控制
@PreAuthorize("hasRole('ADMIN')")
public void adminOperation() {
    // 管理员操作
}

@PreAuthorize("hasAnyRole('ADMIN', 'USER')")
public void userOperation() {
    // 用户操作
}
```

## 异常处理

### 统一异常处理

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException e) {
        return ResponseEntity.badRequest()
            .body(new ErrorResponse("INVALID_PARAMETER", e.getMessage()));
    }

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(new ErrorResponse("NOT_FOUND", e.getMessage()));
    }
}
```

## 配置管理

### 配置文件

```yaml
# application.yml
spring:
  application:
    name: data-management-service
  profiles:
    active: ${SPRING_PROFILES_ACTIVE:dev}

# application-dev.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/datamate

# application-prod.yml
spring:
  datasource:
    url: jdbc:postgresql://${DB_HOST}:${DB_PORT}/${DB_NAME}
```

### 配置中心

使用 Spring Cloud Config 进行配置管理：

```yaml
spring:
  cloud:
    config:
      uri: http://config-server:8888
      name: ${spring.application.name}
      profile: ${spring.profiles.active}
```

## 日志管理

### 日志配置

```xml
<!-- logback-spring.xml -->
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>/var/log/datamate/backend/app.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>/var/log/datamate/backend/app-%d{yyyy-MM-dd}.log</fileNamePattern>
        </rollingPolicy>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

## 性能优化

### 数据库优化

- 连接池配置
- 查询优化
- 索引优化
- 分页查询

### 缓存策略

```java
// 使用 Redis 缓存
@Cacheable(value = "datasets", key = "#id")
public Dataset getDataset(String id) {
    return datasetRepository.findById(id);
}

@CacheEvict(value = "datasets", key = "#id")
public void deleteDataset(String id) {
    datasetRepository.deleteById(id);
}
```

## 相关文档

- [前端架构](/docs/developer-guide/frontend-architecture/) - 前端架构设计
- [数据库设计](/docs/developer-guide/database-schema/) - 数据库表结构
- [开发环境搭建](/docs/getting-started/development/) - 开发环境配置
