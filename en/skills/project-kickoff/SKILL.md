# Project Kickoff - Classification Decision Tree

> **Purpose**: Determine project type through questioning, load corresponding Skill  
> **Users**: AI Agents use this when starting new project conversations

---

## Decision Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      Project Classification Decision Tree                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Q1: What type of software project is this?                                 │
│  ├── Web Application (browser-based) ───────────────────────► Q2           │
│  ├── Mobile App (iOS/Android) ──────────────────────────────► [mobile-app] │
│  ├── Desktop Application (Windows/Mac/Linux) ───────────────► [desktop-app]│
│  ├── CLI Tool / Script ─────────────────────────────────────► [cli-tool]   │
│  ├── Backend Service / API (no frontend) ───────────────────► Q2-backend   │
│  ├── Data Pipeline / ETL ───────────────────────────────────► [data-pipeline]│
│  └── Other ─────────────────────────────────────────────────► [custom]     │
│                                                                             │
│  Q2: What is the nature of this project?                                    │
│  ├── Enterprise / Commercial Project                                        │
│  │   ├── Multi-user / Requires authentication ──────────────► [enterprise-web]│
│  │   ├── Single-user / Tool-like ───────────────────────────► [lightweight-web]│
│  │   └── Internal use / Admin dashboard ────────────────────► [enterprise-web]│
│  │                                                                          │
│  ├── Personal Project / Experimental                                        │
│  │   ├── Learning / Demo ───────────────────────────────────► [minimal]    │
│  │   └── Open Source Project ───────────────────────────────► [opensource] │
│  │                                                                          │
│  └── MVP / Quick Validation ────────────────────────────────► [mvp]        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Agent Questioning Script

When a user says "I want to start a new project", ask in the following order:

### Round 1: Basic Project Classification

```markdown
Alright, let me understand the basic project situation:

1. **Project Type**: What type of software is this?
   - [ ] Web Application (browser-based)
   - [ ] Mobile App
   - [ ] Desktop Application
   - [ ] CLI Tool / Script
   - [ ] Backend Service / API (no frontend)
   - [ ] Other: ______

2. **Project Nature**:
   - [ ] Enterprise / Commercial project (need to consider security, compliance, scalability)
   - [ ] Personal project / Experimental
   - [ ] MVP / Quick validation

3. **Brief Description**: Describe what this project does in one or two sentences?
```

### Round 2: Deep Dive Based on Type (Enterprise Web Example)

```markdown
Got it, this is an enterprise-level web project. Let me understand some key information:

**Users & Scale**:
1. Expected user volume? (Under 10 / Tens / Hundreds / Thousands / More)
2. Estimated concurrent users?
3. Need multi-tenant support?

**Authentication & Permissions**:
4. Need user login?
5. How many user roles? Is the permission model complex?
6. Future need for enterprise SSO integration (like LDAP/SAML/OIDC)?

**Data & Security**:
7. Handling sensitive data (personal info, financial data, etc.)?
8. Any compliance requirements (GDPR, SOC2, etc.)?
9. Data storage location restrictions (must be local / can be cloud)?

**Technical Constraints**:
10. What's the company's current tech stack? Any mandatory requirements?
11. What technologies is the team familiar with?
12. Existing infrastructure (database, CI/CD, etc.)?

**Timeline & Resources**:
13. Expected project timeline?
14. Development team size?
```

---

## Classification Result Mapping

| Classification | Loaded Skill | Key Characteristics |
|----------------|--------------|---------------------|
| `enterprise-web` | `enterprise-web/SKILL.md` | Full security, permissions, audit, scalability considerations |
| `lightweight-web` | `lightweight-web/SKILL.md` | Simplified auth, focus on core functionality |
| `mvp` | `mvp/SKILL.md` | Minimum viable, fast iteration, technical debt acceptable |
| `mobile-app` | `mobile-app/SKILL.md` | Native/cross-platform selection, App Store publishing |
| `cli-tool` | `cli-tool/SKILL.md` | Argument parsing, cross-platform compatibility |
| `minimal` | Don't load specific Skill | Start directly, minimal constraints |

---

## After Information Collection

1. **Confirm Understanding**: Restate project overview, confirm correct understanding
2. **Load Skill**: Load corresponding detailed Skill document based on classification
3. **Start Execution**: Progress step by step following Skill guidance
4. **Produce Documentation**: Output corresponding project documents at key checkpoints

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
