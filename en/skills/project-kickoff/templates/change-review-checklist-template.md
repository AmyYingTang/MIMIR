# Change Review Checklist Template

> **Purpose**: Ensure document consistency after any significant change  
> **Usage**: Fill out this checklist after modifying any project document

---

## Change Review Checklist

**Changed Document**: _________________  
**Change Type**: [ ] New Feature | [ ] Bug Fix | [ ] Enhancement | [ ] Refactor  
**Change Description**: _________________  
**Change Date**: YYYY-MM-DD  
**Changed By**: _________________

---

## Step 1: Identify Affected Documents

Based on `doc-dependencies-template.md`, check which documents may need updates:

### If UI Prototype Changed:
- [ ] API Design - Does this UI need new/modified APIs?
- [ ] State Machines - Does this UI show new states or transitions?
- [ ] Business Rules - Does this UI implement new validation or logic?
- [ ] PRD - Is this feature documented in requirements?
- [ ] Test Cases - Do tests cover this new functionality?

### If PRD Changed:
- [ ] API Design - Do APIs support new requirements?
- [ ] Database Design - Is data model sufficient?
- [ ] UI Prototype - Is UI updated for new features?
- [ ] State Machines - Are new workflows defined?
- [ ] Business Rules - Are new rules documented?

### If API Design Changed:
- [ ] Database Design - Are required tables/fields present?
- [ ] Business Rules - Is validation logic documented?
- [ ] State Machines - Are state transitions via API defined?
- [ ] Error Codes - Are new error codes documented?

### If Database Design Changed:
- [ ] API Design - Should API expose new fields?
- [ ] Migration Scripts - Is migration script created?

### If State Machines Changed:
- [ ] API Design - Are transition APIs defined?
- [ ] Business Rules - Are transition rules documented?
- [ ] Database Design - Are status enums updated?

### If Business Rules Changed:
- [ ] API Design - Is validation implemented in API?
- [ ] Test Cases - Are rules covered by tests?

---

## Step 2: Review Each Affected Document

For each checked item above, evaluate:

| Document | Needs Update? | Action Required | Status |
|----------|:-------------:|-----------------|:------:|
| | [ ] Yes [ ] No | | ⬜ |
| | [ ] Yes [ ] No | | ⬜ |
| | [ ] Yes [ ] No | | ⬜ |
| | [ ] Yes [ ] No | | ⬜ |
| | [ ] Yes [ ] No | | ⬜ |

**Status Legend**: ⬜ Pending | 🔄 In Progress | ✅ Done | ⏭️ Skipped (with reason)

---

## Step 3: Document Updates Made

### Updates Completed

| Document | Version | Changes Made |
|----------|---------|--------------|
| | | |
| | | |
| | | |

### Items Requiring Human Decision

| Item | Question | Decision Maker | Status |
|------|----------|----------------|:------:|
| | | | ⬜ Pending |
| | | | |

### Items Intentionally Skipped

| Document | Reason for Skipping |
|----------|---------------------|
| | |

---

## Step 4: Update Project Control Document

- [ ] Updated document index with new/modified documents
- [ ] Updated version history
- [ ] Updated "Open Issues" if any pending decisions
- [ ] Updated "Communication & Decision Records"

---

## Step 5: Final Verification

- [ ] All affected documents have been reviewed
- [ ] All necessary updates have been made
- [ ] Version numbers are consistent
- [ ] Project control document reflects current state
- [ ] Changes are ready for commit/submission

---

## Notes

_Add any additional notes, context, or follow-up items here:_

---

## Sign-off

**Reviewer**: _________________  
**Review Date**: _________________  
**Approved**: [ ] Yes | [ ] No - Reason: _________________

---

## Quick Reference: Common Change Patterns

### Pattern 1: New Feature
```
PRD → API Design → Database Design → State Machine → UI Prototype → Test Cases
```

### Pattern 2: Bug Fix in Business Logic
```
Business Rules → API Design → Test Cases
```

### Pattern 3: UI Improvement
```
UI Prototype → (possibly) API Design → Test Cases
```

### Pattern 4: Performance Optimization
```
Database Design → API Design → Deployment Docs
```

### Pattern 5: Security Enhancement
```
Security Design → API Design → Business Rules → Test Cases
```

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-28 | Initial version |
