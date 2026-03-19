**简体中文** | [English](README.md)

# MirrorForge

**将 AI 辅助编程会话转化为结构化的工程成长。**

MirrorForge 是一个面向 AI 协作编程工作流的反思型 Skill。  
大多数 AI 编码工具帮助你修复代码；MirrorForge 帮你看清：你的**原始代码、决策方式与工程习惯**到底暴露了哪些能力缺口，并把一次性的 AI 帮助转化为可复用的成长信号。

> **分析的是用户，而不是 Agent。**

---

## 为什么需要 MirrorForge

AI 编码助手很擅长帮你脱困。

但它们往往不会回答最重要的问题：

> **我的原始实现，究竟暴露了我怎样的工作方式？**

一次 bug 修复背后，常常藏着反复出现的模式，例如：

- 对边界和前置假设缺乏防御性编码意识
- 先实现、后验证的工作流
- 模块化浅、职责边界不清
- 在重构时对风险判断过于乐观
- 某些工程习惯在多个会话中悄悄重复出现

MirrorForge 的目标，就是把 AI 辅助编程从短期提效工具，变成长期成长回路。

---

## 它是什么

MirrorForge 是一个 **evidence-first（证据优先）** 的 AI 编程反思 Skill。

它要回答的问题包括：

- 我的原始代码暴露了哪些工程问题？
- 这次重构或调试暴露了哪些能力缺口？
- 哪些模式开始在多个会话中重复出现？
- 从这个案例出发，下一步应该怎么训练？
- 这个案例是否值得整理成可复用的 playbook 草稿？

---

## 它不是什么

MirrorForge **不是**：

- 普通的 code review 工具
- 泛泛而谈的“成长教练”提示词
- 用来夸赞 AI 最终产出有多漂亮的工具
- 不基于证据、只会给抽象标签的总结器

它**并不主要**评价最终代码“好不好”。

它的工作，是分析 AI 协助过程中暴露出来的**开发者信号**。

---

## 核心理念

MirrorForge 关注的是：

- 用户原始实现中的问题
- 用户做决策时体现出的模式
- AI 协作过程中暴露出的工程习惯

因此，它的重心不是：

- “AI 这次重构得很优雅”
- “最终代码看起来更整洁了”
- “这里有一些通用最佳实践”

它始终追问的是：

> **这个案例究竟暴露了开发者的什么问题？**

---

## 它如何工作

MirrorForge 会优先使用**最强的证据**。

### 证据层级

1. **基于 diff 的代码证据**  
   这是最强证据。最适合直接对比用户原始代码与 AI 改进后代码的场景。

2. **单次代码快照**  
   当没有前后 diff 时仍然有效，但强度弱于 before/after 证据。

3. **对话 / 决策证据**  
   适合分析过程、取舍与判断模式；但当存在有意义的 diff 时，它不能替代 code-backed findings。

### 分析模式

MirrorForge 会只选择一个主模式：

- **Mode A — 基于 diff 的代码反思**  
  当存在 meaningful diff 时使用，且必须产出 code-backed findings。

- **Mode B — 代码快照反思**  
  有代码，但拿不到 before/after diff 时使用。

- **Mode C — 纯对话反思**  
  仅在没有可用代码证据时使用。

### 为什么这套流程不一样

MirrorForge 刻意保持严格：

- 不允许把 diff 分析停留在 chunk 记账或模糊感想
- 不把一句短结论当成真正分析
- 要求从证据到 gap 映射的完整推理链
- 将“分析”与“沉淀”明确分开

一个有效 finding 应该走完这样的链路：

**原始证据 → 原来哪里错了 → 为什么这是工程问题 → 为什么映射到这个 gap → 置信度与边界 → 具体修正模式**

---

## 它和普通反思提示词的区别

大多数“反思型”提示词通常只会做两件事之一：

- 总结最终代码
- 输出类似“验证意识薄弱”这种抽象标签

MirrorForge 刻意抵抗这两种偷懒方式。

它的工作流围绕以下原则构建：

- **证据优先诊断**
- **显式推理链**
- **防止浅层总结的硬约束**
- **有限边界内的判断，而不是过度自信的上升**
- **产出天然可整理为 playbook，但不默认自动持久化**

所以它不像一个凭感觉给建议的教练，更像一个可追问、可审计的工程反思审计器。

---

## 快速示例

你让 AI 助手帮你修一个比较混乱的重构。

代码最后被修好了。

普通助手可能会停留在：

> “这次重构更干净了，验证也补上了。”

MirrorForge 会继续追问：

- 原始版本到底哪里有问题？
- 这是单纯的风格差异，还是实打实的工程风险？
- 这样的代码是由什么行为模式导致的？
- 这是一锤子问题，还是某种 recurring capability gap？

一个简化后的输出可能像这样：

### Code-backed finding

原始实现把领域假设和捷径式分支逻辑混在一起，导致验证缺失、控制流脆弱。

### Root gap hypothesis

**先实现、后验证的工作流**

### Why this gap fits

问题不只是“忘了做输入验证”。  
更深一层的模式是：在边界假设还没有稳定之前，就已经开始写分支逻辑。

### Risk

在相邻重构里，容易因为“这些假设看起来很 obvious”而高估正确性。

### Next minimal action

在写分支逻辑之前，强制过一遍简短的验证清单：

- 我现在对 shape、nullability、type 做了哪些假设？
- 哪些假设是调用方保证的，哪些只是我主观上觉得“应该成立”？
- 如果这些假设不成立，第一处会炸在哪里？

---

## Examples

MirrorForge 这种项目，最好同时看一个短例子和一个完整案例，理解会更快。

- [Quick example](docs/examples/quick-example.md) — 一页看懂核心工作流
- [Full analysis report](docs/examples/parsegsdfile-analysis-report.md) — 一个真实的 Mode A 案例，包含 inventory、diff ledger、findings 与 gap mapping
- [Generated playbook](docs/examples/parsegsdfile-playbook.md) — 上述完整案例生成出的 playbook


## Playbook 交接

MirrorForge 将**分析**与**持久化**视为两个不同承诺。

这意味着：

- 它可以自动完成一次分析
- 但它**不应默认**每次分析都要被保存
- 分析结束后，它会明确询问，是否要把这个案例整理成 playbook 草稿

在用户批准后，目标路径为：

```text
docs/mirrorforge-playbook.md
```

Playbook 的目的，不是永久存满所有完整报告。  
它的目标是保留一份**足够耐久、足够轻量**的上下文，让你记住：

- 观察到了哪个 gap
- 判断有多大把握
- 证据锚点是什么
- 下次该警惕哪些信号
- 应该养成什么具体防御习惯

### Playbook 条目示例

```md
## GAP 01: 边界防御薄弱

- 状态: Observed
- 置信度: Medium
- 最后更新: 2026-03-19
- 来源会话: [S1]

### 代表性证据
- Code/Artifact:
  - 分支逻辑在 shape validation 尚未完成前，就假定 `input.data` 存在且结构可用。
- Dialogue/Decision:
  - 实现时优先让主路径跑通，而没有先把边界假设显式化。

### 触发条件
- 涉及解析、外部输入、隐式结构假设的重构
- 把“这个字段应该存在”当成充分验证的场景

### 下一步最小动作
- 在任何控制流扩张前，先增加一轮 pre-branch validation。

### 防御清单
- [ ] 在使用字段前，是否验证了 shape、nullability 和 type？
- [ ] 我是不是依赖了一个只存在于脑海中的假设？
- [ ] 如果这个假设失败，第一处不安全使用在哪里？

### 下次最可能受伤的地方
- Parser 重构
- Data transformation helpers
- 边界条件密集的集成代码
```

---

## 安装方式

### Claude Code

将仓库克隆到本地 skills 目录：

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/hqy2435662352/mirror-forge
```

预期路径：

```text
~/.claude/skills/mirrorforge/SKILL.md
```

之后可以这样调用：

```text
/mirror-forge 这次调试过程暴露了我哪些工程能力问题？
```

### 其他 Agent 环境

MirrorForge 本质上是一份可复用的工作流规范，也可以适配到支持自定义 skills、system prompts 或 instruction modules 的其他 Agent 环境中。

---

## 什么时候适合使用

当你真正想问的不是：

> “这段代码哪里错了？”

而是：

> “这个案例暴露了我哪些工程能力缺口？”

就适合用 MirrorForge。

典型使用场景包括：

- AI 辅助重构
- 调试会话
- Bug 修复
- 编译错误处理
- 性能优化
- 跨会话复盘 recurring patterns
- 从多个案例中整理成长 playbook

---

## 仓库结构

```text
mirrorforge/
├─ README.md
├─ README.zh-CN.md
├─ SKILL.md
└─ examples/
   ├─ quick-example.md
   ├─ parsegsdfile-analysis-report.md
   └─ parsegsdfile-playbook.md
```

---

## 设计哲学

MirrorForge 建立在一个简单信念上：

> **AI 编码不应该只改善代码，也应该改善开发者。**

AI 协作会话里最有价值的东西，未必总是修复本身。

有时候，真正值钱的是修复所暴露出来的模式。

MirrorForge 想做的，就是让这些模式变得可见、可结构化、可复用。

---

## Roadmap

未来可能的方向包括：

- 更强的跨会话模式综合
- 面向不同语言的反思启发式规则
- 更好的 playbook 压缩与整理规则
- 针对不同工程领域的示例包
- 面向更多 Agent 宿主环境的集成模板

---

## 贡献

欢迎贡献，尤其包括：

- 更真实的反思案例
- 语言相关的证据模式
- 更好的 playbook schema
- 更完整的宿主环境集成说明
- 提升分析可靠性与 anti-drift 能力的改进

---

## License

MIT
