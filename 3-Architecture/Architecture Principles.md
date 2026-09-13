# Architecture Principles

1. **Validate at boundaries.** REST, Socket.IO, environment, provider webhook, and persisted-to-public transformations use runtime schemas.
2. **Keep transports thin.** Controllers and socket handlers authenticate, validate, invoke services, and serialize outcomes.
3. **Put business rules in services.** Services enforce authorization, lifecycle transitions, quotas, and transaction boundaries.
4. **Keep persistence in repositories.** Mongoose and database query details do not leak into controllers or clients.
5. **Share public contracts.** API, web, and mobile consume `@intouch/shared`; OpenAPI documents the same shapes.
6. **Treat MongoDB as durable authority.** Redis and provider state are replaceable runtime projections and queues.
7. **Fail closed for authorization.** Redis, storage, media, AI, and webhook failures never weaken access checks.
8. **Emit after commit.** Realtime and provider delivery follow successful domain persistence and are idempotent/reconcilable.
9. **Protect tenant boundaries.** Every organization resource and conversation action proves membership and scoped access.
10. **Observe without leaking.** Logs and metrics use bounded operational labels, never message/Echo content, tokens, presigned URLs, or user identifiers.
11. **Prefer explicit degradation.** Readiness reflects critical dependencies; optional telemetry cannot take the application down.
12. **Test contracts and concurrency.** Strict schemas, idempotency, retries, race conditions, and cross-replica behavior are first-class test targets.
