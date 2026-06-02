# Tags

Use these controlled vocabularies for future case metadata.

## Categories

- `api-http`
- `database-transactions`
- `queues-background-jobs`
- `reliability-failure-recovery`
- `idempotency-consistency`
- `caching-redis`
- `security-auth`
- `observability-debugging`
- `performance-scaling`
- `deployment-operations`

## Technologies

- `python`
- `fastapi`
- `postgresql`
- `sqlalchemy`
- `alembic`
- `redis`
- `celery`
- `dramatiq`
- `pytest`
- `docker-compose`

## Levels

- `beginner` — Assumes basic web API concepts and simple request/response debugging. Cases at this level focus on foundational mistakes like missing authorization checks or missing request context in logs.
- `junior` — Assumes comfort reading endpoint code, database queries, and cache behavior. Cases at this level focus on common backend pitfalls where the feature works on the surface but breaks under load, stale data, or changing datasets.
- `junior-middle` — Assumes familiarity with retries, write paths, and side effects across requests or jobs. Cases at this level focus on correctness problems where repeated execution creates duplicate work unless command identity is handled explicitly.
- `middle` — Assumes confidence with transactions, concurrent requests, and business invariants in shared data. Cases at this level focus on race conditions where individually valid operations still corrupt state when they overlap.
- `middle-advanced` — Assumes strong knowledge of distributed backend workflows, consistency tradeoffs, and production debugging under concurrency. Cases at this level should involve multi-step failures where fixing one layer is not enough without understanding coordination across systems.
- `advanced` — Assumes deep experience with failure recovery, system boundaries, and hard-to-reproduce production incidents. Cases at this level should involve subtle correctness or reliability failures that require careful reasoning across architecture, data flow, and operational constraints.

## Failure Modes

- `data-leak`
- `duplicate-processing`
- `lost-update`
- `race-condition`
- `stale-data`
- `slow-query`
- `timeout`
- `retry-storm`
- `partial-failure`
- `missing-observability`
- `broken-authz`
- `unsafe-migration`
- `blocking-async-code`
- `worker-crash`
- `cache-stampede`

## Patterns

- `idempotency-key`
- `outbox-pattern`
- `transaction-boundary`
- `select-for-update`
- `eager-loading`
- `request-id`
- `structured-logging`
- `retry-policy`
- `circuit-breaker`
- `dead-letter-queue`
- `healthcheck`
- `graceful-shutdown`
- `rate-limiting`
- `cursor-pagination`
