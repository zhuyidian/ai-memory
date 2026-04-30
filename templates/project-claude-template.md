# 项目 CLAUDE 模板

## 用途
- 这份模板用于生成每个项目自己的 `CLAUDE.md`。
- 目标是给 Claude 一个项目级入口，让它在项目中优先按共享规则、全局方法论和项目上下文工作。
- 它不替代 `AGENTS.md`，而是优先导入并复用 `AGENTS.md`。

## 适用场景
- 新项目初始化
- 老项目补齐 Claude 项目入口
- `CC_NEXT` 在生成项目骨架时作为参考模板使用

## 模板正文

```md
@AGENTS.md
@E:/project/ai-memory/INDEX.md
@E:/project/ai-memory/personal/decision-rules.md
@E:/project/ai-memory/company/delivery-system.md

## 项目级 Claude 职责
- 需求收敛
- task 设计
- 当前任务的只读分析
- retrospective
- memory 升级判断

## 项目级工作规则
- 如果当前任务已存在，不重定义 scope
- 当前任务的代码实现、验证和交付优先交给 Codex
- 项目内 `AGENTS.md`、`tasks/`、`workflows/` 是执行阶段的事实来源
- 只有当当前问题属于具体领域时，才按需读取对应领域文件

## 共享文件接力规则
- Claude 做 `CC_RETRO` 时，默认先读取：`memory/handoffs/codex-last-handoff.md`
- 如果 handoff 文件不存在，应先说明缺少输入，再决定是否做有限复盘

## 快捷词协议
当我输入以下完整快捷词时，严格按其含义执行：

- `CC_SCOPE`
  - 收敛新需求或新任务
  - 输出 MVP、非目标、风险、任务拆分建议
  - 不写当前任务代码

- `CC_NEXT`
  - 生成下一张任务
  - 如果项目骨架未完整生成，继续补齐项目 `CLAUDE.md`、`AGENTS.md`、`tasks/`、`workflows/`

- `CC_AUDIT`
  - 对当前任务做只读分析
  - 检查范围、非目标、验收标准、依赖和风险
  - 不改 scope

- `CC_RETRO`
  - 默认先读取 `memory/handoffs/codex-last-handoff.md`
  - 基于最近一次 Codex 交付结果做 retrospective
  - 判断哪些经验留在项目内 memory
  - 判断哪些升级到 `E:/project/ai-memory`

只有当用户输入完全等于快捷词时，才触发快捷词协议；否则按普通自然语言处理。
```

## 使用说明
- 新项目生成后，项目 `CLAUDE.md` 应优先导入 `@AGENTS.md`
- 如果项目属于具体领域，可按需补充导入对应领域文件
- 不要把大量项目专属实现细节硬写进项目 `CLAUDE.md`

## 不要这样做
- 不要让项目 `CLAUDE.md` 取代 `AGENTS.md`
- 不要把全局 `ai-memory` 全量硬塞进项目 `CLAUDE.md`
- 不要把当前任务的实现说明写进项目 `CLAUDE.md`

## 最后复核
- 2026-04-27
