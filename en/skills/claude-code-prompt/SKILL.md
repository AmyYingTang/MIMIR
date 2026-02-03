# Claude Code Prompt Skill

> **Version**: v2.4  
> **Created**: 2025-01-31  
> **Last Updated**: 2025-02-04  
> **Use Case**: Code generation and project implementation using Claude Code  
> **Prerequisites**: Completed system design phase with clear technical specifications

---

## What is This Skill?

This is a methodology and template set for writing high-quality Claude Code Prompts. Through structured Prompt design, it achieves:

- AI-led execution flow
- Users only provide necessary inputs
- Each step is verifiable
- Errors are handleable

---

## When to Use?

When you need to:
1. Have Claude Code help you scaffold a project
2. Have Claude Code implement specific modules
3. Have Claude Code generate test code
4. Have Claude Code create initialization scripts

---

## Prompt Structure Template

### Standard Structure

```markdown
# Claude Code Prompt: [Project Name] - [Task Name]

## Your Role

You are a [role description]. Your task is [task objective].

**Important Principles**:
- You lead the entire process
- All environment info is pre-provided, no need to ask user
- Complete all operations autonomously
- Report progress after completing each phase

---

## Environment Info (Confirmed)

The following info has been provided by the user. Use directly:

- **[Config Item 1]**: {{variable_name_1:default_value}}
- **[Config Item 2]**: {{variable_name_2}}

---

## Step 1: [Environment Verification]

Using the above info, automatically run checks:
- [Prerequisite 1]
- [Prerequisite 2]

---

## Steps 2 to N: [Execute Tasks]

### 2.1 [Subtask 1]

[Specific code/file content/execution commands]

### 2.2 [Subtask 2]

[Specific code/file content/execution commands]

---

## Verification Steps

[Commands or checks to verify task completion]

---

## Completion Report

After all steps are completed, report to user:

```
✅ [Task Name] Complete!

📁 Created/Modified Files:
- [File 1]
- [File 2]

🧪 Verification Results:
- [Check 1]: ✅
- [Check 2]: ✅

Next Step: Execute [next prompt filename]
```

---

## Error Handling

If problems occur:

1. [Common Error 1]: [Solution]
2. [Common Error 2]: [Solution]

Report specific errors and provide solutions.
```

---

## Template Variable Specification (Agent Collaboration)

### Overview

Prompts use `{{variable}}` format template variables instead of "ask the user". When executed through the Agent, it collects all variables upfront, fills them in, then executes in non-interactive mode.

### Syntax

| Format | Meaning | Example |
|--------|---------|---------|
| `{{name}}` | Required variable, no default | `{{project_dir}}` |
| `{{name:default}}` | Variable with default value | `{{mysql_host:localhost}}` |

### Naming Conventions

| Variable Type | Naming Rule | Example |
|---------------|-------------|---------|
| Database connection | `mysql_host`, `mysql_port`, `mysql_user`, `mysql_password`, `db_name` | Agent auto-triggers connection test |
| Python environment | `python_cmd` | `{{python_cmd:python3}}` |
| Project path | `project_dir` | `{{project_dir}}` |
| App configuration | Name by business meaning | `{{app_port:8000}}` |

> **Important**: Using standard names (especially `mysql_*` series) enables the Agent to automatically detect and test connections after collection.

### Usage in Prompts

**Correct (v2.0)**:
```markdown
## Environment Info (Confirmed)

- **Python Command**: {{python_cmd:python3}}
- **Project Directory**: {{project_dir}}
- **MySQL Host**: {{mysql_host:localhost}}
- **MySQL Password**: {{mysql_password}}
- **Database Name**: {{db_name:voice_model_platform}}
```

**Old approach (v1.0, deprecated)**:
```markdown
## Step 1: Information Collection

Ask the user for the following information (all at once):
...
**Wait for user reply before continuing.**
```

### Where Variables Should Appear

Template variables should appear not just in the info section but everywhere the values are referenced:

```markdown
## Execution Flow

### Phase 1: Create Project
cd {{project_dir}}
mkdir -p voice-model-platform/backend

### Phase 3: Setup Virtual Environment
{{python_cmd:python3}} -m venv venv
```

---

## Interactive Mode Marker

### Overview

Some Prompts contain dangerous operations that require user confirmation during execution (e.g., database migrations, delete operations). These Prompts must be marked as interactive mode, and the Agent will use a different execution strategy.

### Syntax

Add to the **first line** of the Prompt file:

```markdown
<!-- agent:interactive -->
# Claude Code Prompt: ...
```

### When to Use

| Scenario | Needs Marker? | Reason |
|----------|:------------:|--------|
| Create files/directories | ❌ | Non-destructive |
| Install dependencies | ❌ | Non-destructive |
| Database migration | ✅ | Modifies DB schema |
| Delete operations | ✅ | Irreversible |
| Init scripts (create DB, tables, seed data) | ✅ | Modifies database |
| Modify production config | ✅ | Affects live environment |

### Mode Comparison

| | Non-interactive (default) | Interactive |
|--|---|---|
| **Marker** | None | `<!-- agent:interactive -->` |
| **Agent behavior** | `claude -p --dangerously-skip-permissions` | `claude "prompt"` |
| **User action** | No intervention needed | May need to answer confirmations |
| **Use case** | File creation, dependency install | DB operations, deletions |

---

## Key Design Principles

### 1. Environment Info via Template Variables

**Correct (v2.0)**:
```markdown
## Environment Info (Confirmed)

- **Python Command**: {{python_cmd:python3}}
- **Project Directory**: {{project_dir}}
- **MySQL Password**: {{mysql_password}}
```

**Transitional (compatible with manual execution)**:
```markdown
## Environment Info (Confirmed)

The following info has been provided by the user. Use directly:

- **Python Command**: {{python_cmd:python3}}
- **MySQL Password**: {{mysql_password}}

> For manual execution, replace {{variables}} with actual values
```

**Old approach (v1.0, deprecated)**:
```
I need the following information:
1. What is your Python command?
2. Where is the project directory?
3. MySQL connection info?

Please provide all at once.
```

### 2. Execute Rather Than Instruct

**Correct**:
```
## Execution Steps

After getting user information, automatically execute:
1. Create directory structure
2. Create all files
3. Install dependencies
4. Run verification
```

**Incorrect**:
```
## Execution Steps

Please operate in the following order:
1. Run mkdir command
2. Run pip install
3. Check if files are created
```

### 3. Code Completeness

**Correct**:
Provide complete, directly usable code

**Incorrect**:
- Provide code snippets for users to assemble
- Use `...` or `# other code` to omit key parts

### 4. Front-load Error Handling

Provide common error handling solutions at the end of the Prompt, allowing AI to solve problems autonomously.

---

## Task Decomposition Guide

### Decomposition Principles

1. **Single Responsibility**: Each Prompt does one thing
2. **Verifiable**: Each Prompt ends with clear acceptance criteria
3. **Sequential Dependencies**: Later Prompts depend on earlier outputs
4. **Incremental**: From infrastructure to business logic

### Pre-Gate: Dependency Resolution Gate (DependencyResolutionGate)

Before starting task decomposition, the Agent must check whether the current module has cross-module dependencies. If unresolved dependencies exist, **decomposition must be blocked** until a Dependency Resolution (DR) document is produced.

**Process:**

1. **Scan Dependencies**: Read reference documents to identify resources the current module depends on but are outside its scope (e.g., database tables, seed data, service interfaces from prior modules)
2. **Assess Status**: For each dependency, determine if it is "ready" or "needs decision"
3. **Block or Proceed**:
   - All dependencies ready → proceed to decomposition
   - Unresolved dependencies exist → produce DR document listing decision options for each (e.g., Mock, Seed, Skip), then wait for user confirmation before continuing
4. **DR Document Format**: Each entry includes `DR-ID`, dependency description, available options, recommended option, and rationale

**Origin**: s-1-2 model training module task decomposition. This module depended on user and chip data from s-1-1. Without first resolving "what data to use during testing," the decomposed prompts would assume data existence and fail during execution.

---

### Quality Principles (Learned from Practice)

These 12 principles were extracted from real project execution experience. Apply them as a checklist when writing or reviewing prompts.

| # | Principle | Description | Example |
|---|-----------|-------------|---------|
| 1 | **ValidateRefs** | All referenced documents/files must exist and be accessible | If prompt says "refer to database-design.md", verify the file is in the expected path |
| 2 | **PathAlign** | Output file paths in prompts must match actual project directory structure | Don't write `backend/models.py` if the project uses `backend/app/models/` |
| 3 | **ProgressSignals** | Inter-prompt progress signals must be explicit | Prompt N's completion report should state what Prompt N+1 expects to find |
| 4 | **UserVerifyGuide** | Agent verification steps must include expected results | Not just "run pytest" but "run pytest, expect 12 tests passed, 0 failed" |
| 5 | **HostEnvAlign** | Commands must match the execution environment | If running inside Docker, use `docker compose exec backend pytest`, not bare `pytest` |
| 6 | **NamingConvention** | File naming conventions must be consistent across all prompts | Pick one pattern (e.g., `s-1-1-p01-xxx.md`) and use it everywhere |
| 7 | **IdempotentPrompts** | Prompts must be safe to re-run | No blocking foreground processes (`uvicorn &` + cleanup); use skip-if-exists logic for file creation and seed data |
| 8 | **UserAcceptGuide** | Each prompt needs user manual acceptance steps beyond agent auto-verification. **The final prompt must generate a standalone user verification guide file** (e.g., `VERIFY-GUIDE.md`) written in plain language describing what the user should do, and prompt the user to open it upon completion | Agent tests prove code works; user acceptance proves the feature meets business requirements. Final prompt output: `"Please open VERIFY-GUIDE.md and follow the verification steps"` |
| 9 | **ServiceDepChain** | Service dependency chains must be robust | One component's config error shouldn't cascade (e.g., a wrong healthcheck path shouldn't prevent dependent services from starting) |
| 10 | **DiscrepancyReport** | When discrepancies are found between reference documents, Agent resolves by choosing the approach that makes the system work, but must report the discrepancy + decision rationale + patch the source document directly. **Pay special attention to shared data across Prompts** (e.g., test user credentials, port numbers, database names) | Example 1: DDL has 4 status ENUM values but state-machines.md defines 5 states → Agent uses state machine as authority, updates DDL. Example 2: seed.py sets password to `Trainer@2025` but conftest.py hardcodes `Test123456` → 24 tests ERROR, root cause is a single mismatched password string |
| 11 | **FullStackFix** | Fix prompts must list changes for **every affected layer** (backend API, frontend calls, test cases, old endpoint cleanup, config files). Agents tend to fix only one layer and stop, leaving frontend/backend out of sync | Example: Backend adds `GET /chips/available` replacing `/tasks/my-chips`, but fix prompt only modifies backend. Frontend still calls old endpoint → 404. Tests still assert old field names → fail. Correct approach: fix prompt explicitly lists `backend/`, `frontend/`, `tests/`, `old endpoint removal` as four change blocks |
| 12 | **InlineAPIContract** | Prompts must **embed exact API request/response JSON schemas inline**, not just say "refer to api-design.md". Agents drift from referenced doc details (field names, paths, nesting) when generating large code volumes; inline schemas are the only reliable fidelity mechanism | Example: api-design.md defines `{chip_id, chip_model, available_functions: [{function_type, function_name}]}`, prompt just says "refer to api-design.md". Agent generates `{id, model, functions: ["KWS"]}` → frontend field parsing fails entirely |

### Recommended Granularity

| Task Type | Suggested Granularity |
|-----------|----------------------|
| Project Initialization | 1 Prompt |
| Database Models | 1 Prompt (per module) |
| Business Logic Module | 1-2 Prompts (depends on complexity) |
| API Endpoints | 1 Prompt (per module) |
| Test Code | 1 Prompt (per module) |
| Initialization Script | 1 Prompt |

### Example: User Authentication Module

```
Prompt 01: Project Initialization (scaffold)
    ↓
Prompt 02: Database Models (User, Role, RefreshToken)
    ↓
Prompt 03: Security Module (JWT, bcrypt)
    ↓
Prompt 04: Auth API (4 endpoints)
    ↓
Prompt 05: API Tests
    ↓
Prompt 06: Init Script (create DB + tables + seed data)
```

---

## Acceptance Criteria Template

Each Prompt should clearly define acceptance criteria:

```markdown
## Acceptance Criteria

- [ ] [File/Directory] created
- [ ] [Command] executed successfully
- [ ] [Test] passed
- [ ] [Service] accessible
- [ ] [Endpoint] returns expected result
```

---

## Relationship with Other Skills

| Skill | Relationship |
|-------|-------------|
| project-kickoff/enterprise-web | This Skill implements its Phase 4 (Code Implementation) |
| meta-knowledge | Can extract experience from Claude Code practice |

---

## Case Reference

See: `templates/` directory for actual cases

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2025-01-31 | Initial version based on user auth module validation practice |
| v2.0 | 2025-02-01 | Major update: Template variables `{{variable}}` replace manual input collection; Interactive mode marker `<!-- agent:interactive -->` for dangerous operations; Agent connection test variable naming conventions |
| v2.1 | 2025-02-02 | Added 9 Task Decompose Quality Principles (ValidateRefs, PathAlign, ProgressSignals, UserVerifyGuide, HostEnvAlign, NamingConvention, IdempotentPrompts, UserAcceptGuide, ServiceDepChain) based on s-1-1 execution experience |
| v2.2 | 2025-02-03 | Added pre-gate DependencyResolutionGate; added 10th Quality Principle DiscrepancyReport (cross-document discrepancy reporting), based on s-1-2 task decomposition experience |
| v2.3 | 2025-02-03 | Enhanced UserAcceptGuide (#8) with delivery format requirement: final prompt must generate a standalone user verification guide file, based on s-1-2 post-execution acceptance experience |
| v2.4 | 2025-02-04 | Added 11th principle FullStackFix (fixes must cover every layer in the stack) and 12th principle InlineAPIContract (prompts must embed exact API schemas inline), based on s-1-2 frontend-backend integration testing experience |
