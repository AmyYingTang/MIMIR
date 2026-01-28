# Phase 3: 系统设计阶段

> **阶段目标**: 完成数据库设计、API 设计、安全设计  
> **产出物**: 数据库设计文档、API 设计文档、配置管理方案  
> **关键原则**: Design-to-Interface、配置驱动、预留扩展

---

## 1. 数据库设计

### 1.1 设计原则

| 原则 | 说明 | 实践 |
|------|------|------|
| **规范化** | 遵循 3NF，避免冗余 | 除非有性能考量 |
| **主键策略** | UUID vs 自增 ID | 分布式用 UUID |
| **软删除** | 重要数据不物理删除 | `deleted_at` 字段 |
| **审计字段** | 记录创建和修改时间 | `created_at`, `updated_at` |
| **索引策略** | 为常用查询创建索引 | 不要过度索引 |

### 1.2 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 表名 | 小写复数，下划线分隔 | `users`, `training_tasks` |
| 字段名 | 小写，下划线分隔 | `created_at`, `user_id` |
| 主键 | `id` | `id` |
| 外键 | `{关联表单数}_id` | `user_id`, `chip_id` |
| 布尔字段 | `is_` 或 `has_` 前缀 | `is_active`, `has_permission` |
| 时间字段 | `_at` 后缀 | `created_at`, `expires_at` |

### 1.3 必备字段清单

每张业务表都应考虑：

```sql
-- 基础字段
id              -- 主键 (UUID 或 BIGINT)
created_at      -- 创建时间
updated_at      -- 更新时间

-- 软删除（重要数据）
deleted_at      -- 删除时间，NULL 表示未删除

-- 审计（如需要）
created_by      -- 创建人
updated_by      -- 更新人
```

### 1.4 敏感数据处理

| 数据类型 | 存储方式 | 字段命名 |
|----------|----------|----------|
| 密码 | bcrypt Hash | `password_hash` |
| 需解密的敏感数据 | AES-256 加密 | `xxx_encrypted` |
| 用于查询的敏感数据 | Hash + 加密双存 | `xxx_hash` + `xxx_encrypted` |
| 手机号/邮箱（展示用） | 原文存储，展示时脱敏 | `phone`, `email` |

### 1.5 设计检查清单

- [ ] 所有表都有主键
- [ ] 外键关系正确定义
- [ ] 敏感字段已标识并有加密方案
- [ ] 常用查询有索引支持
- [ ] 有软删除策略（如需要）
- [ ] 有审计字段
- [ ] ER 图已绘制

---

## 2. API 设计

### 2.1 RESTful 规范

| HTTP 方法 | 语义 | 幂等 | 示例 |
|-----------|------|:----:|------|
| GET | 查询 | ✅ | `GET /users/123` |
| POST | 创建 | ❌ | `POST /users` |
| PUT | 全量更新 | ✅ | `PUT /users/123` |
| PATCH | 部分更新 | ✅ | `PATCH /users/123` |
| DELETE | 删除 | ✅ | `DELETE /users/123` |

### 2.2 URL 设计规范

```
✅ 正确
GET  /api/v1/users              # 获取用户列表
GET  /api/v1/users/123          # 获取单个用户
POST /api/v1/users              # 创建用户
POST /api/v1/users/123/approve  # 操作类动作

❌ 错误
GET  /api/v1/getUsers           # 动词
GET  /api/v1/user               # 单数
POST /api/v1/users/create       # 冗余动词
```

### 2.3 统一响应格式

```json
// 成功响应
{
  "code": 0,
  "message": "success",
  "data": { ... },
  "timestamp": "2025-01-27T10:00:00Z",
  "request_id": "req_xxx"
}

// 分页响应
{
  "code": 0,
  "message": "success",
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "page_size": 20,
      "total": 100,
      "total_pages": 5
    }
  }
}

// 错误响应
{
  "code": 40001,
  "message": "参数校验失败",
  "errors": [
    { "field": "email", "message": "邮箱格式不正确" }
  ],
  "timestamp": "2025-01-27T10:00:00Z",
  "request_id": "req_xxx"
}
```

### 2.4 错误码设计

```
错误码格式: XXYYYY

XX: 类别
  00 - 成功
  40 - 客户端错误
  50 - 服务端错误

YYYY: 具体错误

示例:
  0     - 成功
  40000 - 通用参数错误
  40001 - 业务规则校验失败
  40100 - 未认证
  40101 - 用户名或密码错误
  40300 - 无权限
  40400 - 资源不存在
  50000 - 服务器内部错误
```

### 2.5 认证与权限

```python
# 权限装饰器设计模式
@router.get("/users")
@require_auth                              # 需要登录
@require_role(["ADMIN", "SUPER_ADMIN"])    # 需要角色
@require_permission("user_management")      # 需要权限
async def list_users():
    ...
```

### 2.6 API 设计检查清单

- [ ] URL 遵循 RESTful 规范
- [ ] 统一响应格式
- [ ] 统一错误码
- [ ] 认证方案明确
- [ ] 权限控制方案明确
- [ ] 分页规范明确
- [ ] 版本管理策略（`/api/v1`）

---

## 3. 分层架构设计

### 3.1 标准分层

```
┌─────────────────────────────────────────────────────────────┐
│                     Controller Layer                         │
│  • 接收请求、参数校验                                         │
│  • 调用 Service、返回响应                                     │
│  • 不包含业务逻辑                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      Service Layer                           │
│  • 业务逻辑实现                                               │
│  • 事务管理                                                   │
│  • 调用 Repository 和外部服务                                 │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   Repository    │ │ External Service│ │     Cache       │
│   (数据访问)     │ │   (外部服务)     │ │    (缓存)       │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

### 3.2 外部服务抽象

**必须抽象的外部服务：**

```python
# 邮件服务接口
class IEmailService(ABC):
    @abstractmethod
    async def send(self, to: str, subject: str, body: str) -> bool: ...

# 存储服务接口
class IStorageService(ABC):
    @abstractmethod
    async def upload(self, file: bytes, path: str) -> str: ...
    
    @abstractmethod
    async def download(self, path: str) -> bytes: ...
    
    @abstractmethod
    async def delete(self, path: str) -> bool: ...

# 训练服务接口（示例：特定业务）
class ITrainingService(ABC):
    @abstractmethod
    async def submit(self, task: TaskCreate) -> Task: ...
    
    @abstractmethod
    async def get_status(self, task_id: str) -> TaskStatus: ...
```

**配置切换实现：**

```yaml
# config/services.yaml
services:
  email:
    provider: "smtp"      # 可选: smtp, sendgrid, mock
  storage:
    provider: "local"     # 可选: local, minio, s3
  training:
    provider: "cli"       # 可选: cli, api
```

```python
# 工厂函数
def get_email_service(config: Config) -> IEmailService:
    match config.services.email.provider:
        case "smtp":
            return SMTPEmailService(config.services.email.smtp)
        case "sendgrid":
            return SendGridEmailService(config.services.email.sendgrid)
        case "mock":
            return MockEmailService()
```

---

## 4. 配置管理设计

### 4.1 配置分层

```
┌─────────────────────────────────────────────────────────────┐
│                       配置分层                               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  环境变量 (.env)              敏感配置                       │
│  ├── DATABASE_URL            ├── 数据库密码                 │
│  ├── JWT_SECRET              ├── JWT 密钥                   │
│  ├── SMTP_PASSWORD           ├── 第三方 API Key             │
│  └── ...                     └── 加密密钥                   │
│                                                             │
│  配置文件 (config/*.yaml)    非敏感配置                      │
│  ├── services.yaml           ├── 服务提供者选择             │
│  ├── business.yaml           ├── 业务参数                   │
│  ├── features.yaml           ├── 功能开关                   │
│  └── ...                     └── 默认值                     │
│                                                             │
│  代码常量 (constants.py)     枚举和映射                      │
│  ├── 枚举定义                 ├── FunctionType              │
│  ├── 显示名称映射             ├── FUNCTION_TYPE_NAMES       │
│  └── ...                     └── 类型安全                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 枚举与配置分离

**代码中定义枚举和名称（不变）：**

```python
# constants.py
class FunctionType(str, Enum):
    KWS = "KWS"
    COMMAND_CONTROL = "COMMAND_CONTROL"
    VAD = "VAD"  # 预留

FUNCTION_TYPE_NAMES = {
    FunctionType.KWS: {"zh": "唤醒词识别", "en": "Keyword Spotting"},
    FunctionType.COMMAND_CONTROL: {"zh": "命令词识别", "en": "Command Control"},
    FunctionType.VAD: {"zh": "人声检测", "en": "Voice Activity Detection"},
}
```

**配置文件控制启用状态（可变）：**

```yaml
# business.yaml
function_types:
  KWS:
    enabled: true
  COMMAND_CONTROL:
    enabled: true
  VAD:
    enabled: false  # 暂未上线
```

**扩展新功能只需改配置：**

```yaml
# 上线 VAD 功能
function_types:
  VAD:
    enabled: true  # 改为 true 即可
```

### 4.3 功能开关设计

```yaml
# features.yaml
features:
  # 模块开关
  modules:
    user_registration: true    # 用户自助注册
    email_notification: true   # 邮件通知
    model_testing: false       # 模型测试（P2）
    audit_logging: false       # 审计日志（P3）
  
  # 功能开关
  functions:
    sso_login: false           # SSO 登录
    multi_language: false      # 多语言
    api_rate_limit: true       # API 限流
```

**代码中使用功能开关：**

```python
if feature_flags.is_enabled("email_notification"):
    await email_service.send_notification(user, task)
```

---

## 5. 安全设计

### 5.1 安全检查清单

#### 认证安全

- [ ] 密码使用 bcrypt/Argon2 存储
- [ ] JWT 有合理的过期时间
- [ ] Refresh Token 使用 httpOnly Cookie
- [ ] 登录失败有次数限制和锁定机制
- [ ] 密码重置有安全流程

#### 传输安全

- [ ] 强制 HTTPS
- [ ] 配置 HSTS 头
- [ ] 禁用不安全的 TLS 版本

#### 数据安全

- [ ] 敏感数据加密存储
- [ ] 日志中不记录敏感信息
- [ ] API 响应不泄露敏感信息
- [ ] 数据库连接使用加密

#### API 安全

- [ ] 输入校验（防注入）
- [ ] CORS 配置正确
- [ ] 有请求限流
- [ ] 安全响应头配置

#### 权限安全

- [ ] 每个 API 都有权限检查
- [ ] 数据访问有越权检查
- [ ] 管理功能有审计日志

### 5.2 安全响应头

```python
# 推荐的安全响应头
security_headers = {
    "X-Content-Type-Options": "nosniff",
    "X-Frame-Options": "DENY",
    "X-XSS-Protection": "1; mode=block",
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains",
    "Content-Security-Policy": "default-src 'self'",
    "Referrer-Policy": "strict-origin-when-cross-origin",
}
```

---

## 6. 设计文档模板

### 6.1 数据库设计文档结构

```markdown
# 数据库设计文档

## 1. 设计概述
### 1.1 设计原则
### 1.2 命名规范
### 1.3 表清单

## 2. ER 图

## 3. 表结构详细设计
### 3.1 [模块名] 模块
#### 3.1.1 [表名] 表
- 表说明
- 字段定义 (DDL)
- 索引说明
- 关联关系

## 4. 数据字典
### 4.1 枚举值定义

## 5. 设计说明
### 5.1 主键策略
### 5.2 软删除策略
### 5.3 敏感数据处理
```

### 6.2 API 设计文档结构

```markdown
# API 设计文档

## 1. 设计原则
### 1.1 RESTful 规范
### 1.2 URL 命名规范
### 1.3 响应格式规范

## 2. 认证方案
### 2.1 认证流程
### 2.2 权限模型

## 3. API 接口定义
### 3.1 [模块名] 模块
#### 3.1.1 [接口名]
- 请求方法和 URL
- 请求参数
- 响应格式
- 错误码
- 示例

## 4. 错误码定义

## 5. 扩展点设计
### 5.1 功能开关
### 5.2 服务抽象
```

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2025-01-27 | 初始版本 |
