# 软件项目启动方法论 - Skill 体系

> **版本**: v1.0  
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

## Skill 体系结构

```
skills/
├── SKILL-INDEX.md                          # 📍 你在这里 - 入口文档
│
├── project-kickoff/                        # 项目启动方法论
│   ├── SKILL.md                            # 主文档 - 项目分类决策树
│   │
│   ├── enterprise-web/                     # 企业级 Web 项目
│   │   ├── SKILL.md                        # ⭐ 主指南
│   │   ├── phase-1-requirements.md         # 需求分析阶段
│   │   ├── phase-2-tech-selection.md       # 技术选型阶段
│   │   ├── phase-3-system-design.md        # 系统设计阶段
│   │   ├── phase-4-testing.md              # 🆕 测试策略阶段
│   │   ├── phase-5-documentation.md        # 🆕 文档交付阶段
│   │   └── checklists/                     # 检查清单
│   │       ├── security-checklist.md       # 安全检查清单
│   │       ├── production-readiness.md     # 生产就绪检查清单
│   │       └── enterprise-concerns.md      # 企业级关注点
│   │
│   ├── mobile-app/                         # 移动端 App（未来扩展）
│   │   └── SKILL.md
│   │
│   ├── cli-tool/                           # CLI 工具（未来扩展）
│   │   └── SKILL.md
│   │
│   └── templates/                          # 文档模板
│       ├── prd-template.md                 # PRD 模板
│       ├── tech-selection-template.md      # 技术选型模板
│       ├── database-design-template.md     # 数据库设计模板
│       ├── api-design-template.md          # API 设计模板
│       └── project-control-template.md     # 项目控制文档模板
│
└── retro/                                  # 复盘萃取工具
    ├── RETRO-GUIDE.md                      # 复盘引导文档
    └── RETRO-TEMPLATE.md                   # 复盘记录模板
```

---

## 当前可用的 Skill

| Skill | 状态 | 适用场景 |
|-------|:----:|----------|
| **企业级 Web 项目** | ✅ 可用 | B2B SaaS、内部管理系统、平台型产品 |
| 移动端 App | ⬜ 计划中 | iOS/Android 原生或跨平台 |
| CLI 工具 | ⬜ 计划中 | 命令行工具、脚本 |
| 数据管道 | ⬜ 计划中 | ETL、数据处理 |

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
