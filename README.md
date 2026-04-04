# cloudrun-gateway

一个运行在 Cloud Run 上的可编程 API Gateway / BFF Gateway。

## 为什么做这个项目

这个项目不是为了重复造一个通用网关，而是为了围绕 Cloud Run 的约束做一套更轻、更适合 Serverless 场景的网关实现：

- Data plane 无状态，适合 Cloud Run 横向扩缩容
- 配置存储外置，便于多实例共享
- 聚焦 L7 能力：路由、鉴权、限流、Header 改写、灰度、观测
- 优先满足 BFF / 内部 API 聚合 / 边车前置网关这类场景

## 第一阶段目标

- 支持 `host / path / method` 路由匹配
- 支持 HTTP upstream 转发
- 支持基础中间件链
  - `request-id`
  - `api-key`
  - `jwt`
  - `rate-limit`
  - `header-rewrite`
  - `access-log`
- 支持管理 API
  - route CRUD
  - upstream CRUD
  - middleware binding
- 支持配置热加载
- 支持 OpenTelemetry 指标和 tracing

## 非目标

- 不在第一版支持 TCP / L4 代理
- 不在第一版支持多语言插件运行时
- 不在第一版做完整控制台
- 不在第一版对标 APISIX / Envoy 的全量能力

## 初步架构

- `control plane`
  - 管理 API
  - 配置校验
  - 配置持久化
- `data plane`
  - 请求接入
  - 路由匹配
  - 中间件链执行
  - upstream 转发
- `storage`
  - `Postgres`：配置与审计
  - `Redis`：限流 / 短缓存

## 建议技术栈

- `Go`
- `net/http` + `httputil.ReverseProxy`
- `Postgres`
- `Redis`
- `OpenTelemetry`
- 部署目标：`Cloud Run`

## 开发计划

见 [docs/INITIAL_PLAN.md](docs/INITIAL_PLAN.md)。
