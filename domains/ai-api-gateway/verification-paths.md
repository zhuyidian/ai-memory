# AI API 中转站验证路径

## 记忆状态
- 状态：candidate
- 替代文件：无
- 上次复核：2026-05-05
- 下次复核触发条件：完成首个可调用 MVP、接入 stream、或上线真实计费后复核

## 适用范围
- 适用于 AI API 中转站实现前确认验证策略、实现后执行回归、上线前检查。
- 重点覆盖 OpenAI-compatible 协议、provider 适配、stream、计费、路由和安全边界。

## 核心立场
- 中转站最怕“请求能通，但账单、stream、fallback 或安全边界悄悄错了”。
- 验证必须按层次进行，先验证协议和 provider，再验证 usage、扣费、并发和异常路径。
- 没有失败路径和扣费路径验证的中转站任务，不应进入交付状态。

## 验证顺序
1. 协议兼容验证
2. provider 适配验证
3. stream 行为验证
4. usage 与计费验证
5. 路由和 fallback 验证
6. 安全与风控验证
7. 运维观测验证

## 分层验证模型
### 1. 协议兼容验证
适合验证：
- `/v1/chat/completions`
- request body 必填和可选字段
- response body 格式
- OpenAI-compatible error shape
- 模型不存在、key 无效、参数非法

目标：
- 确认客户端可以按 OpenAI 兼容方式接入，而不是只适配某个内部调用脚本。

### 2. Provider 适配验证
适合验证：
- 上游鉴权
- 模型名映射
- 请求字段转换
- 响应字段归一
- 上游错误码映射
- usage 字段提取

目标：
- 确认 provider 差异被限制在适配层，不污染对外协议。

### 3. Stream 验证
适合验证：
- SSE chunk 格式
- `[DONE]` 结束事件
- 中途断开
- provider 超时
- 客户端取消请求
- stream 错误事件

目标：
- 确认流式输出不丢 chunk、不乱序、不误结束，并且失败时状态可审计。

### 4. Usage 与计费验证
适合验证：
- prompt tokens
- completion tokens
- total tokens
- provider 未返回 usage 时的处理
- 成功请求扣费
- 失败请求不扣或按规则扣费
- stream 完成和中断后的扣费一致性

目标：
- 确认用户余额、平台成本和请求事实能对上。

### 5. 路由和 Fallback 验证
适合验证：
- 指定模型路由到指定 provider
- provider key 轮询
- 上游 429 / 5xx / timeout
- fallback 是否触发
- fallback 后是否重复扣费
- fallback 后日志是否保留原始失败原因

目标：
- 确认高可用能力不会制造账单和审计错误。

### 6. 安全与风控验证
适合验证：
- 用户 API key 鉴权
- 上游 API key 不出现在响应和日志中
- 日志脱敏
- 用户越权访问
- 单用户限流
- 单 IP 限流
- 余额不足拦截

目标：
- 确认中转站不会因为代理能力扩大泄露和滥用风险。

### 7. 运维观测验证
适合验证：
- 请求日志可查
- provider 错误率可查
- 模型级成本可查
- 慢请求可查
- 异常扣费可追踪
- provider key 状态可观测

目标：
- 确认上线后问题能定位，而不是只能通过用户反馈倒查。

## 按改动类型的最小验证集
### OpenAI-compatible 入口改动
至少验证：
- 成功请求
- 参数非法
- 模型不存在
- 鉴权失败
- response shape

### Provider 适配改动
至少验证：
- 请求字段转换
- 响应字段归一
- 错误码映射
- usage 提取
- provider 超时

### Stream 改动
至少验证：
- 正常完整 stream
- 客户端取消
- 上游中断
- `[DONE]`
- stream 后 usage 或估算策略

### 计费改动
至少验证：
- 成功扣费
- 失败不误扣
- 余额不足拦截
- 重试 / fallback 不重复扣费
- 金额精度

### Key 池和路由改动
至少验证：
- key 轮询
- key 禁用
- provider 失败切换
- 并发请求
- 日志记录真实命中的 provider 和 key 标识

## 输出要求
验证结果至少说明：
- 跑了什么
- 没跑什么
- 为什么没跑
- 哪些实际通过
- 哪些仍是剩余风险

## 什么时候用
- 写中转站 task 验收标准前。
- 实现 provider、stream、计费或路由前。
- 上线前做回归检查。

## 不适用场景
- 当前只是静态文档或产品方案。
- 当前只是普通 AI 应用调用单一 provider，不承担中转、计费或路由责任。

## 来源
- 来自 AI API 中转站方向启动前的验证模型预判。
- 目前状态为 candidate，尚未经过完整项目验证。

## 待验证问题
- 是否需要为每个 provider 建独立契约测试。
- stream usage 缺失时的估算策略是否能接受。
- 真实压测下 key 池轮询和限流策略是否足够稳定。

## 最后复核
- 2026-05-05
