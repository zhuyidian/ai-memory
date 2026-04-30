# 项目记忆首页模板

## 用途
- 这份模板用于生成每个项目自己的 `memory/INDEX.md`。
- 目标是给项目内稳定知识一个统一入口。
- 它只记录项目专属、跨任务稳定成立的知识，不记录一次性结论。

## 适用场景
- 新项目初始化
- 项目开始出现稳定专属知识时
- `CC_NEXT` 生成项目骨架时可选创建

## 模板正文

```md
# 项目记忆

## 这是什么
- 这是当前项目的专属记忆层。
- 这里只放对当前项目稳定成立的知识。
- 一次性任务结论留在 task 或 handoff，不直接写到这里。

## 适用范围
- 当前项目内多个任务会反复用到的知识
- 当前项目独有的架构边界、验证入口、宿主约束、回归热点

## 不适用范围
- 你的个人偏好
- 跨项目都成立的方法论
- 还没验证稳定的临时 workaround
- 长篇流水账式复盘

## 推荐结构
- `architecture/`: 项目独有架构边界
- `verification/`: 项目独有验证链路
- `regressions/`: 当前项目高风险回归点
- `decisions/`: 已稳定的项目级决策
- `handoffs/`: Claude 与 Codex 的共享交接文件

## 共享 handoff 规则
- 最新交付结果默认写到：`handoffs/codex-last-handoff.md`
- 如需要长期保留，可额外归档到：`handoffs/archive/`
- Claude 做 `CC_RETRO` 时，默认先读取 `handoffs/codex-last-handoff.md`

## 升级规则
- 只在当前项目成立：留在项目 memory
- 跨项目稳定成立：升级到 `E:/project/ai-memory`
- 必须长期强制遵守：升级到项目 `AGENTS.md`

## 什么时候更新
- 一个任务完成后出现稳定结论
- 一个阶段完成后识别出项目专属规律
- retrospective 明确指出某条知识会反复用到
```

## 使用说明
- 项目 `memory/INDEX.md` 只做入口页，不要堆太多细节
- 细节应进入 `architecture/`、`verification/`、`regressions/`、`decisions/`、`handoffs/` 等子文件
- 如果项目还很小，也可以先只有这一页

## 不要这样做
- 不要把 task 全文复制到项目 memory
- 不要把全局方法论挪进项目 memory
- 不要把客户敏感信息长期写进项目 memory

## 最后复核
- 2026-04-27
