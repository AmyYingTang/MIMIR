# Document Dependency Graph Template

> **Purpose**: Define relationships between project documents for consistency checking  
> **Usage**: When one document changes, consult this graph to identify other documents that may need updates

---

## How to Use

1. When you modify any document, find it in the `documents` section
2. Check its `triggers_review` list to see which documents might need updates
3. Review each listed document for necessary changes
4. Use the `change_review_template` to document your review

---

## Document Dependencies Definition

```yaml
# Project Documents Dependency Graph
# Version: v1.0

documents:
  # UI Prototype
  ui_prototype:
    path: "prototype/"
    description: "UI prototype HTML files"
    triggers_review:
      - api-design.md          # New UI feature → may need new API
      - state-machines.md      # UI state display → may involve state changes
      - business-rules.md      # UI interaction logic → may involve business rules
      - prd.md                 # UI feature → PRD feature description
      - test-cases/            # UI changes → need test case updates
    change_signals:
      - "New page added"
      - "New action button added"
      - "New status display added"
      - "Role permission changes"

  # PRD (Product Requirements Document)
  prd:
    path: "prd-*.md"
    description: "Product requirements document"
    triggers_review:
      - api-design.md          # New requirement → may need new API
      - database-design.md     # New requirement → may need new tables/fields
      - ui_prototype/          # New requirement → needs prototype
      - state-machines.md      # New status/flow → state machine update
      - business-rules.md      # New rules → business rules update
    change_signals:
      - "New feature module"
      - "Business flow modification"
      - "New role or permission"
      - "Validation rule changes"

  # API Design Document
  api-design:
    path: "api-design.md"
    description: "API interface design document"
    triggers_review:
      - database-design.md     # New API → may need new tables/fields
      - business-rules.md      # API validation logic → business rules
      - state-machines.md      # API operations → state changes
      - error-codes.md         # New API → may need new error codes
    change_signals:
      - "New endpoint"
      - "Request/response format changes"
      - "Permission requirement changes"
      - "New error codes"

  # Database Design Document
  database-design:
    path: "database-design.md"
    description: "Database design document"
    triggers_review:
      - api-design.md          # New field → API may need to expose it
      - migration-scripts/     # Schema changes → need migration scripts
    change_signals:
      - "New table"
      - "New field"
      - "Field type change"
      - "Index modification"

  # State Machine Definitions
  state-machines:
    path: "state-machines.md"
    description: "State machine definition document"
    triggers_review:
      - api-design.md          # New transition → may need new API
      - business-rules.md      # State rules → business rules
      - database-design.md     # New status → may need enum update
    change_signals:
      - "New status added"
      - "New transition path"
      - "Transition condition change"

  # Business Rules
  business-rules:
    path: "business-rules.md"
    description: "Business rules documentation"
    triggers_review:
      - api-design.md          # Rule change → API validation logic
      - test-cases/            # Rule change → test cases
    change_signals:
      - "New validation rule"
      - "Calculation logic change"
      - "Constraint change"

  # Tech Stack
  tech-stack:
    path: "tech-stack-*.md"
    description: "Technology selection document"
    triggers_review:
      - deployment/            # Stack change → deployment config
      - api-design.md          # Framework change → API implementation
    change_signals:
      - "Technology component change"
      - "Version upgrade"

  # Project Control Document
  project-control:
    path: "project-control-doc.md"
    description: "Project control document"
    auto_update: true          # Auto-update version and index when other docs change
    update_on_any_change: true
```

---

## Change Review Checklist Template

Use this template after making document changes:

```markdown
## Change Review Checklist

**Changed Document**: [document name]
**Change Description**: [brief description of changes]
**Change Date**: [YYYY-MM-DD]

### Auto-Check Items
Based on doc-dependencies.yaml, these documents should be reviewed:

- [ ] [Document 1] - [Status: needs update / no change needed / updated]
- [ ] [Document 2] - [Status]
- ...

### Manual Confirmation Required
- [ ] [Item requiring human judgment]
- ...

### Update Records
| Document | Status | Notes |
|----------|:------:|-------|
| xxx.md | ✅ Updated | [what was changed] |
| yyy.md | ⏭️ No change needed | [reason] |
```

---

## Best Practices

### 1. Review Before Committing

Before committing document changes:
- Run through the dependency checklist
- Update all affected documents
- Record changes in project control document

### 2. Atomic Updates

When possible, update all related documents in the same commit:
- Keeps documentation in sync
- Easier to track related changes
- Simplifies rollback if needed

### 3. Version Alignment

Keep version numbers aligned across related documents:
- PRD v1.2 → API Design v1.2 → Database Design v1.2
- Use supplement documents (e.g., `api-design-supplement-v1.2.1.md`) for minor updates

### 4. Regular Audits

Periodically review document consistency:
- Weekly: Check recently modified documents
- Monthly: Full consistency audit
- After major features: Comprehensive review

---

## Example: UI Prototype Change

When UI prototype adds a new Admin feature:

```markdown
## Change Review Checklist

**Changed Document**: UI Prototype v2.0
**Change Description**: Added Admin task queue management page
**Change Date**: 2025-01-28

### Auto-Check Items
- [x] api-design.md → ❌ Missing cancel/retry/logs endpoints
- [x] state-machines.md → ❌ Missing Admin cancel transition
- [x] business-rules.md → ❌ Missing Admin operation rules
- [x] prd.md → ❌ Missing feature description

### Manual Confirmation Required
- [ ] Is the "mark complete/failed" feature needed? → Pending expert confirmation

### Update Records
| Document | Status | Notes |
|----------|:------:|-------|
| api-design-supplement-v1.1.md | ✅ Created | Added Admin task operation APIs |
| state-machines.md | ✅ Created | Includes task state machine with Admin ops |
| business-rules.md | ✅ Created | Includes Admin task operation rules |
| prd-supplement-v1.2.1.md | ✅ Created | Added feature description |
| project-control-doc.md | ✅ Updated | Updated doc index and version |
```

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-28 | Initial version |
