# MIMIR - Bilingual Edition

> **M**ethodology for **I**mplementation, **M**anagement & **I**ntelligent **R**eference

*Like Odin consulting the guardian of the Well of Wisdom, consult MIMIR for your next project.*

---

## 🌍 Language / 语言

This repository contains MIMIR methodology in two languages:

| Language | Directory | Status |
|----------|-----------|--------|
| **English** | [`/en`](./en/) | ✅ Complete |
| **中文 (Chinese)** | [`/zh`](./zh/) | ✅ Complete |

---

## Quick Start

### English Users
→ Start with [`/en/MIMIR-README.md`](./en/MIMIR-README.md)

### 中文用户
→ 从 [`/zh/MIMIR-README.md`](./zh/MIMIR-README.md) 开始

---

## What is MIMIR?

MIMIR is an **executable software project startup methodology** designed for AI Agents (like Claude) and human developers.

| Traditional Methodology | MIMIR |
|-------------------------|-------|
| Static knowledge that gets outdated | Continuously updated through retrospectives |
| Scattered concepts | Structured checklists + decision trees |
| Read it all, still don't know what to do | Directly produces project documentation |

---

## Structure Overview

```
MIMIR/
├── en/                          # English version
│   ├── MIMIR-README.md          # Entry point
│   ├── CORE-PRINCIPLES.md       # Core design principles (v1.1)
│   └── skills/
│       ├── SKILL-INDEX.md
│       ├── project-kickoff/
│       │   ├── SKILL.md
│       │   ├── enterprise-web/  # Enterprise Web guide
│       │   └── templates/       # Document templates
│       ├── claude-code-prompt/  # Claude Code Prompt design (v2.0)
│       │   ├── SKILL.md
│       │   └── templates/       # Prompt templates
│       ├── meta-knowledge/      # Meta-knowledge extraction
│       │   └── SKILL.md
│       └── retro/               # Retrospective tools
│
└── zh/                          # 中文版本
    ├── MIMIR-README.md          # 入口
    ├── CORE-PRINCIPLES.md       # 核心设计原则 (v1.1)
    └── skills/
        ├── SKILL-INDEX.md
        ├── project-kickoff/
        │   ├── SKILL.md
        │   ├── enterprise-web/  # 企业级 Web 指南
        │   └── templates/       # 文档模板
        ├── claude-code-prompt/  # Claude Code Prompt 设计 (v2.0)
        │   ├── SKILL.md
        │   └── templates/       # Prompt 模板
        ├── meta-knowledge/      # 元知识提炼
        │   └── SKILL.md
        └── retro/               # 复盘工具
```

---

## How to Use

### With Claude (Recommended)

1. Create a Project at [Claude.ai](https://claude.ai)
2. Upload files from your preferred language folder
3. Start conversation:

**English:**
```
I want to start a new project. Please use MIMIR to help me with project planning.

Project brief: [Describe your project]
```

**中文:**
```
我要启动一个新项目，请用 MIMIR 帮我进行项目规划。

项目简介：[描述你的项目]
```

---

## Currently Supported

| Project Type | Status |
|--------------|:------:|
| Enterprise Web Projects | ✅ |
| Claude Code Prompt Design | ✅ 🆕 |
| Meta-Knowledge Extraction | ✅ |
| Mobile Apps | 🚧 Planned |
| CLI Tools | 🚧 Planned |
| Data Pipelines | 🚧 Planned |

---

## Contributing

MIMIR evolves through project retrospectives. Welcome to:

1. **Use and provide feedback** - Share your experience in Issues
2. **Contribute retrospectives** - Use the retro guide, submit PRs
3. **Add translations** - Help translate to other languages
4. **Extend project types** - Contribute Skills for mobile, CLI, etc.

---

## License

MIT

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.5 | 2025-02-01 | Claude Code Prompt Skill v2.0: template variables, interactive mode marker, connection testing; Core Principles v1.1: added "Validate Inputs Early" |
| v1.4 | 2025-01-31 | Added Core Principles and Claude Code Prompt Skill, based on Task Decomposition validation |
| v1.3 | 2025-01-30 | Added Meta-Knowledge Extraction Skill for AI collaboration insights |
| v1.2 | 2025-01-28 | Added document consistency management templates |
| v1.1 | 2025-01-27 | Added testing strategy and documentation delivery phases |
| v1.0 | 2025-01-27 | Initial version, extracted from real enterprise project experience |
