# AI 记忆库加载策略

## 目的
- 明确全局 `ai-memory` 应该如何被 Claude 和 Codex 使用。
- 目标不是“尽量多加载”，而是“只加载当前决策真正需要的上下文”。
- 这份文件用于防止全局 memory 越积越多后，反而污染判断和执行。

## 核心原则
- 默认加载的是稳定方法论，不是全部知识。
- 按需加载的是领域知识，不是整个目录。
- 当前项目的显式规则、task 和 workflow，优先级高于全局 memory。
- Codex 以显式规则和当前任务为主，Claude 更适合承接较多的软记忆。

## Claude 默认读取集
Claude 默认只建议读取：
- `INDEX.md`
- `personal/decision-rules.md`
- `company/delivery-system.md`

原因：
- 这三份跨项目最稳定
- 能影响判断质量，但不会引入太多执行噪音
- 适合作为长期默认上下文

## Claude 按需读取集
### 1. 商业评估 / 估时 / 报价
按需读取：
- `company/pricing-principles.md`
- `templates/estimation-template.md`

### 2. 新项目启动 / 生命周期管理
按需读取：
- `company/project-lifecycle.md`
- `company/quality-bar.md`

### 3. 写 task / 拆 task
按需读取：
- `templates/task-template.md`
- 对应领域下的 `task-splitting-guide.md`

### 4. 做评审 / 交付 / 复盘
按需读取：
- `templates/review-template.md`
- `templates/handoff-template.md`
- `templates/retrospective-template.md`

### 5. Android 输入法项目
按需读取：
- `domains/android-ime/architecture-patterns.md`
- `domains/android-ime/regression-hotspots.md`
- `domains/android-ime/task-splitting-guide.md`
- `domains/android-ime/verification-paths.md`
- `domains/android-ime/ui-alignment-rules.md`

## Codex 默认读取集
Codex 默认不应全量读取 `ai-memory`。

Codex 的默认输入应是：
- 当前项目 `AGENTS.md`
- 当前 task 文件
- 当前 workflow 文件
- 当前项目内 `memory/`（如果 task 明确要求或项目约定有用）

原因：
- Codex 更偏执行
- 过多全局上下文容易降低任务聚焦度
- 当前 task 和项目规则才是实现阶段的唯一事实来源

## Codex 按需读取集
只有在当前 task 明确相关时，再按需读取某个具体全局 memory 文件。

适合按需读取的场景：
- 需要复用通用 task 模板
- 需要复用评审 / 交付 / 估时模板
- 当前项目属于已沉淀领域，如 Android IME
- 需要借助公司层质量线或生命周期规则做阶段判断

不适合按需读取的场景：
- 当前任务已经很明确，只差执行
- 当前只是局部 bugfix
- 当前 task 已经给了完整范围、风险和验收标准

## 不建议默认加载
以下内容不建议默认加载给任何 agent：
- `retros/` 下的候选经验池
- 还未升级验证的临时规律
- 过长的案例复盘
- 与当前领域无关的 domain 文件
- 所有模板文件全量加载

## 优先级顺序
发生冲突时，优先级按以下顺序判断：
- 当前 task
- 当前项目 `AGENTS.md`
- 当前项目 workflow
- 当前项目 `memory/`
- 全局 `ai-memory`
- agent 自带 memory

## 稳定判断规则
- 默认少读，不默认多读
- 优先读入口文件，不优先读整目录
- 优先读规则和方法论，不优先读案例池
- 当 task 已明确时，减少全局 memory 介入
- 当需求还模糊时，允许 Claude 多读一点全局方法论

## 推荐的 Claude 导入方式
推荐项目级 `CLAUDE.md` 默认导入：
- `@AGENTS.md`
- `@E:/project/ai-memory/INDEX.md`
- `@E:/project/ai-memory/personal/decision-rules.md`
- `@E:/project/ai-memory/company/delivery-system.md`

如果项目属于 Android 输入法，再按需补：
- `@E:/project/ai-memory/domains/android-ime/architecture-patterns.md`
- `@E:/project/ai-memory/domains/android-ime/regression-hotspots.md`

## 维护规则
- 只要默认读取集开始变长，就应重新压缩。
- 如果某条知识很少被真正使用，应降级出默认读取集。
- 默认读取集应长期保持精简、稳定、跨项目通用。

## 最后复核
- 2026-04-27
