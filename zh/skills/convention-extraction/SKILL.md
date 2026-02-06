# Skill: Convention Extraction — 跨模块一致性

> **版本**: v0.2  
> **创建日期**: 2026-02-04  
> **最后更新**: 2026-02-05  
> **分类**: 构建（Prompt 前置）  
> **运行时**: Claude Code CLI 或手动提取

---

## 目的

当项目有多个模块按顺序构建时，第一个模块会建立隐式约定——命名模式、架构分层、错误处理风格、共享数据格式——这些是设计文档不会规定的。如果不捕获这些约定，后续模块会"重新发明"一套不同的做法，导致不一致，修复成本很高。

本 skill 在每个模块完成后从现有代码中提取**约定快照（Convention Snapshot）**，生成一份紧凑的文档，作为下一个模块 prompt 生成的输入。

**核心洞察**：设计文档定义*契约*（做什么）。代码建立*约定*（怎么做）。契约在编码前就指定了；约定在实现过程中涌现。两者都必须跨模块一致，但只有契约有显式的真实来源。本 skill 为约定创建显式的真实来源。

---

## 何时使用

```
模块1:  设计文档 ──────────────→ prompt → 代码 → ★ 提取 snapshot v1 ★
模块2:  设计文档 + snapshot v1 → prompt → 代码 → ★ 更新 snapshot v2 ★
模块3:  设计文档 + snapshot v2 → prompt → 代码 → ★ 更新 snapshot v3 ★
```

在**每个模块完成并验收后**、为下一个模块生成 prompt 之前触发。

**重要**：模块 1 没有约定问题——它*就是*约定的来源。提取从模块 1 完成后开始。

---

## 本 Skill 解决的问题

### 契约 vs 约定

| 方面 | 契约（设计文档） | 约定（代码） |
|------|------------------|--------------|
| **何时定义** | 编码之前 | 编码过程中 |
| **示例** | "POST /api/training/start" | "所有 router 函数命名为 动词_名词" |
| **谁指定** | 架构师/设计者 | 第一个开发者（或第一个 AI agent） |
| **真实来源** | 设计文档 | 第一个模块的代码 |
| **缺失时会怎样** | review-agent 在构建后捕获 | 到集成时才发现，没人捕获 |

### 为什么设计文档不能覆盖这些

设计文档理应停留在接口层面。它们不应该规定：

- 内部变量命名模式
- 模块内部的服务层组织方式
- 错误处理的具体实现
- 前端组件结构约定
- 工具函数的模式和位置
- import 路径约定
- 测试 fixture 组织方式

这些决策在实现过程中做出，必须跨模块保持一致，但提前规定会过于僵化。它们需要在**涌现之后提取**，而非在之前规定。

### 没有约定提取会怎样

```
模块1 (auth):     useAuthStore, /api/auth/*, AppException, Depends(get_current_user)
模块2 (training): trainingStore, /training/*, HTTPException, @require_auth decorator

两种选择都合理。都能通过测试。但项目已经不一致了。
```

---

## 提取维度

快照按 5 个维度组织。每个维度有一组问题，Claude Code 通过扫描现有代码来回答。

### 维度 1：结构约定

提取内容：

```
- 后端分层组织（router → service → repository？还是其他？）
- 前端组件组织（按功能？按页面？按类型？）
- 共享类型/枚举定义的位置
- 测试文件位置和命名模式
- 配置文件组织方式
```

输出示例：

```yaml
structure:
  backend_layers: "routers/ → services/ → repositories/ → models/"
  frontend_components: "views/{module}/ 按页面组织, components/ 放共享组件"
  shared_enums: "后端: app/common/enums.py | 前端: src/types/enums.ts"
  test_location: "tests/{module}/ 镜像 src 结构"
  config: ".env 放密钥, app/core/config.py 放应用配置"
```

### 维度 2：命名约定

提取内容：

```
- API router 函数命名（动词_名词？名词_动词？其他？）
- 前端 store 命名（useXxxStore？xxxStore？其他？）
- Celery/后台任务命名
- 数据库 model 类名 ↔ 表名映射
- API 路由路径模式（/api/v1/{module}/{action}?）
- 前端路由路径模式
```

输出示例：

```yaml
naming:
  router_functions: "动词_名词 — create_user, get_training_status, delete_model"
  stores: "use{Module}Store — useAuthStore, useTrainingStore"
  celery_tasks: "{module}_{action}_task — training_start_task"
  db_models: "PascalCase 类名, snake_case 表名 — class TrainingTask → training_tasks"
  api_paths: "/api/{module}/{resource} — /api/auth/login, /api/training/tasks"
  frontend_routes: "/{module}/{action} — /training/new, /models/list"
```

### 维度 3：模式约定

提取内容：

```
- 错误处理方式（自定义异常？HTTP 异常？错误码？）
- 认证注入方式（Depends？中间件？装饰器？）
- 分页实现方式
- 前端 API 调用模式（集中 client？每模块单独？axios 拦截器？）
- 表单验证方式（前端、后端还是两者？）
- 前端加载/错误状态管理
```

输出示例：

```yaml
patterns:
  error_handling: |
    自定义 AppException(error_code, message, status_code)
    → 全局 exception_handler 转换为 {"detail": str, "error_code": str}
  auth_injection: "Depends(get_current_user) 作为 router 参数，返回 User 对象"
  pagination: "CommonPaginationParams 依赖注入，返回 PaginatedResponse[T]"
  api_client: "src/api/client.ts — 所有请求通过 apiClient 发出，自动附加 Bearer token"
  form_validation: "后端: Pydantic models。前端: Element Plus form rules"
  loading_states: "每个 store 有 loading 标志，组件检查 store.loading"
```

### 维度 4：共享接口约定

提取内容：

```
- 日期时间格式和时区
- ID 格式（UUID 版本、字符串 vs 原生）
- API 响应包装结构
- 状态枚举的字符串值及其定义位置
- 文件上传/下载模式
- WebSocket 消息格式（如适用）
```

输出示例：

```yaml
shared_interfaces:
  datetime: "ISO 8601, UTC, 以字符串传输"
  id_format: "UUID v4, 以字符串传输"
  response_wrapper: "直接返回模型，无包装。错误使用标准格式。"
  status_enums: "定义在后端 enums.py，前端 enums.ts 中镜像"
  file_download: "GET 带 ?token= 查询参数，返回文件流"
```

### 维度 5：基础设施约定

提取内容：

```
- Docker 服务命名
- 环境变量命名模式
- 端口分配模式
- 数据库迁移方式
- 日志格式和级别
- 健康检查模式
```

输出示例：

```yaml
infrastructure:
  docker_services: "project-{service} — project-backend, project-frontend, project-db"
  env_vars: "UPPER_SNAKE — DB_HOST, REDIS_URL, JWT_SECRET_KEY"
  ports: "Backend 8000, Frontend 3000, DB 3306, Redis 6379"
  migrations: "Alembic auto-generate, 每个功能一个迁移"
  logging: "Python logging, 生产环境 JSON 格式, 开发环境人类可读"
  healthcheck: "GET /api/health 返回 {status: 'ok'}"
```

---

## 输出格式

快照保存为项目根目录（或设计文档文件夹）中的 `project-conventions.md`。

```markdown
# 项目约定快照

> **项目**: [项目名称]
> **提取自**: 模块 [N] 完成
> **最后更新**: [日期]
> **版本**: [N]（每个模块递增）

## 结构约定
[提取内容]

## 命名约定
[提取内容]

## 模式约定
[提取内容]

## 共享接口约定
[提取内容]

## 基础设施约定
[提取内容]

## 变更日志
| 版本 | 模块完成后 | 变更内容 |
|------|------------|----------|
| v1 | 认证 (s-1-1) | 初始提取 |
| v2 | 训练 (s-1-2) | 添加 Celery 任务命名、文件下载模式 |
```

**体积目标**：快照应控制在 **200 行以内**。它捕获的是决策，不是代码。如果越来越长，说明包含了过多细节。

---

## 如何与 Prompt 生成集成

为模块 N+1 生成 prompt 时，添加以下部分：

```markdown
## 项目约定

本项目在之前的模块中已建立以下约定。
你必须遵循这些约定以保持跨模块一致性。

[粘贴或引用 project-conventions.md 内容]

做实现决策时，先检查此列表。
如果某个决策已在这里覆盖，遵循现有约定。
如果某个决策未在这里覆盖，做出合理选择并在实现总结末尾的
"新约定"部分记录下来。
```

这创建了一个反馈循环：每个模块既**消费**又**产生**约定数据。

---

## 提取方法

### 方法 A：Claude Code 扫描（推荐）

向 Claude Code 提供提取 prompt 模板（见下方），让它扫描代码库。

**提取 Prompt 模板**：

```markdown
# 约定提取任务

## 你的角色
你是一个代码约定分析师。扫描现有代码库，将隐式约定提取为结构化快照。

## 扫描范围
- [后端/前端目录] 中的所有源文件
- 配置文件
- 测试文件
- Docker 和基础设施文件

## 提取维度
[粘贴本 SKILL.md 中的 5 个维度]

## 输出
按照上述指定格式生成 `project-conventions.md`。
只包含你能从实际代码中验证的约定——不要猜测或推断尚未明确建立的约定。

## 规则
- 如果某个模式只出现在一个地方，记录但标记为"暂定"
- 如果存在冲突的模式，标记为不一致
- 每个条目最多 1-2 行
- 使用代码中的实际示例
```

### 方法 B：复盘后手动提取

在模块复盘时，开发者审查代码并手动填写快照模板。不如方法 A 全面，但对小型项目更简单。

### 方法 C：Review Agent 集成

review-agent 可以扩展为在审查的同时**输出约定观察结果**。当它扫描模块 N 的代码对照设计文档时，可以同步记录观察到的实现模式。

---

## 与其他 MIMIR Skill 的关系

```
project-kickoff          → 定义契约（设计文档）
claude-code-prompt       → 生成 prompt（消费快照）
★ convention-extraction  → 提取约定（桥接缺口）
review-agent             → 验证契约 + 约定
retro                    → 完善提取维度
```

| Skill | 关系 |
|-------|------|
| **claude-code-prompt** | 下游消费者。prompt 中包含快照作为"项目约定"部分 |
| **review-agent** | 互补。Review 检查契约；约定检查实现模式。可通过方法 C 共享数据 |
| **retro** | 反馈循环。复盘中发现的新不一致模式 → 新的提取维度 |
| **project-kickoff** | 上游。设计文档定义约定不覆盖的范围 |

---

## 使用模式：Convention 作为跨模块修复队列

除了作为一致性参考之外，convention snapshot 还天然充当**跨模块的修复队列**。

### 模式描述

当 review-agent 或人工审查发现不一致时（如同一个常量在三处重复定义），这些不一致记入 convention snapshot 的 inconsistency 部分。下一个模块在任务分解时扫描这份列表，如果新模块恰好会接触相关代码，就"顺手"在该模块中修复——零额外成本的闭环。

```
模块 N review → 发现不一致（如 FUNCTION_TYPE_NAMES 三处重复）
    ↓ 记入 convention snapshot inconsistency 列表
模块 N+1 分解 → 扫描 inconsistency 列表
    ↓ 此模块恰好要建 constants.py
模块 N+1 执行 → 建立权威定义 + 清理重复 → 闭环
```

### 关键原则

- **谁第一个碰到这块代码，谁就顺手修**。不需要单独的"修复冲刺"。
- Inconsistency 列表是**追加写入**的：review 发现新的就加进去，模块修复了的就标记为已解决。
- 如果某条 inconsistency 跨了多个模块都没人碰到，它会在列表中积累——这本身就是一个信号，说明可能需要专门的修复 prompt。

### 在 snapshot 中的记录格式

在 `project-conventions.md` 中增加一个 inconsistency 区域：

```markdown
## 已知不一致（待修复）

| ID | 描述 | 发现于 | 涉及文件 | 状态 |
|----|------|--------|----------|------|
| INC-001 | FUNCTION_TYPE_NAMES 在 enums.py, seed.py, constants.py 三处重复 | s-1-2 review | backend/app/ | ✅ s-2-1 P01 修复 |
| INC-002 | 日期格式 ISO vs Unix timestamp 混用 | s-1-2 review | api/, frontend/ | ⬜ 待修复 |
```

### 来源

s-1-2 review 发现 `FUNCTION_TYPE_NAMES` 三处重复定义 → 记入 conventions inconsistency → s-2-1 P01 正好需要建 `constants.py` 权威定义 → 顺手清理三处重复。零额外成本的闭环。这验证了 convention 文档不只是记录，它是跨模块的自然修复队列。

---

## 反模式

| 反模式 | 为什么是错的 | 应该怎么做 |
|--------|-------------|-----------|
| **在设计文档中规定约定** | 过于僵化、杂乱 | 让约定在模块 1 中涌现，然后提取 |
| **把快照当成代码 lint 规则** | 太死板，遗漏语义模式 | 保持为人类/AI 可读的指导 |
| **在模块验收前更新快照** | 有问题的代码中的约定不可靠 | 仅从已验收的、能工作的代码中提取 |
| **包含实现细节** | 快照太大无法嵌入 prompt | 捕获决策和模式，而非代码 |
| **"小"模块就跳过提取** | 小模块仍然建立模式 | 始终提取；小模块 = 小的快照增量 |

---

## 版本历史

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| v0.1 | 2026-02-04 | 初始版本。5 个提取维度、3 种提取方法、prompt 模板 |
| v0.2 | 2026-02-05 | 新增"使用模式：Convention 作为跨模块修复队列"，基于 s-1-2 review → s-2-1 闭环修复的实践验证 |
