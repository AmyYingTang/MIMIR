# Skill: Convention Extraction — Cross-Module Consistency

> **Version**: v0.2  
> **Created**: 2025-02-04  
> **Last Updated**: 2025-02-05  
> **Category**: Build (Pre-Prompt)  
> **Runtime**: Claude Code CLI or manual extraction

---

## Purpose

When a project has multiple modules built sequentially, the first module establishes implicit conventions — naming patterns, architectural layers, error handling styles, shared data formats — that design documents don't specify. Without capturing these conventions, subsequent modules will "reinvent" them differently, creating inconsistencies that are expensive to fix.

This skill extracts a **Convention Snapshot** from existing code after each module, producing a compact document that feeds into the next module's prompt generation.

**Key insight**: Design documents define the *contract* (what). Code establishes the *convention* (how). Contracts are specified upfront; conventions emerge during implementation. Both must be consistent across modules, but only contracts have an explicit source of truth. This skill creates an explicit source of truth for conventions.

---

## When to Use

```
Module 1:  design doc ──────────────→ prompt → code → ★ extract snapshot v1 ★
Module 2:  design doc + snapshot v1 → prompt → code → ★ update snapshot v2 ★
Module 3:  design doc + snapshot v2 → prompt → code → ★ update snapshot v3 ★
```

Trigger **after each module is completed and accepted**, before generating prompts for the next module.

**Important**: Module 1 has no convention problem — it *is* the source of conventions. Extraction starts after Module 1.

---

## The Problem This Solves

### Contract vs Convention

| Aspect | Contract (Design Doc) | Convention (Code) |
|--------|----------------------|-------------------|
| **Defined when** | Before coding | During coding |
| **Example** | "POST /api/training/start" | "All router functions named verb_noun" |
| **Specified by** | Architect/designer | First developer (or first AI agent) |
| **Source of truth** | Design document | First module's code |
| **What happens without it** | review-agent catches it post-build | Nobody catches it until integration |

### Why Design Docs Can't Cover This

Design docs rightly stay at the interface level. They should NOT specify:

- Internal variable naming patterns
- Service layer organization within a module
- Error handling implementation details
- Frontend component structure conventions
- Utility function patterns and locations
- Import path conventions
- Test fixture organization

These decisions are made during implementation and must be consistent across modules, but specifying them upfront would be premature and brittle. They need to be **extracted after they emerge**, not prescribed before.

### What Happens Without Convention Extraction

```
Module 1 (auth):     useAuthStore, /api/auth/*, AppException, Depends(get_current_user)
Module 2 (training): trainingStore, /training/*, HTTPException, @require_auth decorator

Both are valid choices. Both pass tests. But the project is now inconsistent.
```

---

## Extraction Dimensions

The snapshot is organized into 5 dimensions. Each dimension has a set of questions that Claude Code answers by scanning the existing codebase.

### Dimension 1: Structure Conventions

What to extract:

```
- Backend layer organization (router → service → repository? or different?)
- Frontend component organization (by feature? by page? by type?)
- Where shared types/enums are defined
- Test file location and naming pattern
- Config file organization
```

Example output:

```yaml
structure:
  backend_layers: "routers/ → services/ → repositories/ → models/"
  frontend_components: "views/{module}/ by page, components/ for shared"
  shared_enums: "backend: app/common/enums.py | frontend: src/types/enums.ts"
  test_location: "tests/{module}/ mirroring src structure"
  config: ".env for secrets, app/core/config.py for app config"
```

### Dimension 2: Naming Conventions

What to extract:

```
- API router function names (verb_noun? noun_verb? other?)
- Frontend store naming (useXxxStore? xxxStore? other?)
- Celery/background task naming
- Database model class ↔ table name mapping
- API route path patterns (/api/v1/{module}/{action}?)
- Frontend route path patterns
```

Example output:

```yaml
naming:
  router_functions: "verb_noun — create_user, get_training_status, delete_model"
  stores: "use{Module}Store — useAuthStore, useTrainingStore"
  celery_tasks: "{module}_{action}_task — training_start_task"
  db_models: "PascalCase class, snake_case table — class TrainingTask → training_tasks"
  api_paths: "/api/{module}/{resource} — /api/auth/login, /api/training/tasks"
  frontend_routes: "/{module}/{action} — /training/new, /models/list"
```

### Dimension 3: Pattern Conventions

What to extract:

```
- Error handling approach (custom exceptions? HTTP exceptions? error codes?)
- Authentication injection method (Depends? middleware? decorator?)
- Pagination implementation
- Frontend API call pattern (centralized client? per-module? axios interceptors?)
- Form validation approach (frontend, backend, or both?)
- Loading/error state management in frontend
```

Example output:

```yaml
patterns:
  error_handling: |
    Custom AppException(error_code, message, status_code)
    → global exception_handler converts to {"detail": str, "error_code": str}
  auth_injection: "Depends(get_current_user) as router parameter, returns User object"
  pagination: "CommonPaginationParams dependency, returns PaginatedResponse[T]"
  api_client: "src/api/client.ts — all requests via apiClient, auto-attaches Bearer token"
  form_validation: "Backend: Pydantic models. Frontend: Element Plus form rules"
  loading_states: "Per-store loading flags, components check store.loading"
```

### Dimension 4: Shared Interface Conventions

What to extract:

```
- Date/time format and timezone
- ID format (UUID version, string vs native)
- API response wrapper structure
- Status enum string values and where they're defined
- File upload/download patterns
- WebSocket message format (if applicable)
```

Example output:

```yaml
shared_interfaces:
  datetime: "ISO 8601, UTC, transmitted as string"
  id_format: "UUID v4, transmitted as string"
  response_wrapper: "Direct model return, no wrapper. Errors use standard format."
  status_enums: "Defined in backend enums.py, mirrored in frontend enums.ts"
  file_download: "GET with ?token= query param, returns file stream"
```

### Dimension 5: Infrastructure Conventions

What to extract:

```
- Docker service naming
- Environment variable naming pattern
- Port allocation pattern
- Database migration approach
- Logging format and levels
- Health check patterns
```

Example output:

```yaml
infrastructure:
  docker_services: "project-{service} — project-backend, project-frontend, project-db"
  env_vars: "UPPER_SNAKE — DB_HOST, REDIS_URL, JWT_SECRET_KEY"
  ports: "Backend 8000, Frontend 3000, DB 3306, Redis 6379"
  migrations: "Alembic auto-generate, one migration per feature"
  logging: "Python logging, JSON format in production, human-readable in dev"
  healthcheck: "GET /api/health returns {status: 'ok'}"
```

---

## Output Format

The snapshot is saved as `project-conventions.md` in the project root (or design docs folder).

```markdown
# Project Conventions Snapshot

> **Project**: [Project Name]
> **Extracted from**: Module [N] completion
> **Last Updated**: [Date]
> **Version**: [N] (increments with each module)

## Structure Conventions
[extracted content]

## Naming Conventions
[extracted content]

## Pattern Conventions
[extracted content]

## Shared Interface Conventions
[extracted content]

## Infrastructure Conventions
[extracted content]

## Change Log
| Version | After Module | Changes |
|---------|--------------|---------|
| v1 | auth (s-1-1) | Initial extraction |
| v2 | training (s-1-2) | Added Celery task naming, file download pattern |
```

**Size target**: The snapshot should be **under 200 lines**. It captures decisions, not code. If it's getting longer, you're including too much detail.

---

## How to Integrate with Prompt Generation

When generating prompts for Module N+1, add this section:

```markdown
## Project Conventions

This project has established the following conventions in previous modules.
You MUST follow these conventions to maintain cross-module consistency.

[paste or reference project-conventions.md content]

When making implementation decisions, check this list first.
If a decision is covered here, follow the existing convention.
If a decision is NOT covered here, make a reasonable choice and document it
in a "New Conventions" section at the end of your implementation summary.
```

This creates a feedback loop: each module both **consumes** and **produces** convention data.

---

## Extraction Methods

### Method A: Claude Code Scan (Recommended)

Provide Claude Code with the extraction prompt template (see below) and let it scan the codebase.

**Extraction Prompt Template**:

```markdown
# Convention Extraction Task

## Your Role
You are a code convention analyst. Scan the existing codebase and extract
the implicit conventions into a structured snapshot.

## What to Scan
- All source files in [backend/frontend directories]
- Configuration files
- Test files
- Docker and infrastructure files

## Extraction Dimensions
[paste the 5 dimensions from this SKILL.md]

## Output
Produce a `project-conventions.md` following the format specified above.
Only include conventions you can verify from actual code — do NOT guess or
infer conventions that aren't clearly established.

## Rules
- If a pattern appears in only one place, note it but mark as "tentative"
- If conflicting patterns exist, flag them as inconsistencies
- Keep each entry to 1-2 lines maximum
- Use actual examples from the code
```

### Method B: Manual Extraction After Retro

During the module retrospective, the developer reviews the code and fills in the snapshot template manually. Less thorough but simpler for smaller projects.

### Method C: Review Agent Integration

The review-agent can be extended to **output convention observations** as a byproduct of its review. When it scans Module N's code against design docs, it can simultaneously note the implementation patterns it observes.

---

## Relationship to Other MIMIR Skills

```
project-kickoff          → Defines contracts (design docs)
claude-code-prompt       → Generates prompts (consumes snapshot)
★ convention-extraction  → Extracts conventions (bridges the gap)
review-agent             → Verifies contracts + conventions
retro                    → Refines extraction dimensions
```

| Skill | Relationship |
|-------|-------------|
| **claude-code-prompt** | Downstream consumer. Prompts include snapshot as "Project Conventions" section |
| **review-agent** | Complementary. Review checks contracts; conventions check implementation patterns. Can share data via Method C |
| **retro** | Feedback loop. New inconsistency patterns discovered in retro → new extraction dimensions |
| **project-kickoff** | Upstream. Design docs define what conventions do NOT cover |

---

## Usage Pattern: Conventions as Cross-Module Fix Queue

Beyond serving as a consistency reference, the convention snapshot naturally acts as a **cross-module fix queue**.

### Pattern Description

When review-agent or manual review discovers inconsistencies (e.g., the same constant defined in three places), these inconsistencies are recorded in the convention snapshot's inconsistency section. The next module scans this list during task decomposition, and if that module happens to touch the relevant code, it fixes the issue "in passing" — a zero-cost closed loop.

```
Module N review → discovers inconsistency (e.g., FUNCTION_TYPE_NAMES duplicated 3x)
    ↓ record in convention snapshot inconsistency list
Module N+1 decomposition → scans inconsistency list
    ↓ this module happens to need constants.py
Module N+1 execution → establishes authoritative definition + cleans duplicates → closed loop
```

### Key Principles

- **Whoever first touches the code fixes it.** No separate "fix sprint" needed.
- The inconsistency list is **append-only writes**: reviews add new findings, modules mark resolved items as fixed.
- If an inconsistency goes untouched across multiple modules, it accumulates in the list — this itself is a signal that a dedicated fix prompt may be needed.

### Recording Format in Snapshot

Add an inconsistency section to `project-conventions.md`:

```markdown
## Known Inconsistencies (Pending Fix)

| ID | Description | Found in | Files Involved | Status |
|----|-------------|----------|----------------|--------|
| INC-001 | FUNCTION_TYPE_NAMES duplicated in enums.py, seed.py, constants.py | s-1-2 review | backend/app/ | ✅ Fixed in s-2-1 P01 |
| INC-002 | Date format ISO vs Unix timestamp mixed usage | s-1-2 review | api/, frontend/ | ⬜ Pending |
```

### Origin

s-1-2 review discovered `FUNCTION_TYPE_NAMES` duplicated in three places → recorded in conventions inconsistency → s-2-1 P01 happened to need a `constants.py` authoritative definition → cleaned up the three duplicates in passing. Zero additional cost for a complete closed loop. This validated that convention documents are not just records — they are a natural cross-module fix queue.

---

## Anti-Patterns

| Anti-Pattern | Why It's Wrong | What to Do Instead |
|-------------|----------------|-------------------|
| **Prescribing conventions in design docs** | Premature, brittle, clutters the design | Let them emerge in Module 1, then extract |
| **Snapshot as code linting rules** | Too rigid, misses semantic patterns | Keep it as human/AI-readable guidance |
| **Updating snapshot before module acceptance** | Conventions from broken code are unreliable | Extract only from accepted, working code |
| **Including implementation details** | Snapshot becomes too large to embed in prompts | Capture decisions and patterns, not code |
| **Skipping extraction for "small" modules** | Small modules still establish patterns | Always extract; small modules = small snapshot delta |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v0.1 | 2025-02-04 | Initial version. 5 extraction dimensions, 3 extraction methods, prompt template |
| v0.2 | 2025-02-05 | Added "Usage Pattern: Conventions as Cross-Module Fix Queue", validated by s-1-2 review → s-2-1 closed-loop fix practice |
