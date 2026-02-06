# MIMIR Open Issues / 待解决问题

> *Methodology-level issues that have been identified but not yet resolved. These are not bugs — they are design gaps discovered through real project execution.*
>
> *方法论层面已识别但尚未解决的问题。这些不是 bug，而是在真实项目执行中发现的设计空白。*

---

## How to Use This Document / 如何使用本文档

每条 issue 记录问题描述、发现场景、当前状态和可能的探索方向。当某条 issue 被解决后，将其移入文末的「已关闭」区域并注明解决方案。

Each issue records the problem description, discovery context, current status, and possible exploration directions. When an issue is resolved, move it to the "Closed" section at the bottom with a note on the solution.

---

## Open / 未解决

### MIMIR-ISSUE-001: Prompt Spec Fidelity / Prompt 规格保真度

**发现日期 / Discovered**: 2026-02-02  
**发现场景 / Context**: s-1-1 用户认证模块任务分解与执行  
**相关 Skill / Related Skill**: claude-code-prompt

**问题描述 / Problem**:

当 Task Decompose 生成的 Prompt 中包含具体配置值（端口号、路径、服务名等）时，Agent 执行时可能不严格遵循这些值，而是用自己的"常识"替换。例如 Prompt 指定 `app_port:8000`，但 Agent 可能在某些文件中写成 `3000` 或 `5000`。

When a Task Decompose-generated prompt contains specific config values (port numbers, paths, service names, etc.), the Agent may not strictly follow these values during execution, substituting its own "common sense" instead. For example, the prompt specifies `app_port:8000`, but the Agent might write `3000` or `5000` in some files.

**为什么难 / Why It's Hard**:

这触及了 LLM 的一个根本特性：它是概率模型，不是精确指令执行器。即使 Prompt 写得很明确，模型在生成大量代码时仍可能"漂移"。这不是写更好的 Prompt 就能完全解决的。

This touches a fundamental LLM characteristic: it's a probabilistic model, not a precise instruction executor. Even with a very explicit prompt, the model may "drift" when generating large amounts of code. This isn't something that can be fully solved by writing better prompts alone.

**可能的探索方向 / Possible Directions**:

- 在 Prompt 中用醒目格式强调关键配置值（如 `⚠️ CRITICAL: port must be 8000`）
- Agent 执行后增加自动化校验步骤：扫描产出文件中的关键值是否与 Prompt 规格一致
- 将关键配置值提取到独立的 config 文件，Prompt 中引用而非内联
- 在 MIMIR 层面这是抽象原则；具体实现策略应在 enterprise-web skill 或 claude-code-prompt skill 中落地

In prompts, emphasize critical config values with prominent formatting (e.g., `⚠️ CRITICAL: port must be 8000`). Add automated post-execution validation: scan output files to verify key values match the prompt spec. Extract key config values to a standalone config file, referencing rather than inlining in prompts. At the MIMIR level this remains an abstract principle; concrete implementation strategies should land in enterprise-web or claude-code-prompt skills.

**当前状态 / Status**: 🔴 无解决方案，需进一步探索 / No solution yet, needs further exploration

---

### MIMIR-ISSUE-002: Change Management Flow / 变更管理流程

**发现日期 / Discovered**: 2026-02-02  
**发现场景 / Context**: s-1-1 执行过程中发现需求变更（如 refresh_token 合并到 database-design.md）需要回溯修改多个已生成的 Prompt  
**相关 Skill / Related Skill**: claude-code-prompt, project-kickoff

**问题描述 / Problem**:

MIMIR 当前只有"正向流程"：设计文档 → Task Decompose → Prompt → 执行。但没有定义"变更流程"：当设计文档发生变更时，已分解的 Prompt 应该如何更新？

MIMIR currently only has a "forward flow": Design Docs → Task Decompose → Prompts → Execute. But there's no defined "change flow": when a design document changes, how should already-decomposed prompts be updated?

目前的做法是全量重新分解，这在 Prompt 数量多时代价很高，而且会丢失已有 Prompt 中积累的执行经验和修正。

The current approach is full re-decomposition, which is expensive when there are many prompts, and it loses the execution experience and corrections accumulated in existing prompts.

**理想模型 / Ideal Model**:

```
变更触发 → 影响分析 → 增量 Prompt 补丁 → 执行补丁
Change Trigger → Impact Analysis → Delta Prompt Patch → Execute Patch
```

具体来说：

Specifically:

1. **变更触发 / Change Trigger**: 某个设计文档的某个 section 被修改
2. **影响分析 / Impact Analysis**: 自动识别哪些已有 Prompt 引用了被修改的内容
3. **增量补丁 / Delta Prompt**: 只生成差异部分的修改指令，而非重新生成整个 Prompt
4. **执行补丁 / Execute Patch**: Agent 执行补丁时，只修改受影响的文件和代码段

**为什么难 / Why It's Hard**:

- 需要建立设计文档 section 与 Prompt 内容之间的双向追溯关系
- "增量补丁"的粒度很难把握——改太少可能遗漏级联影响，改太多等于全量重做
- LLM 生成的 Prompt 不像代码有明确的 diff，难以做精确的增量比对

Requires establishing bidirectional traceability between design document sections and prompt content. The granularity of "delta patches" is hard to calibrate — too little may miss cascading impacts, too much is effectively a full redo. LLM-generated prompts don't have clear diffs like code, making precise incremental comparison difficult.

**可能的探索方向 / Possible Directions**:

- 在 Task Decompose 输出中增加"引用映射"：每个 Prompt 标注它依赖设计文档的哪些 section
- 设计一种"Prompt Patch"格式：描述对已有 Prompt 的最小修改
- 借鉴数据库 migration 的思路：变更是可堆叠、可回滚的增量操作
- 先从简单场景验证：单个配置值变更 → 自动定位受影响 Prompt → 生成替换指令

Add "reference mapping" to Task Decompose output: each prompt annotates which design document sections it depends on. Design a "Prompt Patch" format: describe minimal modifications to existing prompts. Borrow from database migration thinking: changes are stackable, rollback-able incremental operations. Start validation with simple scenarios: single config value change → auto-locate affected prompts → generate replacement instructions.

**当前状态 / Status**: 🔴 概念阶段，需设计具体方案 / Conceptual stage, needs concrete design

---

### MIMIR-ISSUE-003: Browser File Download Compatibility / 浏览器文件下载兼容性

**发现日期 / Discovered**: 2026-02-04  
**发现场景 / Context**: s-1-2 模型训练模块前端验证测试，文件下载功能在 Chrome 144 上文件名异常  
**相关 Skill / Related Skill**: claude-code-prompt, enterprise-web

**问题描述 / Problem**:

浏览器文件下载看似简单，但实际存在大量兼容性陷阱。在 s-1-2 中尝试了 7 种方案（Blob、iframe、`<a>` click、`window.open`、`window.location.href`、原生 `<a href>`），最终发现 Chrome 144 对 `Content-Disposition` 的 `filename*=utf-8''...` 处理存在 bug，Safari 正常。

Browser file download seems simple but has numerous compatibility traps. In s-1-2, we tried 7 approaches (Blob, iframe, `<a>` click, `window.open`, `window.location.href`, native `<a href>`), ultimately discovering that Chrome 144 has a bug handling `Content-Disposition`'s `filename*=utf-8''...` encoding, while Safari works correctly.

**对 MIMIR 的影响 / Impact on MIMIR**:

Agent 生成的文件下载代码往往采用 Blob + `URL.createObjectURL` 方案，这在实际浏览器中不可靠（Chrome 安全策略导致 `Content-Disposition` 文件名不被尊重）。Prompt 需要明确指定下载方案，而非让 Agent 自由选择。

Agent-generated file download code typically uses Blob + `URL.createObjectURL`, which is unreliable in actual browsers (Chrome security policies cause `Content-Disposition` filenames to be ignored). Prompts need to specify the download approach explicitly rather than letting the Agent choose freely.

**当前推荐方案 / Current Recommendation**:

- 后端支持 `?token=` query param 认证（优先 Authorization header，fallback query param）
- 前端使用原生 `<a :href="url">` 链接（非 JS 触发）
- `Content-Disposition` 同时包含 ASCII `filename` 和 UTF-8 `filename*`
- ASCII fallback 用 `_` 替换非 ASCII 字符

Backend supports `?token=` query param auth (prioritize Authorization header, fallback to query param). Frontend uses native `<a :href="url">` link (not JS-triggered). Content-Disposition includes both ASCII `filename` and UTF-8 `filename*`. ASCII fallback replaces non-ASCII chars with `_`.

**待验证 / To Verify**:

- Chrome 后续版本是否修复了 `filename*` 编码处理
- 短时一次性下载 token 方案是否能绕过问题
- 是否需要将此作为 enterprise-web skill 的标准技术决策

Whether later Chrome versions fix the `filename*` encoding handling. Whether short-lived one-time download tokens bypass the issue. Whether this should become a standard technical decision in the enterprise-web skill.

**当前状态 / Status**: 🟢 已固化为 Prompt 模板片段 / Codified as Prompt template snippet

**解决方案 / Resolution**:

已验证的下载模式（`?token=` query param + 原生 `<a href>` + Content-Disposition 双 filename）已固化为标准 Prompt 模板片段，后续涉及文件下载的 Prompt 可直接引用：

The validated download pattern (`?token=` query param + native `<a href>` + Content-Disposition dual filename) has been codified as a standard Prompt template snippet. Future prompts involving file downloads can reference directly:

- `zh/skills/claude-code-prompt/templates/file-download-pattern.md`
- `en/skills/claude-code-prompt/templates/file-download-pattern.md`

s-2-2 中申请文件下载再次踩了"路由不存在 + token 不支持 query param"的坑，验证了模板化的必要性。

s-2-2 application file download hit the same "route not registered + token not supported via query param" pitfall again, validating the need for templatization.

---

## Closed / 已关闭

*暂无 / None yet*

---

## Document History / 文档历史

| 版本 Version | 日期 Date | 更新 Updates |
|-------------|-----------|-------------|
| v1.0 | 2026-02-02 | 初始版本，记录 2 个 open issues（Prompt Spec Fidelity、Change Management Flow）/ Initial version with 2 open issues |
| v1.1 | 2026-02-04 | 新增 ISSUE-003 浏览器文件下载兼容性 / Added ISSUE-003 Browser File Download Compatibility |
| v1.2 | 2026-02-05 | ISSUE-003 状态从 🟡 升级为 🟢：下载模式已固化为 Prompt 模板片段 / ISSUE-003 status upgraded from 🟡 to 🟢: download pattern codified as Prompt template snippet |
