# Retro: Cross-Module Empirical Validation of Quality Principles

> **Project**: MIMIR Methodology / Voice Model Personalization Platform  
> **Date**: 2026-02-05  
> **Type**: Empirical Validation (quality principles recurring in new modules)  
> **Contributors**: Project Team  
> **Modules Involved**: s-2-2 Permission Management Frontend & Integration

---

## Background

Quality principles #10, #11, and #12 were first discovered and extracted during s-1-1 and s-1-2. During s-2-2 (permission management module frontend + integration) development and validation, all three principles were independently re-validated — the same failure patterns recurred in a different module with different development context.

This is not "reviewing known issues" but rather **the same principles emerging naturally in new scenarios** — proving these are not isolated incidents but stable patterns in Agent behavior.

---

## Evidence 1: #11 FullStackFix — Fix Only Touches One Layer

### Original Principle (Extracted from s-1-2)

> Fix prompts must list changes for every affected layer. Agents tend to fix only one layer and stop.

### Recurrence in s-2-2

**Scenario**: s-2-2's fix prompt addressed backend permission validation integration logic, but did not create the corresponding frontend permission assignment page.

**Consequence**: Backend permission API was ready, but frontend had no matching UI. Required an additional `fix-s-2-2-permission-integration.md` to cover the full stack.

**Comparison with Original Instance**:

| | s-1-2 Instance | s-2-2 Instance |
|---|---|---|
| **Trigger** | Backend added `/chips/available`, frontend still called old endpoint | Backend permission validation integrated, frontend missing permission page |
| **Missed Layer** | Frontend calls + test cases + old endpoint cleanup | Frontend page + frontend API calls |
| **Root Cause** | Fix prompt only described backend changes | Fix prompt only described backend integration |

**Conclusion**: Pattern is identical. The Agent's tendency to "only fix the erroring layer" is not incidental — it's structural.

---

## Evidence 2: #12 InlineAPIContract — Response Structure Drift

### Original Principle (Extracted from s-1-2)

> Prompts must embed exact API request/response JSON schemas inline. Agents drift from referenced documents when generating large volumes of code.

### Recurrence in s-2-2

**Scenario**: `PermissionAssignment.vue` wrote `res.data.permissions` to unwrap the permission list, but the backend actually returns an array directly (not wrapped in a `.permissions` field).

**Root Cause**: Prompt said "refer to api-design.md" but did not inline the exact response schema. Agent added an extra `.permissions` unwrapping layer based on its own assumptions.

**Comparison with Original Instance**:

| | s-1-2 Instance | s-2-2 Instance |
|---|---|---|
| **API** | `GET /chips/available` | `GET /permissions` |
| **Drift Type** | Field names differ (`id` vs `chip_id`) | Nesting level differs (`.permissions` vs direct array) |
| **Root Cause** | Prompt didn't inline schema | Prompt didn't inline schema |

**Conclusion**: The Agent's drift manifests differently (field names vs nesting levels), but the root cause is identical.

---

## Evidence 3: #10 DiscrepancyReport — Cross-Module Convention Natural Closure

### Original Principle (Extracted from s-1-2)

> When discrepancies are found between reference documents, Agent resolves but must report the discrepancy and patch the source document.

### Validation in s-2-1/s-2-2

**Scenario**: s-1-2 review discovered `FUNCTION_TYPE_NAMES` was defined redundantly in three places (seed.py, API router, frontend constants) with inconsistent values. This was recorded in the conventions inconsistency list.

**Closure Path**: During s-2-1 task decomposition, scanning the inconsistency list revealed that P01 was already going to create `constants.py` (permission module constants). The Agent established an authoritative definition in P01 and cleaned up all three duplicates — zero extra cost, no dedicated "fix sprint."

**Pattern Validated**: The convention document as "cross-module fix queue" pattern was first validated in s-2-1. Issue discovery and fix occurred in different modules at different times, but were naturally bridged through the convention snapshot.

---

## Significance for MIMIR

### 1. Principle "Hardening"

When a principle first appears, it may be "just a lesson." When the same pattern recurs across different modules and contexts, the principle hardens from "experience" into "law."

Current hardening status:

| Principle | First Discovery | Second Validation | Status |
|-----------|-----------------|-------------------|--------|
| #10 DiscrepancyReport | s-1-2 | s-2-1 natural closure | ⬛ Hardened |
| #11 FullStackFix | s-1-2 | s-2-2 permission integration | ⬛ Hardened |
| #12 InlineAPIContract | s-1-2 | s-2-2 permission assignment page | ⬛ Hardened |

### 2. The Spiral Continues

This is a natural extension of the evolution chain recorded in `retro-methodology-spiral.md`:

```
s-1-1 → Quality Principles #1–#9 born
  ↓
s-1-2 → #10–#12 born + DependencyResolutionGate
  ↓
s-2-1 → Gate strengthened + convention-as-fix-queue validated + #10 closed loop
  ↓
s-2-2 → #11, #12 re-validated + #14 ErrorCodeFidelity born  ← this retro
```

Each module both consumes knowledge accumulated by prior modules and produces new knowledge for subsequent ones. The spiral continues upward.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2026-02-05 | Initial retro. Records #10/#11/#12 empirical validation in s-2-2 |
