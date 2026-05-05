# 项目 handoffs 目录模板

## 用途
- 这份模板用于生成项目内 `memory/handoffs/README.md`。
- 目标是把 Claude 和 Codex 的共享交接文件规则固定下来。
- 它不负责存放所有任务历史，只负责说明 handoff 文件怎么用。

## 适用场景
- 新项目初始化
- 项目采用 Claude / Codex 双窗口协作时
- 需要避免手动复制粘贴交付结果时

## 模板正文

```md
# Handoffs

## 这是什么
- 这是当前项目中 Claude 和 Codex 的共享交接目录。
- 主要用于让 Codex 写入最近一次交付结果，让 Claude 在复盘时自动读取。
- 同时保留每张已完成 task 的 handoff 归档，避免最近一次快照覆盖历史。

## 默认文件
- `codex-last-handoff.md`
  - 表示最近一次 `CX_HANDOFF` 的结果
  - 默认由 Codex 覆盖写入
  - 默认由 Claude 在 `CC_RETRO` 时优先读取

## 强制归档
- `archive/`
  - 用于保留每张已完成 task 的 handoff
  - 按日期、任务名或阶段名归档
  - 每次 `CX_HANDOFF` 都应写入一份

## 使用规则
- `CX_HANDOFF`：输出到聊天窗口，覆盖写入 `codex-last-handoff.md`，并写入 `archive/<日期或任务名>.md`
- `CC_RETRO`：默认先读取 `codex-last-handoff.md`
- 如果 handoff 文件不存在，Claude 应先说明缺少输入

## 不要这样做
- 不要把整个项目所有文档都塞进这个目录
- 不要把 handoff 文件当作长期架构文档
- 不要把客户敏感信息写成不必要的长记录
```

## 使用说明
- 新项目如果采用双窗口协作，建议在 `memory/handoffs/` 下放这份 README
- 如果项目很小，也应保留 `archive/` 目录规则；可以只在 task 完成时写入归档

## 最后复核
- 2026-04-30
