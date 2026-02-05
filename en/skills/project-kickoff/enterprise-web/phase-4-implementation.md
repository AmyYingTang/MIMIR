# Phase 4: Code Implementation

> **Goal**: Decompose by module, implement incrementally, continuously verify quality  
> **Prerequisites**: System design complete (database, API, security)  
> **Key Outputs**: Working code, tests, convention snapshot  
> **Key Principles**: Make it work first, cross-module consistency, human-AI alignment

---

## 1. Implementation Order Principles

### 1.1 Group Modules by Phase

Group PRD functional modules by priority:

| Phase | Goal | Includes | Dependencies |
|-------|------|----------|--------------|
| Phase 1 | MVP working | Core business flow (auth + core features) | None |
| Phase 2 | Full permissions | Permission control, admin features | Depends on Phase 1 |
| Phase 3 | Enhanced UX | Notifications, auxiliary features | Depends on Phase 2 |
| Phase N | ... | ... | ... |

### 1.2 Module Dependency Principles

- **Foundation before business**: Auth, permissions before business modules
- **Backend before frontend**: API first, then frontend integration
- **Core before enhancement**: P0 features before P1/P2

---

## 2. Single Module Execution Flow

Each module from start to completion goes through these stages:

```
┌──────────────────────────────────────────────────────────────────┐
│                   Single Module Execution Flow                    │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ① Align       ② Decompose    ③ Generate      ④ Execute        │
│  ┌────────┐   ┌────────┐   ┌────────┐      ┌────────┐          │
│  │ Human- │ → │ Task   │ → │ Prompt │ →    │ Code   │          │
│  │ AI     │   │ Decom- │   │ Genera-│      │ Imple- │          │
│  │ Align  │   │ pose   │   │ tion   │      │ ment   │          │
│  └────────┘   └────────┘   └────────┘      └────────┘          │
│       │                                         │               │
│       │    ⑧ Extract      ⑦ Triage     ⑥ Review │  ⑤ Verify    │
│       │   ┌────────┐   ┌────────┐   ┌────────┐ │ ┌────────┐   │
│       │   │ Conven-│ ← │ Human  │ ← │ Indep- │←┘ │ Smoke  │   │
│       │   │ tion   │   │ Triage │   │ endent │   │ Test   │   │
│       │   │ Extract│   │        │   │ Review │   │        │   │
│       │   └────────┘   └────────┘   └────────┘   └────────┘   │
│       │       │                                       ↑         │
│       │       ▼                                       │         │
│       │   Feeds into next module's ① ─────────────────┘         │
│       │                                                         │
└──────────────────────────────────────────────────────────────────┘
```

### Stage Summary

| # | Stage | Purpose | Output |
|---|-------|---------|--------|
| ① | Human-AI Alignment | Ensure shared understanding of process and goals | Difference log (if any) |
| ② | Task Decomposition | Break module into executable prompt sequence | Decomposition plan + prompt files |
| ③ | Prompt Generation | Write concrete implementation instructions | Prompt file set |
| ④ | Code Implementation | Execute prompts, generate code | Code + tests |
| ⑤ | Smoke Test | Quick verification that core paths work | Test results |
| ⑥ | Independent Review | Code review against design docs, separate from coding | Review report |
| ⑦ | Human Triage | Human decides which findings to fix now | Fix prompt + open-issues |
| ⑧ | Convention Extraction | Extract implementation conventions from accepted code | Convention snapshot (updated) |

---

## 3. Stage Details

### 3.1 ① Human-AI Alignment

**Trigger**: Before each module begins

**Why**: Humans and AI each have their own understanding of "how the standard process should work." These understandings may diverge. If divergence isn't exposed before starting, subsequent steps run on different assumptions, causing rework.

**How**:

1. Human and AI each independently output their understanding of the module execution flow:
   - What steps to go through
   - What the inputs and outputs of each step are
   - Any special considerations for this module (dependencies, risks)
2. Compare both outputs, identify differences
3. Reach agreement on differences before proceeding

**Example**:

```
Human: "This module needs backend API first, then frontend pages, then tests"
AI:    "This module needs DB migration first, then backend API, then frontend,
        tests written alongside API"
Difference: Is DB migration a separate step? When are tests written?
→ Discuss and agree
```

**Note**: This is not a ritual. When the module is simple and the process unambiguous, a quick confirmation suffices. The value shows in complex modules or when the process has changed.

**Origin**: At the end of Phase 2, human quizzed AI on standard process understanding. AI missed the decompose step's starting input. This real divergence validated the alignment step — not that AI couldn't do it, but that AI's understanding of "where to start" differed from human's.

### 3.2 ② Task Decomposition

Break the module into an ordered sequence of implementation steps.

**Inputs**:
- Design documents (API design, database design, state machine definitions)
- Convention snapshot (implementation conventions from prior modules)
- Known inconsistency list from convention snapshot (items to fix opportunistically)

**Quality Requirements**:
- Each step has explicit acceptance criteria
- Inter-step dependencies are explicitly declared
- If cross-module dependencies are discovered, must explicitly choose a handling strategy (declare dependency / merge modules / minimal stub) — no silent absorption

**Cross-Module Dependency Handling**:

| Strategy | When to Use | Requirement |
|----------|-------------|-------------|
| Declare dependency | Dependent module is complex, don't merge | Note "assumes X is complete" in prompt |
| Merge modules | Dependent module is simple, manageable | Acceptance criteria must cover absorbed module |
| Minimal stub | Need the dependency but don't want full implementation | Create stub/mock |

Regardless of strategy, human must confirm.

### 3.3 ③ Prompt Generation

Convert decomposition into executable prompts.

**Quality Requirements**:
- Embed exact API request/response schemas (not just "refer to api-design.md")
- Include convention snapshot as "Project Conventions" section
- Every prompt ends with a git commit step
- Fix prompts must use grep/find to locate actual files first, never assume filenames

### 3.4 ④ Code Implementation

Execute prompts, generate code.

**Principles**:
- Make it work first, refine later
- Small commits, frequent integration
- Follow patterns from the convention snapshot

### 3.5 ⑤ Smoke Test

After implementation, quickly verify core paths work.

**Purpose**: Not comprehensive testing — confirm "it runs, main path works."

**Methods**:
- Manually call core APIs
- Check frontend pages load and interact
- Confirm database writes are correct

### 3.6 ⑥ Independent Review

A reviewer independent from coding (human or agent) checks code against design docs.

**Review Dimensions**:
- API contract consistency (code vs design docs)
- Frontend-backend field alignment
- Status enum consistency
- Shared data structure consistency
- Test coverage

**Output**: Structured review report (each finding with ID, severity, file location)

### 3.7 ⑦ Human Triage

Not all findings need immediate fixing. Human judges each finding's disposition:

| Disposition | Meaning | Destination |
|-------------|---------|-------------|
| fix | Fix this round | Generate fix prompt |
| defer | Postpone to later module | Record in open-issues |
| wontfix | Won't fix | Record rationale and close |

**Principle**: Reports and fixes must be decoupled. Not every finding is worth fixing now — humans decide priorities.

### 3.8 ⑧ Convention Extraction

Extract implementation conventions from accepted code, update the convention snapshot.

**Extraction Dimensions**:
- Structural conventions (layering, file organization)
- Naming conventions (functions, variables, routes)
- Pattern conventions (error handling, auth injection, pagination)
- Shared interface conventions (datetime format, ID format, response structure)
- Infrastructure conventions (Docker, env vars, ports)

**Important**: Only extract from accepted, working code. Conventions from broken code are unreliable.

The convention snapshot feeds into the next module's ② Task Decomposition, creating a feedback loop.

---

## 4. Implementation Principles

| Principle | Description |
|-----------|-------------|
| **Make it work first** | Implement core flow first, optimize later |
| **Continuous integration** | Small commits, frequent integration |
| **Cross-module consistency** | Follow patterns from convention snapshot |
| **Code standards** | Follow language/framework best practices |
| **Containerized development** | Dev environment = deployment environment |

---

## 5. Recommended Implementation Order

```
1. Infrastructure
   ├── Project scaffolding
   ├── Database connection
   ├── Configuration management
   └── Logging framework

2. Authentication module
   ├── User model
   ├── Registration / Login
   ├── JWT issuance / validation
   └── Permission middleware

3. Core business modules
   ├── Implement by P0 priority
   ├── Each module goes through the full execution flow (①-⑧)
   └── Unit tests alongside implementation

4. Admin backend
   ├── User management
   ├── Configuration management
   └── Data overview

5. Frontend implementation
   ├── Routing and layout
   ├── Authentication flow
   ├── Core pages
   └── Admin interface
```

---

## 6. FAQ

### Does module 1 need the full flow?

Module 1 is the source of conventions — there are no prior conventions to reference. Its flow starts at ② (skipping convention snapshot input in ①), and at ⑧ it produces the first convention snapshot.

### What about very small modules?

Each step can be executed lightly. A small module's ① might be "same process as last module," and ⑧ might add just two lines to the snapshot. Don't skip steps, but adjust depth as needed.

### What if review finds too many issues?

That's exactly why ⑦ Triage exists. Not everything needs fixing now. Triage by severity: CRITICAL/HIGH fix immediately, MEDIUM can defer, LOW can wontfix. Avoid falling into an endless fix loop.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2025-02-05 | Initial version. Module execution standard flow distilled from s-1-1 through s-2-3 execution experience |
