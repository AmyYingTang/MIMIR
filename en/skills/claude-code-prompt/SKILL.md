# Claude Code Prompt Skill

> **Version**: v1.0  
> **Created**: 2025-01-31  
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
- Only ask users for information when necessary
- Complete all operations autonomously after getting information
- Report progress after completing each phase

---

## Step 1: [Information Collection/Environment Confirmation]

[Information needed from user, ask all at once]

```
I need some information:

1. [Question 1]
2. [Question 2]
3. [Question 3]

Please provide the above information, I will automatically complete all subsequent operations.
```

**After getting information, automatically check**:
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

Next Step: [Guidance]
```

---

## Error Handling

If problems occur:

1. [Common Error 1]: [Solution]
2. [Common Error 2]: [Solution]

Report specific errors and provide solutions.
```

---

## Key Design Principles

### 1. Complete Information Collection at Once

**Correct**:
```
I need the following information:
1. What is your Python command?
2. Where is the project directory?
3. MySQL connection info?

Please provide all at once.
```

**Incorrect**:
```
What is your Python command?
[Wait for reply]
OK, where is the project directory?
[Wait for reply]
What is the MySQL connection info?
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
