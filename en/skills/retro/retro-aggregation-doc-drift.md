# Retrospective: Aggregation Document Drift

> **Project**: MIMIR Methodology Repository  
> **Date**: 2026-02-04  
> **Type**: Process Improvement (Dogfooding)  
> **Contributor**: Project Team

---

## Background

MIMIR's own documentation suffered from the exact consistency problem it teaches others to avoid. The SKILL-INDEX file — which serves as the central directory of all skills — fell 5 versions behind the rest of the repository (stuck at v1.3 while the repo reached v1.7).

### The Incident

1. v1.3 (2026-01-30): meta-knowledge/ skill added. SKILL-INDEX updated.
2. v1.4 (2026-01-31): claude-code-prompt/ skill added. README updated. **SKILL-INDEX not updated.**
3. v1.5–v1.7 (2026-02-01~02): Multiple skill updates, UI/UX principles added. READMEs updated each time. **SKILL-INDEX still at v1.3.**
4. v1.8 (2026-02-04): review-agent/ skill added. Discovery: SKILL-INDEX structure tree was missing 3 skill directories entirely.

The irony: MIMIR contains `doc-dependencies-template.md` and `change-review-checklist-template.md` specifically designed to prevent this. They were not applied to MIMIR's own files.

---

## Root Cause Analysis

The real pattern is not about "index files" specifically. It's about **aggregation documents** — files that summarize information from multiple sources.

### What Makes Aggregation Documents Drift-Prone

| Property | Why It Causes Drift |
|----------|---------------------|
| **Not the "main character"** | When you add a skill, the SKILL.md is the primary deliverable. The index is a side effect. |
| **No ownership signal** | Changing `claude-code-prompt/SKILL.md` doesn't mentally trigger "also update SKILL-INDEX". The dependency is invisible. |
| **Still correct enough** | A stale index doesn't break anything immediately. It reads fine. The staleness is silent. |
| **Updated by everyone, owned by no one** | Every skill addition should update it, but no single skill "owns" it. |

### General Project Equivalents

This is not unique to methodology repos. The same pattern appears in:

- **API documentation hubs** — Individual endpoint docs stay current, but the summary page listing all endpoints falls behind
- **CHANGELOG / release notes** — Features ship, changelog gets updated "later" (i.e. never)
- **README feature lists** — New features added to code, README still describes v1.0
- **Configuration reference docs** — New config options added, reference doc not updated
- **Architecture decision records (ADR) index** — Individual ADRs written, index not updated
- **Onboarding docs** — Systems change, onboarding guide still describes old setup

**Common trait**: these files aggregate information whose source of truth lives elsewhere. They are always derivative, never primary.

---

## What We Learned

### 1. Aggregation Documents Need Explicit Triggers

The dependency graph from our earlier retro (`retro-doc-consistency.md`) correctly identified that documents have hidden dependencies. But it focused on **peer dependencies** (UI prototype → API design). This case reveals a different dependency type: **child → parent aggregation**.

```
Peer dependency:     API design ←→ State machine     (both are primary sources)
Aggregation dependency:  SKILL.md → SKILL-INDEX      (child feeds parent summary)
```

Aggregation dependencies are harder to catch because the child file is complete and correct on its own. The parent's staleness is invisible from the child's perspective.

### 2. "Proximity Bias" in Updates

When making a change, people (and AI agents) naturally update files that are "close" to the change:

```
Distance from change:
  Near:   SKILL.md (the file you just created)         ← Always updated
  Medium: README.md (mentions this skill category)      ← Usually updated
  Far:    SKILL-INDEX.md (lists all skills)             ← Often forgotten
```

The further a file is from the point of change, the more likely it is to be forgotten. This is a cognitive bias, not a process failure — which means process must compensate for it.

### 3. Silent Staleness is Worse Than Loud Breakage

A stale SKILL-INDEX doesn't throw an error. It still renders, still reads well, still "works". The only symptom is that new skills are invisible to anyone navigating via the index. This silent failure mode makes it particularly dangerous — you don't know it's wrong until someone actually tries to use it as an entry point.

---

## Solution: Aggregation Document Checklist

### Add to Change Review Process

When any **new file or directory** is added to a project:

```
□ Identify all aggregation documents that reference this file's parent scope
□ Update each aggregation document's:
    □ Directory/structure tree
    □ Summary tables
    □ Version history
□ If the aggregation document has its own version number, bump it
```

### Concrete MIMIR Rule

For MIMIR specifically, any change that adds/removes/renames a skill directory MUST update:

1. `SKILL-INDEX.md` (both languages) — structure tree + available skills table + version history
2. `MIMIR-README.md` (both languages) — directory tree + project types table + version history
3. `README.md` (root) — structure overview + supported types + version history

### General Project Application

For any project, identify your aggregation documents during setup:

| Aggregation Document | Triggered By |
|----------------------|--------------|
| API reference index | Any new/removed endpoint |
| CHANGELOG | Any user-facing change |
| README feature list | Any new feature |
| Config reference | Any new config option |
| Architecture overview | Any new service/component |

Add these as explicit checklist items in your commit/review process.

---

## Key Takeaways

1. **Aggregation documents drift silently** — they are derivative, not primary, so staleness doesn't break anything visibly
2. **Proximity bias** — the further a file is from the point of change, the more likely it is to be forgotten
3. **Process must compensate for cognitive bias** — explicit checklists, not human memory
4. **Eat your own dog food** — if your methodology teaches consistency, apply it to the methodology itself

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2026-02-04 | Initial retrospective. Extracted from MIMIR dogfooding experience |
