# 项目初始化标准流程

## 目的
- 这份文档定义“新接一个项目时，Claude 和 Codex 如何从 0 到 1 建立可执行骨架”。
- 目标不是直接写代码，而是先把项目规则、任务体系和执行流程搭起来。
- 这份流程适用于你的 `ai-memory` 工作方式，尤其适用于“Claude 先收敛，Codex 后执行”的模式。

## 触发条件
当一个项目目录里还没有这些关键文件时，视为”未初始化项目”：
- `CLAUDE.md`（参考 `templates/project-claude-template.md`）
- `AGENTS.md`（参考 `templates/project-agents-template.md`）
- `tasks/`（至少应承载 `progress.md` 与当前 task；参考 `templates/project-progress-template.md` 与 `templates/task-template.md`）
- `workflows/`（参考：
  - `templates/workflow-task-breakdown-template.md`
  - `templates/workflow-implementation-template.md`
  - `templates/workflow-review-checklist-template.md`
  - `templates/workflow-parallel-template.md`
  ）

此时应先走项目初始化流程，而不是直接进入实现。

## 核心原则
- 先收敛范围，再生成骨架，再进入执行。
- 项目内文件一旦生成，就成为执行阶段的事实来源。
- 全局 `ai-memory` 负责方法论，项目内文件负责当前项目落地。

## 标准顺序
1. Claude: `CC_SCOPE`
2. Claude: `CC_NEXT`
3. Codex: `CX_BREAKDOWN`
4. Codex: `CX_BUILD`
5. Codex: `CX_VERIFY`
6. Codex: `CX_HANDOFF`
7. Claude: `CC_RETRO`

## 阶段说明

### 阶段 1：需求收敛
执行者：Claude
触发词：`CC_SCOPE`

目标：
- 识别项目类型
- 收敛 MVP 范围
- 明确非目标
- 识别主要风险
- 规划任务拆分方向

输出：
- MVP
- 非目标
- 风险清单
- 初步任务拆分建议

规则：
- 不写当前 task 实现代码
- 如果项目还没骨架，只做收敛与规划

### 阶段 2：生成项目骨架
执行者：Claude
触发词：`CC_NEXT`

目标：
- 把收敛结果落成项目内可执行结构

最小输出：
- `CLAUDE.md`（参考 `templates/project-claude-template.md`）
- `AGENTS.md`（参考 `templates/project-agents-template.md`）
- `tasks/`（至少包含：
  - `tasks/progress.md`，用于声明当前只能执行哪一张 task，参考 `templates/project-progress-template.md`
  - 当前第一张 task，参考 `templates/task-template.md`
  ）
- `workflows/`（参考：
  - `templates/workflow-task-breakdown-template.md`
  - `templates/workflow-implementation-template.md`
  - `templates/workflow-review-checklist-template.md`
  - `templates/workflow-parallel-template.md`
  ）

建议输出：
- `tasks/_template.md`（参考 `templates/task-template.md`，供后续任务复用）
- 当前第一张 task 的文件名与目标应在 `tasks/progress.md` 中同步声明
- 项目内 `memory/` 骨架（参考 `templates/project-memory-index-template.md`，如果项目复杂度需要）
- `memory/handoffs/README.md`（参考 `templates/project-handoffs-readme-template.md`，如果项目采用双窗口协作）

规则：
- `AGENTS.md`（参考 `templates/project-agents-template.md`） 必须包含 `CX_*` 执行协议
- `CLAUDE.md`（参考 `templates/project-claude-template.md`） 应导入 `@AGENTS.md`
- 每张 task 必须遵循 `templates/task-template.md` 的稳定章节，至少写清：任务目标、背景问题、指定 module、允许修改范围、禁止修改范围、当前实现约束、非目标、建议修改范围、验收标准、输出要求
- `tasks/progress.md` 必须明确“当前任务只能有一个”，避免执行阶段出现多张并行当前任务
- 当前项目一旦生成这些文件，后续执行以项目内文件为准

### 阶段 3：当前任务拆解
执行者：Codex
触发词：`CX_BREAKDOWN`

目标：
- 读取项目 `AGENTS.md`、当前 task、workflow
- 输出任务理解、上下文分析、作用域确认、分阶段计划、风险预警

规则：
- 不直接写代码
- 只处理当前 task

### 阶段 4：实现与验证
执行者：Codex
触发词：`CX_BUILD` / `CX_VERIFY` / `CX_HANDOFF`

目标：
- 按当前 task 实现
- 做最小必要验证
- 输出交付说明

规则：
- 先骨架后主链路后边界
- 不扩 scope
- 交付时明确已验证项、未验证项和剩余风险

### 阶段 5：复盘与升级
执行者：Claude
触发词：`CC_RETRO`

目标：
- 对刚完成的 task 或阶段做复盘
- 判断哪些经验留在项目内
- 判断哪些经验升级到全局 `ai-memory`

规则：
- 一次性结论不进全局记忆
- 项目专属知识先进项目内 memory
- 跨项目稳定知识再升级到全局

## 新项目最小结构
建议生成：
```text
project/
├─ CLAUDE.md
├─ AGENTS.md
├─ tasks/
│  ├─ progress.md
│  ├─ _template.md
│  └─ <当前任务>.md
├─ workflows/
│  ├─ task_breakdown.md
│  ├─ implementation.md
│  ├─ review_checklist.md
│  └─ parallel_workflow.md
└─ memory/
   └─ INDEX.md
```

## 文件职责
- `CLAUDE.md`（参考 `templates/project-claude-template.md`）：Claude 的项目入口，导入共享规则和必要记忆
- `AGENTS.md`（参考 `templates/project-agents-template.md`）：Codex 的执行规则源，同时承载 `CX_*` 协议
- `tasks/`：当前项目的任务事实来源，其中：
  - `tasks/progress.md`（参考 `templates/project-progress-template.md`）负责声明当前任务、已完成任务和执行入口
  - `tasks/_template.md`（参考 `templates/task-template.md`）负责给后续任务复用统一结构
  - 每张 task（参考 `templates/task-template.md`）必须遵循稳定章节，至少覆盖任务目标、背景问题、指定 module、允许修改范围、禁止修改范围、当前实现约束、非目标、建议修改范围、验收标准、输出要求
- `workflows/`（参考：
  - `templates/workflow-task-breakdown-template.md`
  - `templates/workflow-implementation-template.md`
  - `templates/workflow-review-checklist-template.md`
  - `templates/workflow-parallel-template.md`
  ）：任务执行方法
- `memory/`：项目专属稳定知识

## 不要这样做
- 不要在项目未初始化时就让 Codex 开始实现
- 不要让 Claude 在已有当前 task 后继续重写 scope
- 不要把全局 `ai-memory` 直接当成项目执行文件
- 不要把项目专属知识直接升级到全局记忆

## 最后复核
- 2026-04-27





