# Skill: Review Agent — Independent Code Review

> **Version**: v0.1  
> **Created**: 2026-02-04  
> **Last Updated**: 2026-02-04  
> **Category**: Quality Assurance  
> **Runtime**: MIMIR-BO `review-agent/`

---

## Purpose

After a module is built by runprompt-agent (or any code-generating agent), an **independent** review step compares the design documents against the actual code to catch discrepancies that the building agent's self-tests cannot detect.

**Key insight**: The agent that writes code and the agent that writes tests share the same context and biases. They align with each other but may both drift from the design documents. An independent reviewer with a fresh context, using design docs as the sole source of truth, catches what self-testing misses.

---

## When to Use

```
runprompt-agent executes prompts → [code generated] → ★ review-agent ★ → human acceptance
```

Trigger after **every module completion**, before human acceptance testing.

---

## Review Dimensions

| # | Dimension | What It Catches | Origin |
|---|-----------|----------------|--------|
| 1 | **API Contract Alignment** | Field names, paths, response structure mismatches between design doc / backend / frontend | s-1-2: `available_functions` vs `functions` |
| 2 | **Shared Data Consistency** | Credentials, ports, DB names inconsistent across config files, seeds, tests | s-1-2: `Test@2025` vs `Trainer@2025` |
| 3 | **Frontend-Backend Field Alignment** | Frontend calls URL/fields that backend doesn't actually serve | s-1-2: frontend called `/tasks/my-chips`, backend had `/chips/available` |
| 4 | **State & Enum Consistency** | DDL ENUM values ≠ state machine doc ≠ backend constants | s-1-2: 4 vs 5 status values |
| 5 | **Test Coverage Sanity** | Tests exist but assert wrong field names; fixtures use wrong credentials | s-1-2: 26/26 passed but frontend broken |

These dimensions are extensible. Add new ones as new failure patterns are discovered in practice.

---

## Design Principles

1. **Independence** — The reviewer MUST NOT share context with the building agent. It starts with a blank slate, reads only design docs and code.

2. **Design-doc-is-truth** — When design and code disagree, design doc is the authority. The review report flags the code as non-conforming, not the design doc.

3. **Read-only** — The reviewer does NOT modify any code. Its only output is a review report file.

4. **Exhaustive** — Every endpoint, every shared value, every enum must be checked. "Looks fine" is not acceptable; explicit PASS/FAIL for each item.

5. **Precise** — Every finding includes file path, line number, expected value, and actual value.

---

## Report Format

Output: `review-report-<module>.md` in project root.

```
Summary:  ✅ N PASS  |  ⚠️ N WARN  |  ❌ N FAIL

Per finding:
- Status icon (✅/⚠️/❌)
- Check dimension (#1-5)
- Description
- File:line
- Expected vs Actual
- Suggested fix (for ❌ only)

Footer: Priority-ordered action list
```

---

## Relationship to MIMIR Quality Principles

This skill automates what several quality principles recommend doing manually:

| Quality Principle | Review Agent Check |
|-------------------|--------------------|
| #10 DiscrepancyReport | Check 1, 4 (cross-doc discrepancies) |
| #11 FullStackFix | Check 1, 3 (ensures all layers are aligned) |
| #12 InlineAPIContract | Check 1 (verifies API contract fidelity) |

---

## Runtime Implementation

The runtime implementation lives in **MIMIR-BO** (`review-agent/` directory):
- `review-prompt.md` — The prompt template sent to Claude Code
- `review.sh` — Runner script that fills template variables and invokes Claude Code CLI

**Consistency rule**: Changes to review dimensions or design principles in this SKILL.md must be reflected in the BO prompt template, and vice versa.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v0.1 | 2026-02-04 | Initial version. 5 review dimensions extracted from s-1-1 and s-1-2 experience |
