# Software Project Startup Methodology - Skill System

> **Version**: v1.3  
> **Created**: 2025-01-27  
> **Last Updated**: 2025-01-30  
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

### Scenario C: Meta-Knowledge Extraction

```
1. Tell me: "I want to extract insights from this collaboration"
2. I will guide you through reviewing the collaboration process
3. Identify patterns, abstract principles
4. Produce reusable insights
```

---

## Skill System Structure

```
skills/
├── SKILL-INDEX.md                          # 📍 You are here - Entry document
│
├── project-kickoff/                        # Project startup methodology
│   ├── SKILL.md                            # Main document - Project classification decision tree
│   │
│   ├── enterprise-web/                     # Enterprise Web Projects
│   │   ├── SKILL.md                        # ⭐ Main guide
│   │   ├── phase-1-requirements.md         # Requirements analysis phase
│   │   ├── phase-2-tech-selection.md       # Technology selection phase
│   │   ├── phase-3-system-design.md        # System design phase
│   │   ├── phase-4-testing.md              # Testing strategy phase
│   │   ├── phase-5-documentation.md        # Documentation delivery phase
│   │   └── checklists/                     # Checklists
│   │       ├── security-checklist.md       # Security checklist
│   │       ├── production-readiness.md     # Production readiness checklist
│   │       └── enterprise-concerns.md      # Enterprise concerns
│   │
│   ├── mobile-app/                         # Mobile Apps (Future expansion)
│   │   └── SKILL.md
│   │
│   ├── cli-tool/                           # CLI Tools (Future expansion)
│   │   └── SKILL.md
│   │
│   └── templates/                          # Document templates
│       ├── prd-template.md                 # PRD template
│       ├── tech-selection-template.md      # Tech selection template
│       ├── database-design-template.md     # Database design template
│       ├── api-design-template.md          # API design template
│       ├── project-control-template.md     # Project control document template
│       ├── doc-dependencies-template.md    # 🆕 Document dependencies template
│       └── change-review-checklist-template.md  # 🆕 Change review checklist template
│
├── meta-knowledge/                         # 🆕 Meta-knowledge extraction
│   └── SKILL.md                            # Meta-knowledge extraction skill
│
└── retro/                                  # Retrospective extraction tool
    ├── RETRO-GUIDE.md                      # Retrospective guide document
    ├── RETRO-TEMPLATE.md                   # Retrospective record template
    └── retro-doc-consistency.md            # 🆕 Document consistency retrospective
```

---

## Currently Available Skills

| Skill | Status | Applicable Scenarios |
|-------|:------:|----------------------|
| **Enterprise Web Projects** | ✅ Available | B2B SaaS, internal management systems, platform products |
| **Meta-Knowledge Extraction** | ✅ Available | Extract reusable insights from collaboration history |
| Mobile Apps | ⬜ Planned | iOS/Android native or cross-platform |
| CLI Tools | ⬜ Planned | Command-line tools, scripts |
| Data Pipelines | ⬜ Planned | ETL, data processing |

---

## Quick Start

**If you want to start a new project, say:**

> "I want to start a new project, please help me with project planning"

**If you've completed a project and want to do a retrospective, say:**

> "I just finished a project and want to do a retrospective and update the methodology"

**If you want to extract insights from collaboration, say:**

> "I want to extract reusable insights from this collaboration history"

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version, extracted from real enterprise project experience |
| v1.1 | 2025-01-27 | Added testing strategy (phase-4-testing.md) and documentation delivery (phase-5-documentation.md) phases |
| v1.2 | 2025-01-28 | Added document consistency management templates (doc-dependencies-template.md, change-review-checklist-template.md) |
| v1.3 | 2025-01-30 | Added Meta-Knowledge Extraction Skill (meta-knowledge/), exploring knowledge extraction in AI collaboration |
