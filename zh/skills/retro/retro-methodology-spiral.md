# 复盘记录：方法论的螺旋演进

> **项目**: MIMIR 方法论 / 语音模型个性化构建平台  
> **日期**: 2025-02-05  
> **类型**: L4 元观察（自我进化层面）  
> **贡献者**: 项目团队

---

## 背景

MIMIR 的 PHILOSOPHY.md 中有一句话："这不是设计出来的，是长出来的。"

这份复盘记录了一条**完整的、可追溯的"长出来"的实例**——从第一个模块的坑，到跨模块的自动闭环修复——作为 MIMIR L4（自我进化）的案例素材。

---

## 演进链路

```
s-1-1 踩坑
  │ 密码不一致、路径不对齐、环境不匹配……
  ↓
质量原则诞生（#1–#9）
  │ DiscrepancyReport、HostEnvAlign、InlineAPIContract 等
  ↓
s-1-2 任务分解
  │ DependencyResolutionGate 拦截跨模块依赖
  ↓
s-1-2 review
  │ 发现 FUNCTION_TYPE_NAMES 三处重复、种子数据角色覆盖不全
  ↓
记入 conventions inconsistency 列表
  │ 不是设计文档规定的，是代码审查中涌现的
  ↓
s-2-1 任务分解
  │ 扫描 inconsistency 列表，发现 P01 恰好要建 constants.py
  ↓
s-2-1 P01 执行
  │ 建立权威定义 + 清理三处重复 → 闭环
  │ 零额外成本。没有专门的"修复冲刺"。
  ↓
反馈到 MIMIR
  │ Gate 就绪定义强化、convention-as-fix-queue 模式记录、Alembic 陷阱记录
  ↓
方法论版本递增
  │ claude-code-prompt v2.5 → v2.6
  │ convention-extraction v0.1 → v0.2
  ↓
s-2-2 验证测试
  │ #11 FullStackFix 再次验证：fix prompt 只改后端，缺前端权限分配页
  │ #12 InlineAPIContract 再次验证：前端 .permissions 解包 vs 后端直接返回数组
  │ 422→40100 误映射 → 新原则 #14 ErrorCodeFidelity 诞生
  │ 前端硬编码值与后端约束批量不一致 → #12 增强（参数约束子场景）
  ↓
反馈到 MIMIR
  │ 原则 #10/#11/#12 从"经验"硬化为"规律"（跨模块二次验证）
  ↓
方法论版本递增
  claude-code-prompt v2.6 → v2.7
```

---

## 为什么这条链路值得记录

### 1. 每个节点都是"被逼出来的"

没有哪一步是预先规划的：

- 质量原则不是坐下来"头脑风暴"出来的，是 s-1-1 的 24 个测试全部 ERROR 后逼出来的
- DependencyResolutionGate 不是架构设计文档的一部分，是 s-1-2 分解时发现"没数据怎么测"逼出来的
- Convention-as-fix-queue 不是方法论设计，是 s-2-1 分解时扫到 inconsistency 列表后自然发生的

这与 PHILOSOPHY.md 的"长出来"叙事完全一致——但 PHILOSOPHY 讲的是理念，这里给出了具体的、可追溯的证据链。

### 2. 螺旋不是线性的

注意这条链路不是单纯的"经验积累"。它有**反馈回路**：

- s-1-2 review 的输出（inconsistency 列表）成为 s-2-1 分解的**输入**
- s-2-1 执行的结果反过来**更新** convention snapshot
- 更新后的 snapshot 将作为 s-2-2 分解的输入

每个模块既**消费**前序模块积累的知识，又**产生**新的知识给后续模块。这就是 MIMIR 所说的"知识的自我生长"的一个微缩实例。

### 3. L4 的诚实边界

这条链路中，**人类触发了每一次反思**：

- 是人类在 s-1-1 执行失败后决定"应该总结原则"
- 是人类在 s-1-2 review 后决定"应该记入 conventions"
- 是人类在 s-2-1 分解时注意到"这个 inconsistency 可以顺手修"

AI 做了提取、结构化、执行。但"该反思了"这个判断，每次都来自人类。这再次印证了 PHILOSOPHY.md 中的诚实注脚：L4 的"AI 自我进化"仍然需要人类点燃火花。

---

## 对 MIMIR 的意义

这条演进链路是 MIMIR **自我引用证明（self-referential proof）** 的第二层：

- **第一层**（已在 PHILOSOPHY.md 中）：MIMIR 本身是人机协作的产物。
- **第二层**（本复盘记录）：MIMIR 的每条原则都有一条从泥坑到闭环的可追溯路径。方法论不是从天而降的，而是从真实的失败和修复中螺旋生长出来的。

---

## 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.0 | 2025-02-05 | 初始复盘。记录 s-1-1 → s-1-2 → s-2-1 的完整方法论螺旋演进链路 |
| v1.1 | 2025-02-05 | 追加 s-2-2 节点：#11/#12 二次验证硬化、#14 ErrorCodeFidelity 诞生、#12 参数约束增强。螺旋从 v2.6 延伸到 v2.7 |
