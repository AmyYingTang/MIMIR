# 文档依赖关系图模板

> **用途**: 定义项目文档之间的关联关系，用于一致性检查  
> **使用方式**: 当某个文档变更时，查阅此图以识别可能需要更新的其他文档

---

## 使用方法

1. 当你修改任何文档时，在 `documents` 部分找到该文档
2. 查看其 `triggers_review` 列表，了解哪些文档可能需要更新
3. 逐一检查列表中的文档是否需要同步修改
4. 使用 `变更检查清单` 记录你的检查结果

---

## 文档依赖关系定义

```yaml
# 项目文档依赖关系图
# 版本: v1.0

documents:
  # UI 原型
  ui_prototype:
    path: "prototype/"
    description: "UI 原型 HTML 文件"
    triggers_review:
      - api-design.md          # UI 新功能 → 可能需要新 API
      - state-machines.md      # UI 状态展示 → 可能涉及状态变更
      - business-rules.md      # UI 交互逻辑 → 可能涉及业务规则
      - prd.md                 # UI 功能 → PRD 功能描述
      - test-cases/            # UI 变更 → 需要更新测试用例
    change_signals:
      - "新增页面"
      - "新增操作按钮"
      - "新增状态展示"
      - "角色权限变化"

  # PRD 产品需求文档
  prd:
    path: "prd-*.md"
    description: "产品需求文档"
    triggers_review:
      - api-design.md          # 新需求 → 可能需要新 API
      - database-design.md     # 新需求 → 可能需要新表/字段
      - ui_prototype/          # 新需求 → 需要原型展示
      - state-machines.md      # 新状态/流程 → 状态机更新
      - business-rules.md      # 新规则 → 业务规则更新
    change_signals:
      - "新增功能模块"
      - "修改业务流程"
      - "新增角色或权限"
      - "修改校验规则"

  # API 设计文档
  api-design:
    path: "api-design.md"
    description: "API 接口设计文档"
    triggers_review:
      - database-design.md     # 新 API → 可能需要新表/字段
      - business-rules.md      # API 校验逻辑 → 业务规则
      - state-machines.md      # API 操作 → 状态变更
      - error-codes.md         # 新 API → 可能需要新错误码
    change_signals:
      - "新增接口"
      - "修改请求/响应格式"
      - "修改权限要求"
      - "新增错误码"

  # 数据库设计文档
  database-design:
    path: "database-design.md"
    description: "数据库设计文档"
    triggers_review:
      - api-design.md          # 新字段 → API 可能需要暴露
      - migration-scripts/     # 表结构变更 → 需要迁移脚本
    change_signals:
      - "新增表"
      - "新增字段"
      - "修改字段类型"
      - "修改索引"

  # 状态机定义
  state-machines:
    path: "state-machines.md"
    description: "状态机定义文档"
    triggers_review:
      - api-design.md          # 新状态转换 → 可能需要新 API
      - business-rules.md      # 状态规则 → 业务规则
      - database-design.md     # 新状态 → 可能需要枚举更新
    change_signals:
      - "新增状态"
      - "新增状态转换路径"
      - "修改转换条件"

  # 业务规则
  business-rules:
    path: "business-rules.md"
    description: "业务规则详述"
    triggers_review:
      - api-design.md          # 规则变更 → API 校验逻辑
      - test-cases/            # 规则变更 → 测试用例
    change_signals:
      - "新增校验规则"
      - "修改计算逻辑"
      - "修改约束条件"

  # 技术选型
  tech-stack:
    path: "tech-stack-*.md"
    description: "技术选型文档"
    triggers_review:
      - deployment/            # 技术栈变更 → 部署配置
      - api-design.md          # 框架变更 → API 实现方式
    change_signals:
      - "更换技术组件"
      - "升级版本"

  # 项目控制文档
  project-control:
    path: "project-control-doc.md"
    description: "项目控制文档"
    auto_update: true          # 其他文档更新时自动更新版本号和索引
    update_on_any_change: true
```

---

## 变更检查清单模板

在文档变更后使用此模板：

```markdown
## 变更检查清单

**变更文档**: [文档名称]
**变更内容**: [变更的简要描述]
**变更日期**: [YYYY-MM-DD]

### 自动检查项
根据文档依赖关系，以下文档需要检查：

- [ ] [文档1] - [状态: 需要更新 / 无需更新 / 已更新]
- [ ] [文档2] - [状态]
- ...

### 需要人工确认
- [ ] [需要人类判断的事项]
- ...

### 更新记录
| 文档 | 状态 | 备注 |
|------|:----:|------|
| xxx.md | ✅ 已更新 | [更新了什么] |
| yyy.md | ⏭️ 无需更新 | [原因] |
```

---

## 最佳实践

### 1. 提交前检查

在提交文档变更前：
- 执行依赖检查清单
- 更新所有受影响的文档
- 在项目控制文档中记录变更

### 2. 原子更新

尽可能在同一次提交中更新所有相关文档：
- 保持文档同步
- 便于追踪相关变更
- 简化回滚操作

### 3. 版本对齐

保持相关文档版本号一致：
- PRD v1.2 → API 设计 v1.2 → 数据库设计 v1.2
- 小更新使用补充文档（如 `api-design-supplement-v1.2.1.md`）

### 4. 定期审计

定期检查文档一致性：
- 每周：检查近期修改的文档
- 每月：完整一致性审计
- 大功能完成后：全面检查

---

## 示例：UI 原型变更

当 UI 原型新增 Admin 功能时：

```markdown
## 变更检查清单

**变更文档**: UI 原型 v2.0
**变更内容**: 新增 Admin 任务队列管理页面
**变更日期**: 2025-01-28

### 自动检查项
- [x] api-design.md → ❌ 缺少取消/重试/日志接口
- [x] state-machines.md → ❌ 缺少 Admin 取消的状态转换
- [x] business-rules.md → ❌ 缺少 Admin 操作规则
- [x] prd.md → ❌ 缺少功能描述

### 需要人工确认
- [ ] 「标记完成/失败」功能是否需要？ → 待专家确认

### 更新记录
| 文档 | 状态 | 备注 |
|------|:----:|------|
| api-design-supplement-v1.1.md | ✅ 已创建 | 补充 Admin 任务操作接口 |
| state-machines.md | ✅ 已创建 | 含任务状态机和 Admin 操作 |
| business-rules.md | ✅ 已创建 | 含 Admin 任务操作规则 |
| prd-supplement-v1.2.1.md | ✅ 已创建 | 补充功能说明 |
| project-control-doc.md | ✅ 已更新 | 更新文档索引和版本 |
```

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2025-01-28 | 初始版本 |
