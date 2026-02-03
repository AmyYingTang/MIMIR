# Claude Code Prompt Skill

> **版本**: v2.4  
> **创建日期**: 2025-01-31  
> **最后更新**: 2025-02-04  
> **适用场景**: 使用 Claude Code 进行代码生成和项目实现  
> **前置要求**: 已完成系统设计阶段，有明确的技术规格

---

## 这个 Skill 是什么？

这是一套用于编写高质量 Claude Code Prompts 的方法论和模板。通过结构化的 Prompt 设计，实现：

- AI 主导执行流程
- 用户只需提供必要输入
- 每一步可验证
- 错误可处理

---

## 何时使用？

当你需要：
1. 让 Claude Code 帮你搭建项目骨架
2. 让 Claude Code 实现特定模块
3. 让 Claude Code 生成测试代码
4. 让 Claude Code 创建初始化脚本

---

## Prompt 结构模板

### 标准结构

```markdown
# Claude Code Prompt: [项目名] - [任务名]

## 你的角色

你是一个 [角色描述]。你的任务是 [任务目标]。

**重要原则**：
- 你主导整个流程
- 所有环境信息已提供，无需询问用户
- 自主完成所有操作
- 每完成一个阶段，汇报进度

---

## 环境信息（已确认）

以下信息已由用户提供，请直接使用：

- **[配置项 1]**：{{variable_name_1:default_value}}
- **[配置项 2]**：{{variable_name_2}}

---

## 第一步：[环境验证]

使用上述信息，自动执行检查：
- [前置条件 1]
- [前置条件 2]

---

## 第二步到第 N 步：[执行任务]

### 2.1 [子任务 1]

[具体的代码/文件内容/执行命令]

### 2.2 [子任务 2]

[具体的代码/文件内容/执行命令]

---

## 验证步骤

[验证任务完成的命令或检查项]

---

## 完成报告

所有步骤完成后，向用户汇报：

```
✅ [任务名] 完成！

📁 创建/修改的文件：
- [文件 1]
- [文件 2]

🧪 验证结果：
- [验证项 1]: ✅
- [验证项 2]: ✅

下一步：执行 [下一个 Prompt 文件名]
```

---

## 错误处理

如果遇到问题：

1. [常见错误 1]：[解决方案]
2. [常见错误 2]：[解决方案]

报告具体错误并提供解决方案。
```

---

## 模板变量规范（Agent 协作）

### 概述

Prompt 中使用 `{{variable}}` 格式的模板变量代替"向用户询问"。当通过 Agent 执行时，Agent 会在执行前一次性收集所有变量，填充后以非交互模式执行。

### 语法

| 格式 | 含义 | 示例 |
|------|------|------|
| `{{name}}` | 必填变量，无默认值 | `{{project_dir}}` |
| `{{name:default}}` | 带默认值的变量 | `{{mysql_host:localhost}}` |

### 命名约定

| 变量类型 | 命名规则 | 示例 |
|----------|----------|------|
| 数据库连接 | `mysql_host`, `mysql_port`, `mysql_user`, `mysql_password`, `db_name` | Agent 会自动触发连接测试 |
| Python 环境 | `python_cmd` | `{{python_cmd:python3}}` |
| 项目路径 | `project_dir` | `{{project_dir}}` |
| 应用配置 | 按业务含义命名 | `{{app_port:8000}}` |

> **重要**：使用标准命名（特别是 `mysql_*` 系列），Agent 能自动识别并在收集完后测试连接。

### 在 Prompt 中的使用方式

**正确做法（v2.0）**：
```markdown
## 环境信息（已确认）

- **Python 命令**：{{python_cmd:python3}}
- **项目目录**：{{project_dir}}
- **MySQL 主机**：{{mysql_host:localhost}}
- **MySQL 密码**：{{mysql_password}}
- **数据库名称**：{{db_name:voice_model_platform}}
```

**旧做法（v1.0，已弃用）**：
```markdown
## 第一步：环境信息收集

请向用户询问以下信息（一次性问完）：
...
**等待用户回复后再继续。**
```

### 变量应出现在哪里

模板变量不仅用于信息收集区域，也应出现在 Prompt 中所有引用这些值的地方：

```markdown
## 执行流程

### 阶段 1：创建项目
cd {{project_dir}}
mkdir -p voice-model-platform/backend

### 阶段 3：设置虚拟环境
{{python_cmd:python3}} -m venv venv
```

---

## 交互模式标记

### 概述

某些 Prompt 包含需要用户在执行过程中确认的危险操作（如数据库迁移、删除操作）。这些 Prompt 必须标记为交互模式，Agent 会使用不同的执行策略。

### 语法

在 Prompt 文件的**第一行**添加：

```markdown
<!-- agent:interactive -->
# Claude Code Prompt: ...
```

### 何时使用

| 场景 | 是否需要标记 | 说明 |
|------|:----------:|------|
| 创建文件/目录 | ❌ | 非破坏性操作 |
| 安装依赖 | ❌ | 非破坏性操作 |
| 数据库迁移 | ✅ | 修改数据库结构 |
| 删除操作 | ✅ | 不可逆操作 |
| 初始化脚本（建库、建表、初始数据） | ✅ | 修改数据库 |
| 修改生产配置 | ✅ | 影响线上环境 |

### 两种模式对比

| | 非交互模式（默认） | 交互模式 |
|--|---|---|
| **标记** | 无 | `<!-- agent:interactive -->` |
| **Agent 行为** | `claude -p --dangerously-skip-permissions` | `claude "prompt"` |
| **用户操作** | 无需介入 | 可能需要回答确认问题 |
| **适用场景** | 创建文件、安装依赖等 | 数据库操作、删除等 |

---

## 关键设计原则

### 1. 环境信息使用模板变量

**正确做法（v2.0）**：
```markdown
## 环境信息（已确认）

- **Python 命令**：{{python_cmd:python3}}
- **项目目录**：{{project_dir}}
- **MySQL 密码**：{{mysql_password}}
```

**过渡期做法（兼容手动执行）**：
```markdown
## 环境信息（已确认）

以下信息已由用户提供，请直接使用：

- **Python 命令**：{{python_cmd:python3}}
- **MySQL 密码**：{{mysql_password}}

> 手动执行时，请将 {{变量}} 替换为实际值
```

**旧做法（v1.0，已弃用）**：
```
我需要以下信息：
1. Python 命令是什么？
2. 项目目录在哪？
3. MySQL 连接信息？

请一次性提供。
```

### 2. 执行而非指导

**正确做法**：
```
## 执行步骤

获得用户信息后，自动执行：
1. 创建目录结构
2. 创建所有文件
3. 安装依赖
4. 运行验证
```

**错误做法**：
```
## 执行步骤

请按以下顺序操作：
1. 运行 mkdir 命令
2. 运行 pip install
3. 检查文件是否创建
```

### 3. 代码完整性

**正确做法**：
提供完整的、可直接使用的代码

**错误做法**：
- 提供代码片段让用户自己组装
- 使用 `...` 或 `# 其他代码` 省略关键部分

### 4. 错误处理前置

在 Prompt 末尾提供常见错误的处理方案，让 AI 能够自主解决问题。

---

## 任务分解指南

### 分解原则

1. **单一职责**：每个 Prompt 只做一件事
2. **可验证**：每个 Prompt 结束时有明确的验收标准
3. **顺序依赖**：后续 Prompt 依赖前序 Prompt 的产出
4. **渐进式**：从基础设施到业务逻辑

### 前置关卡：依赖决策门（DependencyResolutionGate）

在开始分解任务之前，Agent 必须检查当前模块是否存在跨模块依赖。如果存在未解决的依赖，**必须先阻塞分解**，产出 Dependency Resolution（DR）文档后才能继续。

**流程：**

1. **扫描依赖**：读取参考文档，识别当前模块依赖但不在本模块范围内的资源（如前序模块的数据库表、种子数据、服务接口等）
2. **评估状态**：每个依赖是"已就绪"还是"需决策"
3. **阻塞或放行**：
   - 所有依赖已就绪 → 直接进入分解
   - 存在"需决策"依赖 → 产出 DR 文档，列出每个依赖的决策选项（如 Mock、Seed、Skip），交由用户确认后再继续
4. **DR 文档格式**：每条记录包含 `DR-编号`、依赖描述、可选方案、推荐方案及理由

**来源**：s-1-2 模型训练模块任务分解实践。该模块依赖 s-1-1 的用户和芯片数据，如果不先解决"测试时用什么数据"的问题，分解出的 Prompt 会假设数据存在而在执行时失败。

---

### 质量原则（实践提炼）

以下 12 条原则从真实项目执行经验中提炼。编写或审查 Prompt 时，将其作为检查清单使用。

| # | 原则 | 说明 | 示例 |
|---|------|------|------|
| 1 | **ValidateRefs** | 所有引用的文档/文件必须存在且可访问 | Prompt 中写了"参考 database-design.md"，需验证文件确实在预期路径 |
| 2 | **PathAlign** | Prompt 中的产出文件路径必须与实际项目目录结构一致 | 不要写 `backend/models.py`，如果项目实际用的是 `backend/app/models/` |
| 3 | **ProgressSignals** | Prompt 间的进度信号必须显式声明 | Prompt N 的完成报告应说明 Prompt N+1 期望找到的内容 |
| 4 | **UserVerifyGuide** | Agent 验证步骤必须包含预期结果 | 不只是"运行 pytest"，而是"运行 pytest，期望 12 个测试通过，0 个失败" |
| 5 | **HostEnvAlign** | 命令必须与执行环境匹配 | 如果在 Docker 内运行，用 `docker compose exec backend pytest`，不是裸 `pytest` |
| 6 | **NamingConvention** | 文件命名规范必须在所有 Prompt 间保持一致 | 选定一种模式（如 `s-1-1-p01-xxx.md`）后全局统一 |
| 7 | **IdempotentPrompts** | Prompt 必须可安全重复运行 | 不阻塞前台进程（`uvicorn &` + 清理）；文件创建和种子数据使用 skip-if-exists 逻辑 |
| 8 | **UserAcceptGuide** | 每个 Prompt 需要用户手动验收步骤，超越 Agent 自动验证。**最终 Prompt 必须生成独立的用户验收指南文件**（如 `VERIFY-GUIDE.md`），用非技术语言写清楚用户该做什么，并在完成时提示用户打开该文件 | Agent 测试证明代码能跑；用户验收证明功能满足业务需求。最终 Prompt 完成后输出：`"请打开 VERIFY-GUIDE.md 按步骤验收"` |
| 9 | **ServiceDepChain** | 服务依赖链必须健壮 | 一个组件的配置错误不应级联影响（如错误的 healthcheck 路径不应阻止依赖服务启动） |
| 10 | **DiscrepancyReport** | 发现参考文档间不一致时，Agent 自行选择能让系统跑通的方案解决，但必须报告差异 + 决策逻辑 + 直接修补源文档。**特别注意跨 Prompt 的共享数据**（如测试用户凭据、端口号、数据库名） | 实例 1：DDL 中 status ENUM 只有 4 个值，但 state-machines.md 定义了 5 个状态 → Agent 以状态机为准，更新 DDL。实例 2：seed.py 设密码为 `Trainer@2025`，但 conftest.py 写死 `Test123456` → 24 个测试全部 ERROR，根因仅是一个密码字符串不一致 |
| 11 | **FullStackFix** | 修复 Prompt 必须列出**每一个受影响层**的改动（后端 API、前端调用、测试用例、旧端点清理、配置文件）。Agent 倾向于只修一层就停，导致前后端不同步 | 实例：后端新增 `GET /chips/available` 替代旧的 `/tasks/my-chips`，但 fix prompt 只改了后端。前端仍调旧端点 → 404。测试断言仍用旧字段 → 失败。正确做法：fix prompt 内显式列出 `backend/`, `frontend/`, `tests/`, `旧端点删除` 四个改动区块 |
| 12 | **InlineAPIContract** | Prompt 必须**内嵌精确的 API 请求/响应 JSON 模式**，而不是仅仅写"参考 api-design.md"。Agent 在生成大量代码时会偏离引用文档的细节（字段名、路径、嵌套结构），内嵌模式是唯一可靠的保真手段 | 实例：api-design.md 定义返回 `{chip_id, chip_model, available_functions: [{function_type, function_name}]}`，Prompt 只写"参考 api-design.md"。Agent 实际生成了 `{id, model, functions: ["KWS"]}` → 前端字段解析全部失败 |

### 推荐的分解粒度

| 任务类型 | 建议粒度 |
|----------|----------|
| 项目初始化 | 1 个 Prompt |
| 数据库模型 | 1 个 Prompt（每个模块） |
| 业务逻辑模块 | 1-2 个 Prompt（取决于复杂度） |
| API 端点 | 1 个 Prompt（每个模块） |
| 测试代码 | 1 个 Prompt（每个模块） |
| 初始化脚本 | 1 个 Prompt |

### 示例：用户认证模块

```
Prompt 01: 项目初始化（搭骨架）
    ↓
Prompt 02: 数据库模型（User, Role, RefreshToken）
    ↓
Prompt 03: 安全模块（JWT, bcrypt）
    ↓
Prompt 04: 认证 API（4 个端点）
    ↓
Prompt 05: API 测试
    ↓
Prompt 06: 初始化脚本（建库 + 建表 + 初始数据）
```

---

## 验收标准模板

每个 Prompt 应明确验收标准：

```markdown
## 验收标准

- [ ] [文件/目录] 已创建
- [ ] [命令] 执行成功
- [ ] [测试] 通过
- [ ] [服务] 可访问
- [ ] [接口] 返回预期结果
```

---

## 与其他 Skill 的关系

| Skill | 关系 |
|-------|------|
| project-kickoff/enterprise-web | 本 Skill 用于实现其 Phase 4（代码实现） |
| meta-knowledge | 可从 Claude Code 实践中提炼经验 |

---

## 案例参考

参见：`templates/` 目录下的实际案例

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2025-01-31 | 初始版本，基于用户认证模块实践验证 |
| v2.0 | 2025-02-01 | 重大更新：模板变量 `{{variable}}` 替代手动输入收集；交互模式标记 `<!-- agent:interactive -->` 支持危险操作确认；Agent 连接测试变量命名约定 |
| v2.1 | 2025-02-02 | 新增 9 条任务分解质量原则（ValidateRefs、PathAlign、ProgressSignals、UserVerifyGuide、HostEnvAlign、NamingConvention、IdempotentPrompts、UserAcceptGuide、ServiceDepChain），基于 s-1-1 执行经验提炼 |
| v2.2 | 2025-02-03 | 新增前置关卡 DependencyResolutionGate（依赖决策门）；新增第 10 条质量原则 DiscrepancyReport（文档差异报告），基于 s-1-2 任务分解经验提炼 |
| v2.3 | 2025-02-03 | 补充 UserAcceptGuide（#8）交付形式要求：最终 Prompt 必须生成独立的用户验收指南文件，基于 s-1-2 执行后验收经验 |
| v2.4 | 2025-02-04 | 新增第 11 条 FullStackFix（修复必须覆盖全链路每一层）和第 12 条 InlineAPIContract（Prompt 必须内嵌精确 API 模式），基于 s-1-2 前后端联调验证经验 |
