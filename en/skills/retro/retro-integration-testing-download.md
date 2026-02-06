# Retrospective: Frontend-Backend Integration & File Download

> **Date**: 2026-02-04  
> **Type**: Technical Practice / Quality Principle Extraction  
> **Contributor**: Project Team  
> **Module**: Model Training Module (async tasks + file download)

---

## Background

After Agent execution of the model training module, we entered manual verification testing (smoke test + frontend manual acceptance). Despite all automated tests passing (26/26 integration, 6/6 smoke), frontend manual acceptance revealed multiple issues that Agent execution had not exposed.

---

## Issues Discovered

### Issue 1: Frontend-Backend API Data Structure Mismatch

**Symptom**: Task wizard step 2 function type list was empty

**Root Cause**: Backend did not implement the interface according to api-design.md field definitions
- Design doc: `GET /api/v1/resources/available` returns `{resource_id, resource_model, available_functions: [{function_type, function_name}]}`
- Backend actual: `GET /api/v1/tasks/my-resources` returns `{id, model, functions: ["TypeA"]}`
- Frontend developed per design doc; field names and nesting structure were completely different

**Principle Extracted**: **InlineAPIContract (#12)** — Prompts must embed exact API request/response JSON schemas inline. Just writing "refer to api-design.md" is insufficient; Agents drift from referenced doc details when generating large code volumes.

### Issue 2: Fix Only Touches One Layer, Frontend-Backend Out of Sync

**Symptom**: After fixing the backend API, frontend still called old endpoint → 404

**Root Cause**: Fix prompt only described backend changes, missing frontend call site, test cases, and old endpoint cleanup

**Principle Extracted**: **FullStackFix (#11)** — Fix prompts must explicitly list changes for every affected layer. Agents naturally tend to "fix the layer that errored and stop."

### Issue 3: Browser File Download Pitfalls

**Symptom**: Download button shows "download success" but no file found in `~/Downloads` (or filename incorrect)

**Debugging Journey**:

| Attempt | Approach | Result |
|---------|----------|--------|
| 1 | Axios Blob + `URL.createObjectURL` | Chrome downloaded file but with random filename |
| 2 | Fix Axios interceptor (skip unwrap for Blob) | Filename still wrong |
| 3 | Backend `?token=` query param + `window.open` | Opened blank tab |
| 4 | Hidden iframe approach | Downloaded but Chrome didn't respect Content-Disposition |
| 5 | `<a>` tag + `click()` | Same issue |
| 6 | `window.location.href` | Same issue |
| 7 | Native `<a href>` link | Chrome 144 still incorrect, but Safari works fine |

**Root Cause**: Chrome 144 has a bug handling `Content-Disposition`'s `filename*=utf-8''...` encoding. When the ASCII fallback `filename` contains `?` (Chinese chars replaced with question marks), Chrome refuses to use that filename.

**Final Solution**:
- Backend: Download endpoint supports both `Authorization` header and `?token=` query param
- Frontend: Use native `<a :href="url">` link, no JS-triggered download
- ASCII fallback: Replace non-ASCII chars with `_` instead of `?`
- Content-Disposition includes both `filename` and `filename*`

**Industry Comparison**:

| Approach | Use Case |
|----------|----------|
| Short-lived one-time download token | Recommended, best security |
| Signed URL (S3) | Cloud storage scenarios |
| Cookie auth | Traditional web apps |
| Query param with JWT | Intranet small-scale apps (current choice) |

---

## MIMIR Improvements

### New Quality Principles

| # | Principle | Source |
|---|-----------|--------|
| 11 | **FullStackFix** — Fixes must cover every layer in the stack | Issue 2 |
| 12 | **InlineAPIContract** — Embed exact API schemas inline in prompts | Issue 1 |

### Standard Prompt Closing Steps Confirmed

Every prompt must end with:
1. `git commit`
2. Rebuild affected services (`backend/*` → build backend, `frontend/*` → build frontend, `docker-compose.yml` → `up -d`)

---

## Key Takeaways

1. **Automated tests passing ≠ feature correctness** — 26/26 tests passed, but frontend UI was non-functional. Agent-written tests may align with actual interface but not with design docs
2. **Frontend-backend integration is a non-skippable acceptance step** — Backend integration tests alone cannot cover frontend-backend field alignment issues
3. **Browser file download is more complex than expected** — Blob, iframe, `<a>` click, `window.open` all have compatibility traps. The most reliable approach is the simplest: native `<a href>` link
4. **Fixes are more error-prone than initial development** — Because the Agent only sees the currently erroring layer and lacks global perspective. Fix prompts must mandate listing all affected layers

---

## Version History

| Version | Date | Updates |
|---------|------|---------|
| v1.0 | 2026-02-04 | Initial retrospective |
