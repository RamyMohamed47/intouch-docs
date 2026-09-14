# Backend Architecture

The backend is a strict TypeScript Express application under `apps/api`. It
serves REST, Socket.IO, provider webhooks, health endpoints, and background-job
coordination while keeping business rules independent of transport code.

## Composition and Startup

- `src/bootstrap.ts` loads environment configuration and initializes optional
  observability before importing the application composition root.
- `src/server.ts` constructs repositories, services, provider adapters,
  background jobs, runtime stores, REST routers, Socket.IO, and graceful
  shutdown handling.
- `src/app.ts` composes HTTP middleware and routers and exposes `/health` and
  `/ready`.

`/health` confirms that the process is alive. `/ready` reports whether MongoDB,
Redis runtime state, and the configured background-job provider can serve work.
Optional telemetry providers never control readiness.

## Layering

The normal feature path is:

```text
Route -> Controller -> Service -> Repository -> Model
```

- Routes attach authentication, authorization prerequisites, validation, and
  rate limits.
- Controllers translate HTTP input and output but do not own business rules.
- Services enforce domain policy and coordinate transactions, providers, and
  post-commit realtime delivery.
- Repositories own MongoDB persistence and aggregation behavior.
- Models define storage shape and indexes, not authorization.

Socket handlers follow the same service boundary. They do not bypass domain
authorization or perform durable writes independently from application
services.

## Feature Modules

Feature-oriented modules under `src/modules` cover authentication,
organizations, memberships and invitations, categories, conversations,
messages, reactions, receipts, notifications and push, uploads and assets,
search, Echo, presence, abuse protection, and voice/call lifecycle.

Cross-cutting infrastructure under `src/infrastructure` owns Redis, BullMQ, and
observability runtime composition. External systems are accessed through
provider interfaces so controllers and domain services do not depend directly
on vendor SDKs.

## Data and Consistency

- MongoDB is the durable source of truth and transactions protect multi-record
  domain changes.
- Redis stores distributed ephemeral state such as presence, admission leases,
  rate limits, and deduplication state.
- BullMQ schedules delayed and retryable work. MongoDB outboxes preserve
  durable intent where losing a queued job would lose user-visible work.
- Realtime events are emitted after commit and carry scoped facts or cache
  invalidations, never uncommitted state.

## Boundaries and Errors

Public inputs and outputs are parsed through shared Zod contracts. Operational
errors use typed application errors and a centralized error handler. Expected
validation, authentication, authorization, conflict, not-found, and throttling
responses remain controlled 4xx responses; unexpected failures are sanitized
for logs and Sentry.

## Verification

From the monorepo root:

```bash
npm run typecheck --workspace @intouch/api
npm test --workspace @intouch/api
npm run build:api
```

The normal repository-wide verification command is `npm run check`.
