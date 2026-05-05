# Agent 快捷词协议

## 记忆状态
- 状态：active
- 替代文件：无
- 上次复核：2026-04-30
- 下次复核触发条件：当快捷词集合、交接文件路径或 Claude/Codex 分工变化时复核

## 目的
- 这份文件定义 Claude 和 Codex 的固定快捷词协议。
- 目标是用最少的固定词，驱动稳定的双窗口协作流程。
- 快捷词是全局元协议，不依赖某个具体项目先存在。

## 生命周期来源
- 项目阶段顺序以 `company/project-lifecycle.md` 为唯一主文档。
- 本文件只定义快捷词含义、触发条件和 Claude/Codex 交接规则。
- 新项目骨架生成细节以 `company/project-bootstrap-standard.md` 为准。

## 触发规则
- 只有当用户输入完全等于某个快捷词时，才触发对应协议。
- 否则按普通自然语言处理。
- 不要把相似文本误判成快捷词。

## 共享文件接力规则
- 不依赖跨窗口会话内存传递信息。
- Codex 与 Claude 的交接，默认通过项目内共享文件完成。
- 最新 handoff 快照路径为：`memory/handoffs/codex-last-handoff.md`
- 每张完成的 task 必须额外归档一份 handoff：`memory/handoffs/archive/<日期或任务名>.md`

## Claude 快捷词

### `CC_SCOPE`
含义：
- 收敛新需求或新任务
- 输出 MVP、非目标、风险、任务拆分建议
- 不写当前任务代码

特殊规则：
- 如果当前项目还没有完整骨架，不要求项目先有 `CLAUDE.md`
- 此时应使用全局 `~/.claude/CLAUDE.md` 与 `E:/project/ai-memory`
- 如果发现项目尚未建立执行骨架，应进入“骨架规划”模式：
  - 识别项目类型
  - 规划需要的 `AGENTS.md`
  - 规划 `tasks/`
  - 规划 `workflows/`
- `CC_SCOPE` 主要负责“想清楚”，不负责最终把所有文件落盘

### `CC_NEXT`
含义：
- 生成下一张任务
- 在新项目阶段，把收敛结果落成项目内执行骨架

新项目阶段额外职责：
- 生成项目 `CLAUDE.md`
- 生成项目 `AGENTS.md`
- 生成 `tasks/`
- 生成 `workflows/`
- 把 `CX_*` 协议写入项目 `AGENTS.md`

老项目阶段职责：
- 按现有风格生成下一张任务
- 不改已明确的当前任务范围

### `CC_AUDIT`
含义：
- 对当前任务做只读分析
- 检查范围、非目标、验收标准、依赖和风险
- 不写代码
- 不重定义范围

### `CC_RETRO`
含义：
- 默认先读取 `memory/handoffs/codex-last-handoff.md`
- 基于最近一次 Codex 交付结果做复盘
- 判断哪些经验留在项目内
- 判断哪些经验升级到 `E:/project/ai-memory`
- 判断哪些经验还不够稳定，只能进入 `retros/`

规则：
- 如果 handoff 文件不存在，应先明确说明缺少输入，而不是凭空做完整复盘
- 如 handoff 文件存在，优先以 handoff 文件为事实来源，再结合 task、workflow 和项目 memory 做判断

## Codex 快捷词

### `CX_BREAKDOWN`
含义：
- 读取项目 `AGENTS.md`、当前任务、当前工作流
- 只做任务拆解
- 不直接写代码

前提：
- 只有在项目 `AGENTS.md` 已存在时才可用
- 若项目骨架不存在，不使用此快捷词

### `CX_BUILD`
含义：
- 执行当前任务
- 顺序固定为：先骨架、后主链路、后边界
- 只改允许范围内文件
- 不扩范围

### `CX_VERIFY`
含义：
- 只做当前任务的最小必要验证
- 输出已验证项、未验证项、未验证原因

### `CX_HANDOFF`
含义：
- 输出当前任务的交付总结
- 包括修改范围、验证结果、已知限制、剩余风险、建议下一步
- 同时覆盖写入最新快照：`memory/handoffs/codex-last-handoff.md`
- 同时归档写入：`memory/handoffs/archive/<日期或任务名>.md`

规则：
- handoff 文件应始终表示“最近一次交付结果”
- 如果项目内还没有 `memory/handoffs/` 或 `memory/handoffs/archive/`，应先创建后再写入
- handoff 文件是 Claude 做 `CC_RETRO` 的默认输入

## 使用顺序来源
- 新项目协作顺序见 `company/project-bootstrap-standard.md`。
- 老项目新任务应遵循 `company/project-lifecycle.md` 的阶段顺序，并按需使用本文件的快捷词。

## 角色分工
- Claude 负责：收敛需求、任务设计、只读审查、复盘、记忆升级
- Codex 负责：当前任务实现、验证、交付

## 最后复核
- 2026-04-30
