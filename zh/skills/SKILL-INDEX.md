# 软件项目启动方法论 - Skill 体系

> **版本**: v1.8  
> **创建日期**: 2025-01-27  
> **维护方式**: 通过项目复盘持续更新  
> **适用对象**: AI Agent (如 Claude) 或人类开发者/架构师

---

## 这是什么？

这是一套**可执行的软件项目启动方法论**，不同于传统书籍：

| 传统方法论书籍 | 本 Skill 体系 |
|----------------|---------------|
| 知识是静态的、会过时 | 通过项目复盘持续更新 |
| 知识是分散的、无关联 | 结构化的 checklist + 决策树 |
| 需要读者自己判断适用性 | 通过提问自动匹配适用场景 |
| 读完还是不知道怎么做 | 直接产出项目文档 |

---

## 如何使用？

### 场景 A：启动新项目

```
1. 告诉我："我要启动一个新项目"
2. 我会问你几个分类问题（项目类型、规模、约束等）
3. 根据你的回答，我会加载对应的 Skill 文档
4. 按照 Skill 指导，我会主动提问、给出建议、产出文档
```

### 场景 B：项目复盘更新方法论

```
1. 告诉我："我要做项目复盘"
2. 我会引导你回顾项目中的关键决策和踩坑点
3. 提炼出可复用的经验
4. 更新到对应的 Skill 文档中
```

---

## Skill 生命周期阶段

MIMIR 的 Skill 对应开发生命周期的各个阶段：

```
🔵 规划          →  🟢 构建            →  🟡 验证          →  🟣 反思        →  ⚪ 复盘
project-kickoff     claude-code-prompt      review-agent        meta-knowledge      retro
```

并非所有阶段都是必需的，但它们的顺序代表了开发的自然流程。

---

## Skill 体系结构

```
skills/
├── SKILL-INDEX.md                          # 📍 你在这里 - 入口文档
│
│ ── 🔵 规划 ──────────────────────────────────────────────────────────────────
│
├── project-kickoff/                        # 项目启动方法论
│   ├── SKILL.md                            # 主文档 - 项目分类决策树
│   │
│   ├── enterprise-web/                     # 企业级 Web 项目
│   │   ├── SKILL.md                        # ⭐ 主指南
│   │   ├── phase-1-requirements.md         # 需求分析阶段
│   │   ├── phase-2-tech-selection.md       # 技术选型阶段
│   │   ├── phase-3-system-design.md        # 系统设计阶段
│   │   ├── phase-3-ui-design-principles.md # UI/UX 设计原则
│   │   ├── phase-4-testing.md              # 测试策略阶段
│   │   ├── phase-5-documentation.md        # 文档交付阶段
│   │   └── checklists/                     # 检查清单
│   │       ├── security-checklist.md
│   │       ├── production-readiness.md
│   │       └── enterprise-concerns.md
│   │
│   ├── mobile-app/                         # 移动端 App（未来扩展）
│   │   └── SKILL.md
│   │
│   ├── cli-tool/                           # CLI 工具（未来扩展）
│   │   └── SKILL.md
│   │
│   └── templates/                          # 文档模板
│       ├── prd-template.md
│       ├── tech-selection-template.md
│       ├── database-design-template.md
│       ├── api-design-template.md
│       ├── project-control-template.md
│       ├── doc-dependencies-template.md
│       └── change-review-checklist-template.md
│
│ ── 🟢 构建 ──────────────────────────────────────────────────────────────────
│
├── claude-code-prompt/                     # Claude Code Prompt 设计
│   ├── SKILL.md                            # Prompt 结构、质量原则、任务分解
│   └── templates/                          # Prompt 模板
│       └── 01-project-init-template.md
│
│ ── 🟡 验证 ──────────────────────────────────────────────────────────────────
│
├── review-agent/                           # 🆕 独立代码审查（质量保障）
│   └── SKILL.md                            # 审查维度、设计原则、报告格式
│                                           # 运行时: MIMIR-BO review-agent/
│
│ ── 🟣 反思 ──────────────────────────────────────────────────────────────────
│
├── meta-knowledge/                         # 元知识提炼
│   └── SKILL.md                            # 从 AI 协作中提取可复用洞察
│
│ ── ⚪ 复盘 ──────────────────────────────────────────────────────────────────
│
└── retro/                                  # 复盘萃取工具
    ├── RETRO-GUIDE.md                      # 复盘引导文档
    ├── RETRO-TEMPLATE.md                   # 复盘记录模板
    ├── retro-doc-consistency.md            # 文档一致性复盘记录
    └── retro-integration-testing-download.md  # 集成测试复盘记录
```

---

## 当前可用的 Skill

| Skill | 阶段 | 状态 | 适用场景 |
|-------|:----:|:----:|----------|
| **企业级 Web 项目** | 🔵 规划 | ✅ 可用 | B2B SaaS、内部管理系统、平台型产品 |
| **Claude Code Prompt 设计** | 🟢 构建 | ✅ 可用 | 为 AI 驱动的代码生成设计 Prompt |
| **独立代码审查** | 🟡 验证 | ✅ 可用 | 构建完成后对照设计文档验证代码一致性。运行时在 MIMIR-BO 中 |
| **元知识提炼** | 🟣 反思 | ✅ 可用 | 从 AI 协作中提取可复用洞察 |
| 移动端 App | 🔵 规划 | ⬜ 计划中 | iOS/Android 原生或跨平台 |
| CLI 工具 | 🔵 规划 | ⬜ 计划中 | 命令行工具、脚本 |
| 数据管道 | 🔵 规划 | ⬜ 计划中 | ETL、数据处理 |

---

## 快速开始

**如果你要启动一个新项目，请说：**

> "我要启动一个新项目，请帮我进行项目规划"

**如果你完成了一个项目想要复盘，请说：**

> "我刚完成一个项目，想做复盘并更新方法论"

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2025-01-27 | 初始版本，基于真实企业级项目经验提炼 |
| v1.1 | 2025-01-27 | 添加测试策略(phase-4-testing.md)和文档交付(phase-5-documentation.md)阶段 |
| v1.2 | 2025-01-28 | 添加文档一致性管理模板 (doc-dependencies-template.md, change-review-checklist-template.md) |
| v1.3 | 2025-01-30 | 添加元知识提炼 Skill (meta-knowledge/)，从 AI 协作中提取可复用洞察 |
| v1.4 | 2025-01-31 | 添加核心原则 (CORE-PRINCIPLES.md) 和 Claude Code Prompt Skill (claude-code-prompt/)，基于任务分解验证实践。*注：发布时未同步更新 SKILL-INDEX 结构树* |
| v1.5 | 2025-02-01 | Claude Code Prompt Skill v2.0：模板变量、交互模式标记、连接测试；核心原则 v1.1：新增"尽早验证输入" |
| v1.6 | 2025-02-01 | 添加 UI/UX 设计原则 (phase-3-ui-design-principles.md)：向导式交互、角色分层体验、配置驱动 UI 适配 |
| v1.7 | 2025-02-02 | claude-code-prompt v2.1：9 条任务分解质量原则；enterprise-web phase-2 v1.1：全容器化原则 + Healthcheck 路径对齐 |
| v1.8 | 2025-02-04 | 添加 Review Agent skill（独立代码审查）。引入生命周期阶段：规划 → 构建 → 验证 → 反思 → 复盘。同步结构树以反映所有现有 skill（claude-code-prompt、meta-knowledge、review-agent、retro）。回溯补录 v1.4–v1.7 版本历史 |
