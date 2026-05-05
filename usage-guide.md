# AI 记忆库使用说明

## 记忆状态
- 状态：active
- 替代文件：无
- 上次复核：2026-04-30
- 下次复核触发条件：日常读取路径、写回规则、维护节奏或默认配套文件变化时复核

## 目的
- 这份文档说明 `E:\project\ai-memory` 在日常工作里怎么用。
- 目标不是把所有东西都记下来，而是在正确时机把正确的知识沉淀到正确位置。
- 这份指南默认服务于“个人 + AI 公司 + 多项目并行”的工作方式。

## 核心原则
- `ai-memory` 不是任务系统。
- `ai-memory` 不是项目文档替代品。
- `ai-memory` 是跨项目稳定知识的主脑。
- 当前项目的 task、workflow、AGENTS 和项目内 memory，仍然是执行阶段的第一事实来源。

## 日常使用模型
日常工作建议按下面顺序使用：
- 开始一个新项目或新需求时，先读全局方法论
- 进入具体领域时，再读对应领域文件
- 当前 task 明确后，以项目规则和 task 为主
- task 做完后，再回到 `ai-memory` 做知识升级

## 什么时候读什么

### 1. 新项目刚开始
优先读：
- `INDEX.md`
- `personal/decision-rules.md`
- `company/delivery-system.md`
- `company/project-lifecycle.md`
- `company/project-bootstrap-standard.md`

目的：
- 先统一判断方式和项目节奏
- 避免一上来就陷入实现细节

### 2. 需要估时、报价、判断值不值得做
优先读：
- `company/pricing-principles.md`
- `templates/estimation-template.md`

目的：
- 先收敛范围和风险
- 再给时间与成本判断

### 3. 需要把需求写成 task
优先读：
- `templates/task-template.md`
- 对应领域的 `task-splitting-guide.md`
- 必要时读 `company/quality-bar.md`

目的：
- 保证 task 是单一主目标、范围清楚、可验证的

### 4. 进入具体领域工作
按需读：
- 对应 `domains/` 下的相关文件

例如 Android 输入法项目：
- `domains/android-ime/architecture-patterns.md`
- `domains/android-ime/regression-hotspots.md`
- `domains/android-ime/verification-paths.md`
- `domains/android-ime/ui-alignment-rules.md`

目的：
- 减少重复踩坑
- 优先复用同类项目的稳定打法

### 5. 当前 task 已明确、开始实现
默认不要再大量读全局 memory。
此时应切到：
- 项目 `AGENTS.md`
- 当前 task
- 当前 workflow
- 当前项目内 `memory/`

目的：
- 保持执行聚焦
- 减少全局知识对当前实现的干扰

### 6. 做评审、交付、复盘
优先读：
- `templates/review-template.md`
- `templates/handoff-template.md`
- `templates/retrospective-template.md`
- 必要时读 `company/quality-bar.md`

目的：
- 把输出统一成高信号格式
- 方便后续升级成 memory

## 什么时候写回

### 1. task 执行过程中
默认不立即写入全局 memory。
原因：
- 很多结论还没稳定
- 容易把临时 workaround 提升成长期规则

建议写回位置：
- 当前 task
- 项目 handoff
- 项目内 `memory/`

### 2. task 完成后
如果发现一条规律：
- 只对当前项目成立：写到项目 `memory/`
- 跨同类项目成立：写到 `domains/`
- 跨项目都成立：写到 `company/`
- 明显体现你个人稳定判断：写到 `personal/`
- 还没坐实但值得观察：写到 `retros/stable-patterns.md`

### 3. 项目交付后
建议至少检查一次：
- 有没有值得升级的稳定规律
- 有没有该淘汰的旧规则
- 有没有某个模板需要补强

## 快速路径
如果你时间很少，最小使用方式是：

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

这样已经足够让整个系统转起来。

## 更新规则
- 默认追加到现有文件，不轻易新建文件
- 同主题内容明显变多、边界稳定时，再拆新文件
- 写进 `ai-memory` 的内容必须能复用、能解释、能限定边界
- 没验证够的知识，先放 `retros/`
- 一次性内容不要写进全局 memory

## 每周维护建议
每周或每完成 2 到 3 个 task，做一次轻量维护：
- 读 `retros/stable-patterns.md`
- 挑出值得升级的规律
- 删除已经失效或重复的内容
- 检查默认读取集是否变长、是否需要压缩

## 每月维护建议
每月做一次结构性检查：
- `personal/` 是否仍体现你真实判断
- `company/` 是否仍符合你当前交付模式
- `domains/` 是否出现新领域值得单独建目录
- `templates/` 是否有哪个模板已经不用了
- `load-strategy.md` 是否还合理

## 不要这样做
- 不把客户专属信息写进全局 memory
- 不把一次性问题直接升级为全局规则
- 不把项目内 task 文档复制到全局 memory
- 不默认让 Claude 或 Codex 全量读取整个 `ai-memory`
- 不因为目录好看就持续拆新文件

## 推荐配套文件
平时最常一起用的文件：
- `INDEX.md`
- `load-strategy.md`
- `knowledge-intake-checklist.md`
- `personal/decision-rules.md`
- `company/delivery-system.md`
- 对应领域文件

## 最后复核
- 2026-04-30

