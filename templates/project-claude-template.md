# 项目 CLAUDE 模板

## 记忆状态
- 状态：active
- 替代文件：无
- 上次复核：2026-04-27
- 下次复核触发条件：当项目 CLAUDE.md 的职责边界、导入方式或与全局 CLAUDE.md 的分工变化时复核

## 用途
- 这份模板用于生成每个项目自己的 `CLAUDE.md`。
- 目标是给 Claude 一个项目级入口，只补充项目特有规则，不重复全局 `~/.claude/CLAUDE.md` 已提供的内容。
- 它不替代 `AGENTS.md`，而是优先导入并复用 `AGENTS.md`。

## 适用场景
- 新项目初始化
- 老项目补齐 Claude 项目入口
- `CC_NEXT` 在生成项目骨架时作为参考模板使用

## 模板正文

```md
@AGENTS.md

## 项目特有规则
- 项目内 `AGENTS.md`、`tasks/`、`workflows/` 是执行阶段的事实来源
- 只有当当前问题属于具体领域时，才按需读取对应领域文件
- handoff 路径与接力规则见 `AGENTS.md` 的"共享文件接力规则"
```

## 使用说明
- 新项目生成后，项目 `CLAUDE.md` 应优先导入 `@AGENTS.md`
- 全局 `~/.claude/CLAUDE.md` 已提供默认读取集、快捷词列表和角色边界，项目 `CLAUDE.md` 不需要重复
- 如果项目属于具体领域，可按需在 `@AGENTS.md` 之后补充导入对应领域文件
- 不要把大量项目专属实现细节硬写进项目 `CLAUDE.md`

## 不要这样做
- 不要让项目 `CLAUDE.md` 取代 `AGENTS.md`
- 不要把全局 `ai-memory` 全量硬塞进项目 `CLAUDE.md`
- 不要把当前任务的实现说明写进项目 `CLAUDE.md`
- 不要在项目 `CLAUDE.md` 中重复全局 `~/.claude/CLAUDE.md` 已有的内容（快捷词、角色边界、默认读取集）

## 最后复核
- 2026-04-27
