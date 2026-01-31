# MIMIR

> **M**ethodology for **I**mplementation, **M**anagement & **I**ntelligent **R**eference

*Like Odin consulting the guardian of the Well of Wisdom, consult MIMIR for your next project.*

---

## What is this?

MIMIR is an **executable software project startup methodology** designed for AI Agents (like Claude) and human developers.

It's not another methodology book full of theory—it's:

| Traditional Methodology | MIMIR |
|-------------------------|-------|
| Static knowledge that gets outdated | Continuously updated through project retrospectives |
| Scattered concepts | Structured checklists + decision trees |
| Read it all, still don't know what to do | Directly produces project documentation |
| Need to judge applicability yourself | Automatically matches scenarios through questioning |

## Core Philosophy

```
Real project experience ──► Extract methodology ──► Guide new projects ──► Retrospective updates methodology
      ▲                                                                            │
      └────────────────────────────────────────────────────────────────────────────┘
```

The name MIMIR comes from **the guardian of the Well of Wisdom** in Norse mythology. Odin sacrificed one of his eyes to gain his wisdom. This methodology is also earned through real project pitfalls.

---

## What's Included?

```
MIMIR/
├── SKILL-INDEX.md                      # 📍 Entry point - Start here
│
├── project-kickoff/                    # Project startup methodology
│   ├── SKILL.md                        # Project classification decision tree
│   │
│   ├── enterprise-web/                 # 🏢 Enterprise Web Projects
│   │   ├── SKILL.md                    # Main guide
│   │   ├── phase-1-requirements.md     # Requirements analysis
│   │   ├── phase-2-tech-selection.md   # Technology selection
│   │   ├── phase-3-system-design.md    # System design
│   │   ├── phase-4-testing.md          # Testing strategy
│   │   ├── phase-5-documentation.md    # Documentation delivery
│   │   └── checklists/                 # Checklists
│   │
│   └── templates/                      # Document templates
│       ├── prd-template.md
│       ├── tech-selection-template.md
│       └── project-control-template.md
│
└── retro/                              # Retrospective extraction tool
    └── RETRO-GUIDE.md
```

---

## Quick Start

### Method 1: Use with Claude (Recommended)

1. Create a Project at [Claude.ai](https://claude.ai)
2. Upload MIMIR files to the Project
3. Start the conversation:

```
I want to start a new project. Please use MIMIR to help me with project planning.

Project brief: [Describe your project]
```

Claude will follow MIMIR guidelines to ask questions, give suggestions, and produce documentation.

### Method 2: Direct Human Use

1. Start from `SKILL-INDEX.md`
2. Enter the corresponding Skill directory based on your project type
3. Follow the phases in order, completing deliverables at each stage

---

## Currently Supported Project Types

| Type | Status | Applicable Scenarios |
|------|:------:|----------------------|
| **Enterprise Web Projects** | ✅ | B2B SaaS, internal management systems, platform products, multi-user systems |
| Mobile Apps | 🚧 | iOS/Android native or cross-platform |
| CLI Tools | 🚧 | Command-line tools, scripts |
| Data Pipelines | 🚧 | ETL, data processing |

---

## Phases Covered for Enterprise Web Projects

```
Phase 1        Phase 2        Phase 3        Phase 4        Phase 5        Phase 6
Requirements → Tech          → System       → Code         → Testing      → Documentation
Analysis       Selection      Design         Implementation  Strategy       Delivery
   │              │              │              │              │              │
   ▼              ▼              ▼              ▼              ▼              ▼
  PRD         Tech Stack     Database      Code Repo      Test Cases    User Manual
 User Roles    Document       Design                      E2E Checklist  Admin Manual
 Priorities   Architecture   API Design                                 Deployment Docs
```

Each phase includes:
- ✅ Must-answer question checklists
- ✅ Deliverable templates
- ✅ Verification checklists
- ✅ Common pitfall reminders

---

## Key Features

### 🎯 Decision Tree Driven

Instead of giving you a pile of knowledge to sort through yourself, it helps you make decisions through questions:

```
Q: Expected user scale?
   < 100      → Monolith, single server
   100-1K     → Monolith, consider read/write separation
   1K-10K     → Load balancing, caching
   > 10K      → Microservices, container orchestration
```

### 📋 Checklists Prevent Oversights

Reminders for commonly overlooked points in enterprise projects:

- Password policies, login protection, session management
- Sensitive data encryption, log masking
- Audit trails, data retention
- Error handling, timeout control, rate limiting

### 🔄 Retrospective-Driven Updates

After each project, extract experience through retrospectives:

```
Project Retrospective → Extract reusable experience → Update MIMIR → Next project benefits
```

### 🤖 AI Agent Friendly

Structure designed specifically for AI Agents:
- Clear instructions and deliverable requirements
- Templates can be filled directly
- Checklists can be confirmed item by item

---

## Example: Starting a Project with MIMIR

### Input

```
I want to build an internal permission management platform,
supporting multi-level approvals, role-based permission assignment, and audit trails.
About a few hundred users, no high concurrency needs.
```

### MIMIR Will Guide You Through

1. **Requirements Analysis** → Produce PRD, clarify user roles, permission model, P0-P3 feature prioritization
2. **Technology Selection** → Produce tech selection document, confirm tech stack
3. **System Design** → Produce database design, API design
4. **Detailed Specifications** → Produce state machine definitions, business rules, UI prototypes
5. **Testing Strategy** → Produce test case documents, establish TDD workflow
6. **Documentation Delivery** → Produce user manual, admin manual

---

## Contributing

MIMIR evolves through project retrospectives. Welcome to:

1. **Use and Provide Feedback** - Share your experience in Issues
2. **Contribute Retrospectives** - Use `retro/RETRO-GUIDE.md` for project retrospectives, submit PRs
3. **Extend Project Types** - Contribute Skills for mobile, CLI, and other types

---

## Why the Name MIMIR?

> In Norse mythology, **Mímir** is the guardian of the Well of Wisdom. Odin, the All-Father, sacrificed one of his eyes to drink from the well and gain cosmic wisdom.
>
> Later, Mímir was beheaded, but Odin used magic to keep his head alive as an eternal wisdom advisor. Whenever facing major decisions, Odin would consult Mímir.

This MIMIR methodology is also wisdom earned through the cost of real project pitfalls.

**Go ask MIMIR.**

---

## License

MIT

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.4 | 2025-01-31 | Added Core Principles (CORE-PRINCIPLES.md) and Claude Code Prompt Skill, based on Task Decomposition validation practice |
| v1.3 | 2025-01-30 | Added Meta-Knowledge Extraction Skill (meta-knowledge/) |
| v1.2 | 2025-01-28 | Added document consistency management templates |
| v1.1 | 2025-01-27 | Added testing strategy and documentation delivery phases |
| v1.0 | 2025-01-27 | Initial version, extracted from real enterprise project experience |
