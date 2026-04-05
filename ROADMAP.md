# Roadmap

## Phase 1: Gateway Core

- Route matching by `host / path / method`
- HTTP upstream proxying
- Request context with trace and request ID
- Static in-memory configuration
- Minimal config validation

## Phase 2: Policies and Control Plane

- Route and upstream CRUD APIs
- External config storage with `Postgres`
- Config versioning and hot reload
- Middleware chain with:
  - `api-key`
  - `jwt`
  - `header-rewrite`
  - `access-log`

## Phase 3: Production Features

- Rate limiting with `Redis`
- Basic canary and traffic split
- Health checks and retries
- OpenTelemetry metrics and tracing
- Audit log for control plane changes

## Phase 4: Cloud Run Focus

- Cloud Run deployment examples
- Cloud Run tuning guidance
- Stateless multi-instance behavior validation
- Cost and latency benchmarks

## Not Planned for Early Versions

- L4/TCP proxying
- WASM or multi-language plugin runtime
- Large management UI
- Full service mesh features
