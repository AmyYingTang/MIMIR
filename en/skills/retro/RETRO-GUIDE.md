# Project Retrospective Extraction Guide

> **Purpose**: Extract reusable experience from completed projects, update methodology library  
> **When to Use**: After project completion, after milestone achievements, when encountering significant pitfalls  
> **Goal**: Don't let knowledge slip away, don't step in the same hole twice

---

## 1. Why Retrospective Extraction?

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Knowledge Loss vs Knowledge Accumulation                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  No Retrospective                        With Retrospective                 │
│  ────────────────                        ──────────────────                 │
│  Project A pitfall → Forget → Project B  Project A pitfall → Record cause  │
│       same pitfall again                      and solution → Update Skill   │
│       ↓                                           ↓                 ↓       │
│  "This problem seems familiar..."        Project B startup ← Auto avoid ←  │
│                                               Read Skill                    │
│                                                                             │
│  Result: Same mistakes repeated          Result: Knowledge accumulates,    │
│                                                  efficiency improves        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Retrospective Timing

| Timing | Retrospective Type | Depth |
|--------|-------------------|-------|
| **Project Completion** | Full retrospective | Deep |
| **Phase Completion** | Phase retrospective | Medium |
| **When Hitting Pitfall** | Immediate record | Quick |
| **Good Practice Discovered** | Immediate record | Quick |

---

## 3. Retrospective Guiding Questions

### 3.1 Overall Review

```markdown
Let's review this project:

**Project Overview**
1. What is the project name?
2. Project type? (Web/App/CLI etc.)
3. Project duration? (Start to finish)
4. Team size?

**Goal Achievement**
5. What was the original goal?
6. Was it achieved? Any deviation?
7. If there was deviation, what was the reason?
```

### 3.2 Phase-by-Phase Review

```markdown
**Requirements Analysis Phase**
1. Was requirements analysis thorough?
2. Any requirements discovered later that were missed?
3. Many requirement changes? What caused them?
4. What questions should be asked next time?

**Technology Selection Phase**
5. Was the tech selection correct?
6. Any wrong tech choices that were changed later?
7. Any technical debt caused by selection?
8. What should be noted in next selection?

**System Design Phase**
9. Any rework during design phase?
10. Any scenarios not considered during design?
11. Many interface design adjustments needed?
12. Many database design changes needed?

**Implementation Phase**
13. Was development smooth?
14. Any problems that took long to resolve?
15. Any good code practices worth keeping?
16. Any code structures found to be problematic later?

**Testing & Deployment**
17. Was test coverage sufficient?
18. Was the launch process smooth?
19. Any issues discovered only after launch?
```

### 3.3 Pitfalls & Learnings

```markdown
**Pitfalls**
1. What was the biggest pitfall in this project?
2. What wasted the most time?
3. What had the most rework?
4. If starting over, how would you avoid these pitfalls?

**Learnings & Highlights**
5. What was done best in this project?
6. Any new technologies or methods learned?
7. Any code/designs reusable for other projects?
8. Any practices worth promoting?
```

### 3.4 Methodology Update Suggestions

```markdown
**Update Suggestions**
Based on this project experience, do you think the methodology should:

1. **Add**: What content should be added that's not in the methodology?
   - New checklist items?
   - New decision tree branches?
   - New best practices?

2. **Modify**: What content needs updating?
   - Outdated recommendations?
   - Priorities need adjusting?
   - Need more explanation?

3. **Emphasize**: What content is in the methodology but easily overlooked?
   - Need to bold/highlight?
   - Need warning?
   - Need examples?
```

---

## 4. Extraction Template

After retrospective, record methodology updates using this template:

```markdown
# Retrospective Extraction Record

## Project Information
- **Project Name**: [Name]
- **Project Type**: [Type]
- **Retrospective Date**: [Date]
- **Retrospective By**: [Name]

---

## Pitfall Records

### Pitfall 1: [Brief Title]

**Situation Description**:
What happened?

**Root Cause**:
Why did it happen?

**Solution**:
How was it resolved?

**Lesson Learned**:
How to avoid next time?

**Suggested Update**:
- [ ] Update to `[filename]` section `[section]`
- Update content: [Specific content]

---

### Pitfall 2: [Brief Title]
...

---

## Good Practices

### Practice 1: [Brief Title]

**Practice Description**:
What was specifically done?

**Effect**:
What benefits did it bring?

**Applicable Scenarios**:
When is it applicable?

**Suggested Update**:
- [ ] Add to `[filename]` section `[section]`
- New content: [Specific content]

---

## Methodology Update Checklist

| Update Type | Target File | Update Content | Status |
|-------------|-------------|----------------|:------:|
| Add | `xxx.md` | [Content summary] | ⬜ |
| Modify | `xxx.md` | [Content summary] | ⬜ |
| Delete | `xxx.md` | [Content summary] | ⬜ |
```

---

## 5. Common Extractable Content

### 5.1 Checklist Additions

```markdown
Discovered missing checklist items:
- "We forgot to consider XXX, only discovered after launch"
→ Update to corresponding checklist

Examples:
- "Forgot to consider database timezone issues" → Add to database design checklist
- "Forgot to handle concurrent locks" → Add to API design checklist
```

### 5.2 Decision Tree Additions

```markdown
Discovered new decision branches:
- "We encountered XXX scenario, existing decision tree didn't cover it"
→ Add decision tree branch

Examples:
- "Need to handle large file uploads" → Add to tech selection decision tree
```

### 5.3 Best Practice Additions

```markdown
Discovered good approaches:
- "We used XXX method, worked great"
→ Record as best practice

Examples:
- "Using UUID as primary key avoided ID exposure issues"
- "Separating config from enums made feature launch config-only"
```

### 5.4 Anti-Pattern Records

```markdown
Discovered approaches to avoid:
- "We did XXX, turned out badly"
→ Record as anti-pattern/warning

Examples:
- "Directly depending on concrete implementation made switching services painful"
- "Not masking logs caused sensitive information leak"
```

---

## 6. Update Process

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      Methodology Update Process                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. Retrospective          2. Extract              3. Update                │
│  ┌─────────┐              ┌─────────┐              ┌─────────┐             │
│  │ Answer  │      →      │ Fill in │      →      │ Update  │             │
│  │ guiding │              │ template│              │ files   │             │
│  │ questions│              │ record  │              │         │             │
│  └─────────┘              └─────────┘              └─────────┘             │
│                                                          │                  │
│                                                          ▼                  │
│                                                    4. Mark Version          │
│                                                    ┌─────────┐             │
│                                                    │ Update  │             │
│                                                    │ version │             │
│                                                    │ history │             │
│                                                    └─────────┘             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Version Marking Standards

When updating methodology files, record in version history:

```markdown
## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
| v1.1 | 2025-02-15 | Added database timezone checklist item | XX Project |
| v1.2 | 2025-03-01 | Added large file upload decision branch | YY Project |
```

---

## 8. Agent Usage Instructions

When user says "I want to do a project retrospective" or "update methodology":

1. **Confirm project info**: Ask project name, type, duration
2. **Guide review**: Guide step by step through questions 3.1-3.4
3. **Fill template**: Organize user's answers into extraction record
4. **Confirm updates**: Confirm with user what content to update
5. **Execute updates**: Update corresponding Skill files
6. **Mark version**: Record this update in version history

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2025-01-27 | Initial version |
