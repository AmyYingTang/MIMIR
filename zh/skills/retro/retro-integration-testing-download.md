# 复盘记录：前后端联调与文件下载

> **日期**: 2026-02-04  
> **类型**: 技术实践 / 质量原则提炼  
> **贡献者**: 项目团队  
> **涉及模块**: 模型训练模块（含异步任务 + 文件下载）

---

## 背景

模型训练模块完成 Agent 执行后，进入人工验证测试阶段（smoke test + 前端手动验收）。在自动化测试（26/26 集成测试、6/6 smoke test）全部通过的情况下，前端手动验收时发现了多个 Agent 执行未暴露的问题。

---

## 发现的问题

### 问题 1：前后端 API 数据结构不匹配

**症状**：前端任务向导第 2 步功能类型列表为空

**根因**：后端未按 api-design.md 的字段定义实现接口
- 设计文档：`GET /api/v1/resources/available` 返回 `{resource_id, resource_model, available_functions: [{function_type, function_name}]}`
- 后端实际：`GET /api/v1/tasks/my-resources` 返回 `{id, model, functions: ["TypeA"]}`
- 前端按设计文档开发，字段名和嵌套结构完全不同

**提炼的原则**：**InlineAPIContract（#12）** — Prompt 必须内嵌精确的 API 请求/响应 JSON 模式。仅写"参考 api-design.md"不够，Agent 在生成大量代码时会偏离引用文档的细节。

### 问题 2：修复只改一层，前后端不同步

**症状**：修复后端 API 后，前端仍调旧端点 → 404

**根因**：fix prompt 只描述了后端改动，遗漏了前端调用端、测试用例、旧端点清理

**提炼的原则**：**FullStackFix（#11）** — 修复 Prompt 必须显式列出每一个受影响层的改动。Agent 天然倾向于"改完报错的那一层就停"。

### 问题 3：浏览器文件下载的坑

**症状**：点下载按钮显示"下载成功"，但 `~/Downloads` 里找不到文件（或文件名不正确）

**调试历程**：

| 尝试 | 方案 | 结果 |
|------|------|------|
| 1 | Axios Blob + `URL.createObjectURL` | Chrome 下载了文件但文件名是随机的 |
| 2 | 修复 Axios 拦截器（Blob 跳过解包） | 仍然文件名不对 |
| 3 | 后端 `?token=` query param + `window.open` | 开了空白标签页 |
| 4 | Hidden iframe 方案 | 下载了但 Chrome 不尊重 Content-Disposition |
| 5 | `<a>` 标签 + `click()` | 同上 |
| 6 | `window.location.href` | 同上 |
| 7 | 原生 `<a href>` 链接 | Chrome 144 仍不正确，但 Safari 正常 |

**根因**：Chrome 144 对 `Content-Disposition` 的 `filename*=utf-8''...` 编码处理存在 bug。当 ASCII fallback `filename` 包含 `?`（中文替换为问号）时，Chrome 拒绝使用该文件名。

**最终方案**：
- 后端：下载接口同时支持 `Authorization` header 和 `?token=` query param
- 前端：使用原生 `<a :href="url">` 链接，不通过 JS 触发
- ASCII fallback：用 `_` 替换非 ASCII 字符，而非 `?`
- Content-Disposition 同时包含 `filename` 和 `filename*`

**行业对比**：

| 方案 | 适用场景 |
|------|----------|
| 短时一次性下载 token | 推荐方案，安全性最好 |
| Signed URL (S3) | 云存储场景 |
| Cookie 认证 | 传统 Web 应用 |
| Query param 传 JWT | 内网小规模应用（当前选择） |

---

## 对 MIMIR 的改进

### 新增质量原则

| # | 原则 | 来源 |
|---|------|------|
| 11 | **FullStackFix** — 修复必须覆盖全链路每一层 | 问题 2 |
| 12 | **InlineAPIContract** — Prompt 内嵌精确 API 模式 | 问题 1 |

### 标准收尾步骤确认

每个 Prompt 必须以以下步骤结束：
1. `git commit`
2. 重建受影响的服务（`backend/*` → 构建后端，`frontend/*` → 构建前端，`docker-compose.yml` → `up -d`）

---

## 关键收获

1. **自动化测试通过 ≠ 功能正确** — 26/26 测试通过，但前端界面功能不可用。Agent 编写的测试可能与实际接口对齐但与设计文档不对齐
2. **前后端联调是不可跳过的验收环节** — 仅靠后端集成测试无法覆盖前后端字段对齐问题
3. **浏览器文件下载比想象中复杂** — Blob、iframe、`<a>` click、`window.open` 都有各自的兼容性陷阱。最可靠的方案是最简单的：原生 `<a href>` 链接
4. **修复往往比初始开发更容易出错** — 因为 Agent 只看到当前报错的那一层，缺乏全局视角。Fix prompt 必须强制列出所有受影响层

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2026-02-04 | 初始复盘记录 |
