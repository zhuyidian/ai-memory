# AI API 中转站回归热点

## 记忆状态
- 状态：candidate
- 替代文件：无
- 上次复核：2026-05-05
- 下次复核触发条件：首个 MVP 上线、出现真实事故、或接入多个 provider 后复核

## 适用范围
- 适用于 AI API 中转站项目的风险预判、评审和回归检查。
- 重点覆盖协议兼容、stream、provider fallback、计费、安全和运维观测。

## 核心立场
- 中转站的高风险不在“请求是否能转发”，而在异常路径、账单一致性和密钥安全。
- 每次改 provider、stream、路由或计费，都应默认触发回归检查。

## 高风险热点
### 1. Stream 中断和扣费不一致
风险：
- 用户中途断开但系统已扣全额。
- provider 中断但平台记录成功。
- stream 没有正常 `[DONE]`，客户端仍认为完成。

回归重点：
- 客户端取消
- provider timeout
- 网络中断
- 部分输出后的 usage 记录

### 2. Fallback 导致重复扣费
风险：
- 第一个 provider 失败已记录扣费，fallback 成功后再次扣费。
- 多次重试生成多个 usage ledger。
- 用户看到一次请求，后台形成多笔消费。

回归重点：
- 请求级 idempotency
- fallback 前后账单状态
- 原始失败日志

### 3. Provider 响应格式差异污染对外协议
风险：
- 不同 provider 的 response shape 直接透出。
- error code 不统一，客户端兼容失败。
- usage 字段缺失导致账单异常。

回归重点：
- response normalizer
- error mapper
- usage extractor

### 4. 模型能力误标
风险：
- 对外声称支持 tool calling，但 provider 不支持或格式不同。
- vision / reasoning / JSON mode 能力被错误开放。
- 客户端请求特殊能力后返回不可解释错误。

回归重点：
- 模型 registry
- capability flags
- 请求参数校验

### 5. 上游 API key 泄露
风险：
- 上游 key 出现在日志、错误响应、后台页面或前端 bundle。
- 调试日志保留完整 authorization header。
- 管理后台权限过宽。

回归重点：
- 日志脱敏
- error sanitize
- 后台权限
- 环境变量和密钥存储

### 6. 用户 API key 越权
风险：
- 一个用户能查到另一个用户的请求日志或余额。
- 被禁用 key 仍可调用。
- 删除 / 轮换 key 后旧 key 仍有效。

回归重点：
- key scope
- key status
- cache invalidation
- admin 操作审计

### 7. Token 和金额精度错误
风险：
- 小数金额精度丢失。
- provider usage 与平台估算混用。
- 模型价格更新影响历史账单。

回归重点：
- 金额使用定点数或最小货币单位
- 历史价格快照
- usage 来源标记

### 8. 并发下额度被打穿
风险：
- 多个并发请求同时通过余额检查，最终消费超过余额。
- 限流只在单进程有效。
- key 池并发选择同一个已超限 key。

回归重点：
- 原子扣减
- 并发测试
- 分布式限流
- key 状态更新

### 9. Provider 错误处理过度重试
风险：
- 对不可重试错误反复请求，扩大成本。
- 429 后没有 backoff。
- timeout 设置过长拖垮请求池。

回归重点：
- retry policy
- backoff
- timeout
- retryable error classification

### 10. 日志不可追踪
风险：
- 用户请求、provider 请求、账单记录无法关联。
- 只记录成功，不记录失败。
- fallback 后看不到实际命中的 provider。

回归重点：
- request id
- provider request id
- ledger id
- route decision log

## 改动触发回归规则
- 改 stream：必须回归中断、取消、完成、扣费。
- 改 provider：必须回归 response normalizer、error mapper、usage extractor。
- 改路由：必须回归 fallback、重复扣费、日志关联。
- 改计费：必须回归金额精度、余额不足、并发扣减。
- 改后台：必须回归权限、脱敏、操作审计。
- 改模型表：必须回归能力标记和价格快照。

## 什么时候用
- 做中转站 code review 前。
- 上线 provider、stream、路由或计费改动前。
- 复盘线上问题时。

## 不适用场景
- 当前只是方案草稿，尚未进入实现。
- 当前只是普通 AI 应用内部 SDK 调用。

## 来源
- 来自 AI API 中转站方向启动前的风险预判。
- 目前状态为 candidate，尚未经过完整项目验证。

## 待验证问题
- 哪些热点会在首个 MVP 中真实出现。
- 是否需要单独建立 provider 契约测试清单。
- 金额和 usage 的审计模型是否应独立成正式领域文件。

## 最后复核
- 2026-05-05
