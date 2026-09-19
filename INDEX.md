# AI 记忆库

## 记忆状态
- 状态：active
- 替代文件：无
- 上次复核：2026-04-30
- 下次复核触发条件：目录结构、默认读取集、升级路径或入口文件变化时复核

## 这是什么
- 这是我的全局主记忆仓库。
- 目标是沉淀跨项目稳定成立的个人判断、公司方法、领域经验和可复用模板。
- 它不是任务系统，不替代项目内 `AGENTS.md`、`CLAUDE.md`、`tasks/`、`workflows/` 和项目内 `memory/`。

## 读取策略
- 加载策略、写回规则、快速路径和维护建议见 `load-strategy.md`，本文件不重复定义。
- 按需读取的触发条件和强制前置规则以 `load-strategy.md` 为准，本文件不再单独列出。

## 记忆地图
- `personal/`：我的长期判断方式与取舍标准
  - `personal/decision-rules.md`：个人决策规则（默认加载）
- `company/`：我的 AI 公司交付系统、质量线、生命周期、报价原则、协作协议与项目初始化
  - `company/delivery-system.md`：交付系统（默认加载）
- `domains/`：某类项目的稳定打法、架构套路、回归热点、验证路径
- `templates/`：任务、评审、交付、复盘、估时模板
- `retros/`：还没坐实、但值得继续观察的规律池

## 领域入口
- Android 输入法：
  - `domains/android-ime/architecture-patterns.md`
  - `domains/android-ime/regression-hotspots.md`
  - `domains/android-ime/task-splitting-guide.md`
  - `domains/android-ime/verification-paths.md`
  - `domains/android-ime/ui-alignment-rules.md`
- AI API 中转站（candidate，启动阶段按需读取）：
  - `domains/ai-api-gateway/architecture-patterns.md`
  - `domains/ai-api-gateway/regression-hotspots.md`
  - `domains/ai-api-gateway/task-splitting-guide.md`
  - `domains/ai-api-gateway/verification-paths.md`

## 项目启动入口
- 见 `load-strategy.md` section 2

## 复盘入口
- `retros/stable-patterns.md`：候选稳定规律池

## 模板入口
- `templates/task-template.md`
- `templates/review-template.md`
- `templates/handoff-template.md`
- `templates/retrospective-template.md`
- `templates/estimation-template.md`
- `templates/project-agents-template.md`
- `templates/project-claude-template.md`
- `templates/project-memory-index-template.md`
- `templates/project-progress-template.md`
- `templates/project-handoffs-readme-template.md`
- `templates/workflow-task-breakdown-template.md`
- `templates/workflow-implementation-template.md`
- `templates/workflow-review-checklist-template.md`
- `templates/workflow-parallel-template.md`

## 知识收录与升级
- 知识收录判断、升级路径和不该放什么，见 `knowledge-intake-checklist.md`，本文件不重复定义。

## 最后复核
- 2026-05-06
