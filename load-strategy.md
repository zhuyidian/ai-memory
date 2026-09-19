# AI 记忆库加载策略

## 记忆状态
- 状态：active
- 替代文件：无（已合并 `usage-guide.md`）
- 上次复核：2026-05-06
- 下次复核触发条件：默认读取集变长、核心 memory 降级、Claude/Codex 默认输入策略变化或写回规则变化时复核

## 目的
- 明确全局 `ai-memory` 应该如何被 Claude 和 Codex 使用。
- 目标不是"尽量多加载"，而是"只加载当前决策真正需要的上下文"。
- 这份文件用于防止全局 memory 越积越多后，反而污染判断和执行。

## 核心原则
- 默认加载的是稳定方法论，不是全部知识。
- 按需加载的是领域知识，不是整个目录。
- 当前项目的显式规则、task 和 workflow，优先级高于全局 memory。
- Codex 以显式规则和当前任务为主，Claude 更适合承接较多的软记忆。

## Claude 默认读取集
Claude 默认只建议读取：
- `INDEX.md`
- `load-strategy.md`
- `personal/decision-rules.md`
- `company/delivery-system.md`

原因：
- 这三份跨项目最稳定
- 能影响判断质量，但不会引入太多执行噪音
- 适合作为长期默认上下文

## Claude 按需读取集

通用快捷词规则：当用户输入任何 `CC_*` / `CX_*` 快捷词时，强制前置读取 `company/agent-shortcut-protocol.md`，不得跳过。以下各 section 不再重复声明此规则。

### 1. 商业评估 / 估时 / 报价
快捷词触发：当用户输入 `CC_PRICE` 时，强制前置读取，不得跳过：
- `company/pricing-principles.md`

按需读取：
- `templates/estimation-template.md`

### 2. 新项目启动 / 生命周期管理
触发条件：当项目目录缺少 `CLAUDE.md`、`AGENTS.md`、`tasks/`、`workflows/` 中任意一个时，或当用户使用 `CC_SCOPE` / `CC_NEXT` 且项目未初始化时，以下文件为强制前置读取，不得跳过：
- `company/project-lifecycle.md`
- `company/project-bootstrap-standard.md`

### 3. 写 task / 拆 task
按需读取：
- `templates/task-template.md`
- 对应领域下的 `task-splitting-guide.md`

### 4. 做评审 / 交付 / 复盘
触发条件：当用户使用 `CC_RETRO` 或需要做记忆升级判断时，以下文件为强制前置读取，不得跳过：
- `knowledge-intake-checklist.md`

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

### 6. AI API 中转站项目候选方向
仅在准备启动类似 AICodeMirror / OpenRouter 的 AI API 中转站项目时按需读取：
- `domains/ai-api-gateway/architecture-patterns.md`
- `domains/ai-api-gateway/regression-hotspots.md`
- `domains/ai-api-gateway/task-splitting-guide.md`
- `domains/ai-api-gateway/verification-paths.md`

说明：
- 当前领域状态为 `candidate`，不进入任何默认读取集。
- 只用于启动阶段收敛 MVP、拆 task、列验证路径和提前识别风险。
- 等完成首个 MVP 或经过真实项目验证后，再决定是否升级为 `active`。

### 7. 质量判断
快捷词触发：当用户输入 `CC_QUALITY` 时，强制前置读取，不得跳过：
- `company/quality-bar.md`

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

## 写回规则

### task 执行过程中
默认不立即写入全局 memory。

原因：
- 很多结论还没稳定
- 容易把临时 workaround 提升成长期规则

建议写回位置：
- 当前 task
- 项目 handoff
- 项目内 `memory/`

### task 完成后
如果发现一条规律：
- 只对当前项目成立：写到项目 `memory/`
- 跨同类项目成立：写到 `domains/`
- 跨项目都成立：写到 `company/`
- 明显体现你个人稳定判断：写到 `personal/`
- 还没坐实但值得观察：写到 `retros/stable-patterns.md`

### 项目交付后
建议至少检查一次：
- 有没有值得升级的稳定规律
- 有没有该淘汰的旧规则
- 有没有某个模板需要补强

## 快速路径
最小使用方式：

### 开始项目时
读：
- `INDEX.md`
- `personal/decision-rules.md`
- `company/delivery-system.md`

### 写 task 时
读：
- `templates/task-template.md`
- 对应领域的 `task-splitting-guide.md`

### 做完 task 时
读：
- `templates/handoff-template.md`
- `templates/retrospective-template.md`
- `knowledge-intake-checklist.md`

## 不建议默认加载
以下内容不建议默认加载给任何 agent：
- `retros/` 下的候选经验池
- 还未升级验证的临时规律
- 过长的案例复盘
- 与当前领域无关的 domain 文件
- 所有模板文件全量加载

## 不要这样做
- 不把客户专属信息写进全局 memory
- 不把一次性问题直接升级为全局规则
- 不把项目内 task 文档复制到全局 memory
- 不默认让 Claude 或 Codex 全量读取整个 `ai-memory`
- 不因为目录好看就持续拆新文件

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
项目级 `CLAUDE.md` 只需导入全局 CLAUDE.md 未覆盖的项目专属文件：
- `@AGENTS.md`

以下文件已由全局 `~/.claude/CLAUDE.md` 默认导入，项目级不要重复：
- `INDEX.md`
- `load-strategy.md`
- `personal/decision-rules.md`
- `company/delivery-system.md`

领域文件不走 CLAUDE.md `@` 导入，走本文件按需读取集（section 5/6），按场景触发读取。

## 维护规则
- 默认追加到现有文件，不轻易新建文件
- 同主题内容明显变多、边界稳定时，再拆新文件
- 写进 `ai-memory` 的内容必须能复用、能解释、能限定边界
- 没验证够的知识，先放 `retros/`
- 一次性内容不要写进全局 memory
- 只要默认读取集开始变长，就应重新压缩
- 如果某条知识很少被真正使用，应降级出默认读取集
- 默认读取集应长期保持精简、稳定、跨项目通用
- 核心 memory 文件应带有轻量状态头：状态、替代文件、上次复核、下次复核触发条件
- `active` 可进入默认或按需读取集；`candidate` 默认只进入 `retros/` 或按需观察；`deprecated` 不进入默认读取集
- 当某条规则被新规则替代时，应标记 `deprecated` 并写明替代文件或替代规则

### 每周维护
每周或每完成 2 到 3 个 task，做一次轻量维护：
- 读 `retros/stable-patterns.md`
- 挑出值得升级的规律
- 删除已经失效或重复的内容
- 检查默认读取集是否变长、是否需要压缩

### 每月维护
每月做一次结构性检查：
- `personal/` 是否仍体现你真实判断
- `company/` 是否仍符合你当前交付模式
- `domains/` 是否出现新领域值得单独建目录
- `templates/` 是否有哪个模板已经不用了
- `load-strategy.md` 是否还合理

## 最后复核
- 2026-05-06
