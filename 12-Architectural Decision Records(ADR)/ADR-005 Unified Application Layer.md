# ADR-005: Unified Application Service Layer

- **Status:** Accepted

## Context

REST controllers, Socket.IO handlers, BullMQ workers, provider webhooks, reconcilers, and migrations may initiate or complete related domain operations. Duplicating rules across transports would make authorization and lifecycle behavior inconsistent.

## Decision

Application services own business rules, policies, transaction orchestration, and provider-port coordination. Controllers and socket handlers remain transport adapters. Repositories own persistence queries. Workers and reconcilers invoke service-level operations or narrow idempotent use cases rather than writing domain state ad hoc.

Realtime/provider delivery occurs only after transaction commit. Failures are logged and retried/reconciled without rolling back already-committed domain state.


## Decision Trade-offs

### Chosen: Service-owned business logic behind thin transports

**Pros**

- REST, Socket.IO, workers, webhooks, and reconcilers reuse the same domain rules.
- Authorization, transactions, retries, and lifecycle transitions can be unit tested directly.
- Repositories remain replaceable persistence boundaries rather than policy containers.

**Cons**

- Introduces interfaces, dependency wiring, mappers, and additional files.
- Poorly scoped services can become large orchestration objects.
- Developers must resist bypassing services from workers or handlers.

### Alternative: Business logic in controllers or socket handlers

**Pros**

- Less ceremony for a very small prototype.

**Cons**

- Duplicates behavior across transports and makes rollback/concurrency testing difficult.
- Couples domain rules to Express or Socket.IO APIs.

### Alternative: Business logic in repositories

**Pros**

- Persistence operations and rules appear colocated.

**Cons**

- Mixes policy with database mechanics and makes provider orchestration awkward.
- Encourages transport-specific repositories and hidden authorization assumptions.

## Consequences

- One behavior path is reusable from HTTP, realtime lifecycle, jobs, and scripts.
- Services require explicit dependency interfaces and focused tests.
- Additional layers create some boilerplate but make concurrency, rollback, and authorization testable.
