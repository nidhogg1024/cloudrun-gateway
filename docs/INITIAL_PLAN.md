# cloudrun-gateway 初步方案

## 项目定位

`cloudrun-gateway` 是一个面向 Cloud Run / Serverless 服务的轻量 API Gateway 与 BFF Gateway。

它的目标不是成为通用网关平台，而是围绕“无状态实例 + 外部配置存储 + 快速扩缩容”的运行模型，沉淀一套简洁但可扩展的网关内核。

## 目标用户

- 使用 Cloud Run 部署内部服务的团队
- 需要一层统一 API 入口的 BFF 场景
- 希望在接入层统一做鉴权、限流、Header 改写、灰度和观测的后端开发者

## 核心能力

### 1. 路由与转发

- 基于 `host / path / method` 匹配 Route
- Route 关联 Upstream
- 支持 URI 重写、Header 改写、超时配置

### 2. 中间件链

采用“请求阶段串行执行”的模型，后续可演进为更细粒度 phase。

第一版中间件：

- `request-id`
- `api-key`
- `jwt`
- `rate-limit`
- `header-rewrite`
- `access-log`

### 3. 配置体系

- 配置外置存储于 `Postgres`
- data plane 通过轮询或订阅方式拉取最新配置
- 配置变更保留版本号，便于审计和回滚

### 4. 观测能力

- Access log
- 请求耗时
- 状态码分布
- 上游错误统计
- Trace ID 透传

## 初版数据模型

### Route

- `id`
- `name`
- `host`
- `path`
- `methods`
- `upstream_id`
- `middlewares`
- `enabled`
- `version`

### Upstream

- `id`
- `name`
- `target_url`
- `timeout_ms`
- `retries`
- `health_check`

### MiddlewareBinding

- `route_id`
- `kind`
- `config_json`
- `order_no`

## 技术难点

- 无状态实例下的配置热更新
- Cloud Run 横向扩容时的限流一致性
- 代理超时与客户端取消的正确传递
- 中间件链顺序与可扩展性设计
- 日志、指标、链路追踪的统一打通

## 里程碑

### Milestone 1

- 单进程网关
- 静态内存配置
- Route + Upstream + Reverse Proxy
- request-id / access-log

### Milestone 2

- Postgres 配置存储
- 管理 API
- 配置热加载
- api-key / jwt / header-rewrite

### Milestone 3

- Redis 限流
- 灰度发布基础能力
- OTel 指标与 tracing
- Cloud Run 部署模板

## 仓库结构建议

```text
cmd/
  gateway/
  control-plane/
internal/
  config/
  proxy/
  router/
  middleware/
  storage/
  telemetry/
api/
deploy/
docs/
```
