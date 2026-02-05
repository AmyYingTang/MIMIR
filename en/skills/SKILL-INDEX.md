# Software Project Startup Methodology - Skill System

> **Version**: v2.0  
> **Created**: 2025-01-27  
> **Maintenance**: Continuously updated through project retrospectives  
> **Target Users**: AI Agents (like Claude) or human developers/architects

---

## What is this?

This is an **executable software project startup methodology**, different from traditional books:

| Traditional Methodology Books | This Skill System |
|-------------------------------|-------------------|
| Knowledge is static, gets outdated | Continuously updated through project retrospectives |
| Knowledge is scattered, disconnected | Structured checklists + decision trees |
| Requires readers to judge applicability | Automatically matches scenarios through questioning |
| Read it all, still don't know what to do | Directly produces project documentation |

---

## How to Use?

### Scenario A: Starting a New Project

```
1. Tell me: "I want to start a new project"
2. I will ask you a few classification questions (project type, scale, constraints, etc.)
3. Based on your answers, I will load the corresponding Skill document
4. Following the Skill guidance, I will proactively ask questions, give suggestions, and produce documentation
```

### Scenario B: Project Retrospective to Update Methodology

```
1. Tell me: "I want to do a project retrospective"
2. I will guide you through reviewing key decisions and pitfalls in the project
3. Extract reusable experience
4. Update the corresponding Skill documents
```

---

## Skill Lifecycle Stages

Skills in MIMIR map to stages of the development lifecycle:

```
🔵 Plan          →  🟢 Build            →  🟡 Verify          →  🟣 Reflect        →  ⚪ Retrospect
project-kickoff     claude-code-prompt      review-agent            meta-knowledge      retro
```

Not all stages are required for every project, but the sequence represents the natural flow of development.

---

## Skill System Structure

```
skills/
├── SKILL-INDEX.md                          # 📍 You are here - Entry document
│
│ ── 🔵 PLAN ──────────────────────────────────────────────────────────────────
│
├── project-kickoff/                        # Project startup methodology
│   ├── SKILL.md                            # Main document - Project classification decision tree
│   │
│   ├── enterprise-web/                     # Enterprise Web Projects
│   │   ├── SKILL.md                        # ⭐ Main guide
│   │   ├── phase-1-requirements.md         # Requirements analysis phase
│   │   ├── phase-2-tech-selection.md       # Technology selection phase
│   │   ├── phase-3-system-design.md        # System design phase
│   │   ├── phase-3-ui-design-principles.md # UI/UX design principles
│   │   ├── phase-4-testing.md              # Testing strategy phase
│   │   ├── phase-5-documentation.md        # Documentation delivery phase
│   │   └── checklists/                     # Checklists
│   │       ├── security-checklist.md
│   │       ├── production-readiness.md
│   │       └── enterprise-concerns.md
│   │
│   ├── mobile-app/                         # Mobile Apps (Future expansion)
│   │   └── SKILL.md
│   │
│   ├── cli-tool/                           # CLI Tools (Future expansion)
│   │   └── SKILL.md
│   │
│   └── templates/                          # Document templates
│       ├── prd-template.md
│       ├── tech-selection-template.md
│       ├── database-design-template.md
│       ├── api-design-template.md
│       ├── project-control-template.md
│       ├── doc-dependencies-template.md
│       └── change-review-checklist-template.md
│
│ ── 🟢 BUILD ─────────────────────────────────────────────────────────────────
│
├── claude-code-prompt/                     # Claude Code Prompt design
│   ├── SKILL.md                            # Prompt structure, quality principles, task decomposition
│   └── templates/                          # Prompt templates
│       ├── 01-project-init-template.md
│       └── file-download-pattern.md        # 🆕 File download pattern code snippet
│
├── convention-extraction/                  # 🆕 Cross-module convention consistency
│   └── SKILL.md                            # Extraction dimensions, snapshot format, prompt template
│
│ ── 🟡 VERIFY ────────────────────────────────────────────────────────────────
│
├── review-agent/                           # 🆕 Independent code review (Quality Assurance)
│   └── SKILL.md                            # Review dimensions, design principles, report format
│                                           # Runtime: MIMIR-BO review-agent/
│
│ ── 🟣 REFLECT ───────────────────────────────────────────────────────────────
│
├── meta-knowledge/                         # Meta-knowledge extraction
│   └── SKILL.md                            # Extract reusable insights from AI collaboration
│
│ ── ⚪ RETROSPECT ─────────────────────────────────────────────────────────────
│
└── retro/                                  # Retrospective extraction tool
    ├── RETRO-GUIDE.md                      # Retrospective guide document
    ├── RETRO-TEMPLATE.md                   # Retrospective record template
    ├── retro-doc-consistency.md            # Document consistency retrospective
    ├── retro-integration-testing-download.md  # Integration testing retrospective
    ├── retro-aggregation-doc-drift.md      # Aggregation document drift retrospective
    ├── retro-methodology-spiral.md         # Methodology spiral evolution retrospective
    └── retro-s22-validation-evidence.md    # Quality principle cross-module validation evidence
```

---

## Currently Available Skills

| Skill | Stage | Status | Applicable Scenarios |
|-------|:-----:|:------:|----------------------|
| **Enterprise Web Projects** | 🔵 Plan | ✅ Available | B2B SaaS, internal management systems, platform products |
| **Claude Code Prompt Design** | 🟢 Build | ✅ Available | Designing prompts for AI-driven code generation |
| **Convention Extraction** | 🟢 Build | ✅ Available | Extracting cross-module conventions from code after each module |
| **Independent Code Review** | 🟡 Verify | ✅ Available | Post-build verification against design docs. Runtime in MIMIR-BO |
| **Meta-Knowledge Extraction** | 🟣 Reflect | ✅ Available | Extracting reusable insights from AI collaboration |
| Mobile Apps | 🔵 Plan | ⬜ Planned | iOS/Android native or cross-platform |
| CLI Tools | 🔵 Plan | ⬜ Planned | Command-line tools, scripts |
| Data Pipelines | 🔵 Plan | ⬜ Planned | ETL, data processing |

---

## Quick Start

**If you want to start a new project, say:**

> "I want to start a new project, please help me with project planning"

**If you've completed a project and want to do a retrospective, say:**

> "I just finished a project and want to do a retrospective and update the methodology"

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version, extracted from real enterprise project experience |
| v1.1 | 2025-01-27 | Added testing strategy (phase-4-testing.md) and documentation delivery (phase-5-documentation.md) phases |
| v1.2 | 2025-01-28 | Added document consistency management templates (doc-dependencies-template.md, change-review-checklist-template.md) |
| v1.3 | 2025-01-30 | Added Meta-Knowledge Extraction Skill (meta-knowledge/) for AI collaboration insights |
| v1.4 | 2025-01-31 | Added Core Principles (CORE-PRINCIPLES.md) and Claude Code Prompt Skill (claude-code-prompt/), based on Task Decomposition validation. *Note: SKILL-INDEX structure tree not updated at time of release* |
| v1.5 | 2025-02-01 | Claude Code Prompt Skill v2.0: template variables, interactive mode marker, connection testing; Core Principles v1.1: added "Validate Inputs Early" |
| v1.6 | 2025-02-01 | Added UI/UX Design Principles (phase-3-ui-design-principles.md): wizard pattern, role-based experience design, config-driven UI adaptation |
| v1.7 | 2025-02-02 | Claude Code Prompt v2.1: 9 Task Decompose Quality Principles; enterprise-web phase-2 v1.1: Full Containerization + Healthcheck Alignment |
| v1.8 | 2025-02-04 | Added Review Agent skill (independent code review). Introduced lifecycle stages: Plan → Build → Verify → Reflect → Retrospect. Synced structure tree to reflect all existing skills (claude-code-prompt, meta-knowledge, review-agent, retro). Backfilled v1.4–v1.7 version history |
| v1.9 | 2025-02-04 | Added Convention Extraction skill (cross-module consistency). Bridges the gap between design docs (contracts) and code (conventions) |
| v2.0 | 2025-02-05 | Synced structure tree: claude-code-prompt/templates added file-download-pattern.md; retro/ added retro-aggregation-doc-drift.md, retro-methodology-spiral.md, retro-s22-validation-evidence.md |
