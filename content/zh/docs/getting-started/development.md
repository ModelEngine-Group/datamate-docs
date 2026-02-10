---
title: 开发环境搭建
description: DataMate 本地开发环境配置指南
weight: 3
---

{{% pageinfo %}}
本文档介绍如何搭建 DataMate 的本地开发环境。
{{% /pageinfo %}}

## 前置要求

### 必需软件

| 软件 | 版本要求 | 用途 |
|------|----------|------|
| Node.js | 18.x+ | 前端开发 |
| pnpm | 8.x+ | 前端包管理 |
| Java | 21 | 后端开发 |
| Maven | 3.9+ | 后端构建 |
| Python | 3.11+ | Python 服务开发 |
| Docker | 20.10+ | 容器化部署 |
| Docker Compose | 2.0+ | 服务编排 |
| Git | 2.x+ | 版本控制 |
| Make | 4.x+ | 构建自动化 |

### 推荐软件

- **IDE**：IntelliJ IDEA (后端) + VS Code (前端/Python)
- **数据库客户端**：DBeaver、pgAdmin
- **API 测试工具**：Postman、curl
- **Git 客户端**：GitKraken、SourceTree

## 代码结构

```
DataMate/
├── backend/                 # Java 后端
│   ├── services/           # 微服务模块
│   │   ├── main-application/
│   │   ├── data-management-service/
│   │   ├── data-cleaning-service/
│   │   ├── data-collection-service/
│   │   ├── data-annotation-service/
│   │   ├── data-synthesis-service/
│   │   ├── data-evaluation-service/
│   │   ├── operator-market-service/
│   │   └── rag-indexer-service/
│   ├── openapi/            # OpenAPI 规范
│   └── scripts/            # 构建脚本
├── frontend/               # React 前端
│   ├── src/
│   │   ├── components/    # 公共组件
│   │   ├── pages/         # 页面组件
│   │   ├── services/      # API 服务
│   │   ├── store/         # Redux store
│   │   └── routes/        # 路由配置
│   └── package.json
├── runtime/                # Python 运行时
│   ├── deer-flow/         # DeerFlow 集成
│   └── datamate/          # DataMate 运行时
└── deployment/             # 部署配置
    ├── docker/            # Docker 配置
    └── helm/              # Helm Charts
```

## 后端开发环境

### 1. 安装 Java 21

```bash
# macOS (使用 Homebrew)
brew install openjdk@21

# Linux (Ubuntu/Debian)
sudo apt update
sudo apt install openjdk-21-jdk

# 验证安装
java -version
```

### 2. 安装 Maven

```bash
# macOS
brew install maven

# Linux
sudo apt install maven

# 验证安装
mvn -version
```

### 3. 配置 IDE（IntelliJ IDEA）

#### 安装插件
- Lombok Plugin
- MyBatis Plugin
- Rainbow Brackets
- GitToolBox

#### 导入项目
1. 打开 IntelliJ IDEA
2. 选择 `File` → `Open`
3. 选择 `backend` 目录
4. 等待 Maven 依赖下载完成

#### 配置运行配置

创建 Spring Boot 运行配置：
- 主类：对应服务的 Application 类
- 工作目录：服务模块目录
- VM 参数：`-Dspring.profiles.active=dev`

### 4. 配置数据库

#### 启动本地数据库（Docker）

```bash
# 仅启动数据库
docker compose -f deployment/docker/datamate/docker-compose.yml up -d datamate-database

# 查看数据库连接信息
docker logs datamate-database
```

默认连接信息：
- 主机：localhost
- 端口：5432
- 数据库：datamate
- 用户名：postgres
- 密码：password

#### 初始化数据库

```bash
# 使用 Flyway 迁移（如果配置）
mvn flyway:migrate

# 或执行 SQL 脚本
psql -h localhost -U postgres -d datamate -f schema.sql
```

### 5. 运行后端服务

#### 使用 Maven 运行

```bash
cd backend/services/main-application
mvn spring-boot:run
```

#### 使用 IDE 运行

1. 找到对应的 Application 类
2. 右键 → Run
3. 访问 http://localhost:8080

### 6. 开发配置

编辑 `application-dev.yml`：

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/datamate
    username: postgres
    password: password
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: update

logging:
  level:
    com.datamate: DEBUG
```

## 前端开发环境

### 1. 安装 Node.js

```bash
# macOS (使用 Homebrew)
brew install node@18

# Linux (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# 验证安装
node -v
npm -v
```

### 2. 安装 pnpm

```bash
npm install -g pnpm

# 验证安装
pnpm -v
```

### 3. 安装依赖

```bash
cd frontend

# 安装依赖
pnpm install
```

### 4. 配置开发环境

创建 `.env.development`：

```bash
# API 地址
VITE_API_BASE_URL=http://localhost:8080
VITE_API_TIMEOUT=30000

# 其他配置
VITE_APP_TITLE=DataMate Dev
```

### 5. 启动开发服务器

```bash
# 启动开发服务器
pnpm dev

# 或指定端口
pnpm dev --port 3000
```

访问 http://localhost:3000

### 6. 构建生产版本

```bash
# 构建
pnpm build

# 预览构建结果
pnpm preview
```

### 7. 前端开发技巧

#### 热更新
- 修改 `.tsx` 或 `.css` 文件会自动热更新
- 修改配置文件需要重启

#### 代理配置

在 `vite.config.ts` 中配置代理：

```typescript
server: {
  proxy: {
    '/api': {
      target: 'http://localhost:8080',
      changeOrigin: true,
    }
  }
}
```

#### 路径别名

```typescript
import MyComponent from '@/components/MyComponent';
import { useStore } from '@/store/hooks';
```

## Python 服务开发环境

### 1. 安装 Python 3.11

```bash
# macOS
brew install python@3.11

# Linux
sudo apt install python3.11 python3.11-venv

# 验证安装
python3.11 --version
```

### 2. 创建虚拟环境

```bash
# 进入项目目录
cd runtime/datamate

# 创建虚拟环境
python3.11 -m venv venv

# 激活虚拟环境
source venv/bin/activate  # Linux/macOS
# venv\Scripts\activate   # Windows
```

### 3. 安装依赖

```bash
# 安装依赖
pip install -r requirements.txt

# 或使用 uv（更快）
pip install uv
uv pip install -r requirements.txt
```

### 4. 配置环境变量

创建 `.env` 文件：

```bash
# 数据库配置
PG_HOST=localhost
PG_PORT=5432
PG_USER=postgres
PG_PASSWORD=password
PG_DATABASE=datamate

# 日志级别
log_level=DEBUG

# JWT 配置
datamate_jwt_enable=false
```

### 5. 运行 Python 服务

```bash
# 运行运行时服务
python /opt/runtime/datamate/operator_runtime.py --port 8081

# 或使用 FastAPI
uvicorn main:app --reload --port 18000
```

### 6. Python 开发工具

#### 推荐插件
- Python (VS Code)
- Pylance
- Python Test Explorer

#### 代码格式化
```bash
# Black
pip install black
black .

# isort
pip install isort
isort .

# 自动格式化
pip install autopep8
```

## 本地调试

### 1. 启动所有服务

#### 使用 Docker Compose

```bash
# 启动基础服务（数据库、Redis 等）
docker compose -f deployment/docker/datamate/docker-compose.yml up -d \
  datamate-database \
  datamate-redis

# 启动 Milvus（可选）
docker compose -f deployment/docker/datamate/docker-compose.yml --profile milvus up -d
```

#### 启动后端服务

```bash
# 终端 1：启动 Main Application
cd backend/services/main-application
mvn spring-boot:run

# 终端 2：启动 Data Management Service
cd backend/services/data-management-service
mvn spring-boot:run

# ... 其他服务
```

#### 启动前端

```bash
cd frontend
pnpm dev
```

#### 启动 Python 服务

```bash
# 终端 N：启动运行时服务
cd runtime/datamate
python operator_runtime.py --port 8081

# 终端 N+1：启动后端 Python 服务
cd backend-python
uvicorn main:app --reload --port 18000
```

### 2. 调试技巧

#### 后端调试

在 IntelliJ IDEA 中：
1. 在代码行号处点击设置断点
2. 右键 → Debug 运行配置
3. 发送请求触发断点

#### 前端调试

在浏览器中：
1. 按 F12 打开开发者工具
2. 使用 `console.log` 或 `debugger` 语句
3. React DevTools 查看组件状态

#### Python 调试

在 VS Code 中：
1. 安装 Python 扩展
2. 创建 `launch.json` 配置
3. 设置断点并 F5 启动调试

### 3. 日志查看

```bash
# Docker 容器日志
docker logs -f datamate-backend

# 应用日志
tail -f /var/log/datamate/backend/app.log

# 只查看错误日志
tail -f /var/log/datamate/backend/app.log | grep ERROR
```

## 代码规范

### Java 代码规范

#### 命名规范
- 类名：大驼峰 `UserService`
- 方法名：小驼峰 `getUserById`
- 常量：全大写 `MAX_SIZE`
- 变量：小驼峰 `userName`

#### 注释规范
```java
/**
 * 用户服务
 *
 * @author Your Name
 * @since 1.0.0
 */
public class UserService {

    /**
     * 根据用户 ID 获取用户信息
     *
     * @param userId 用户 ID
     * @return 用户信息
     * @throws UserNotFoundException 用户不存在异常
     */
    public User getUserById(Long userId) {
        // ...
    }
}
```

### TypeScript 代码规范

#### 命名规范
- 组件：大驼峰 `UserProfile`
- 类型/接口：大驼峰 `UserData`
- 函数：小驼峰 `getUserData`
- 常量：全大写 `API_BASE_URL`

#### 组件规范
```typescript
interface UserProfileProps {
  userId: string;
  onUpdate?: () => void;
}

export const UserProfile: React.FC<UserProfileProps> = ({
  userId,
  onUpdate
}) => {
  // 组件逻辑
  return <div>...</div>;
};

export default UserProfile;
```

### Python 代码规范

#### 遵循 PEP 8
```python
def get_user(user_id: int) -> dict:
    """
    获取用户信息

    Args:
        user_id: 用户 ID

    Returns:
        用户信息字典
    """
    # ...
```

## 测试

### 后端测试

```bash
# 单元测试
mvn test

# 集成测试
mvn verify

# 生成测试报告
mvn jacoco:report
```

### 前端测试

```bash
# 单元测试
pnpm test

# E2E 测试
pnpm test:e2e

# 测试覆盖率
pnpm test:coverage
```

## 常见问题

### 后端启动失败

1. 检查 Java 版本：`java -version`
2. 检查端口占用：`lsof -i :8080`
3. 查看详细日志
4. 清理并重新构建：`mvn clean install`

### 前端启动失败

1. 检查 Node 版本：`node -v`
2. 删除 node_modules 重新安装：`rm -rf node_modules && pnpm install`
3. 检查端口占用：`lsof -i :3000`
4. 清理缓存：`pnpm store prune`

### 数据库连接失败

1. 检查数据库是否启动：`docker ps | grep postgres`
2. 测试连接：`psql -h localhost -U postgres -d datamate`
3. 检查防火墙设置
4. 验证连接配置

## 下一步

- [后端架构](/docs/developer-guide/backend-architecture/) - 详细的后端架构说明
- [前端架构](/docs/developer-guide/frontend-architecture/) - 详细的前端架构说明
- [贡献指南](/docs/developer-guide/contribute/) - 如何贡献代码
