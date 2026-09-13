# ADR-008: Distributed Runtime State and Jobs

- **Status:** Accepted

## Decision

Use Redis for ephemeral cross-replica state: Socket.IO adapter rooms, presence/typing leases, authenticated abuse limits, active socket limits, voice admission/capacity, and webhook deduplication. Use BullMQ for delayed/retryable work including mail, assets, push, call alerts, voice deadlines, and reconciliation.

MongoDB remains durable authority. Jobs carry opaque IDs where a MongoDB outbox exists, and reconcilers recover committed intent missed during process failure. Production runtime operations fail closed when Redis is required and unavailable.


## Decision Trade-offs

### Chosen: Redis runtime state, BullMQ scheduling, and MongoDB durable intent

**Pros**

- Presence, typing, socket rooms, voice admission, and rate limits remain consistent across replicas.
- Delayed work and retries survive API request completion.
- MongoDB outboxes and reconcilers recover jobs missed during process or Redis disruption.

**Cons**

- Adds Redis operations, Lua/atomicity concerns, queues, workers, and monitoring burden.
- Critical runtime operations become unavailable when required Redis is down.
- Idempotency is mandatory because jobs and webhooks may repeat or arrive out of order.

### Alternative: In-memory state and polling only

**Pros**

- Simple local setup and no Redis dependency.

**Cons**

- Replicas disagree about presence, limits, rooms, and voice capacity.
- Process restarts erase leases and delayed scheduling state.

### Alternative: Redis/BullMQ as the sole durable authority

**Pros**

- Fewer MongoDB outbox records and lower scheduling latency.

**Cons**

- Queue loss or eviction can discard committed business intent.
- Domain state and delivery state can diverge without a durable reconciler.

## Consequences

Multiple API replicas share consistent runtime state and scheduling. Redis loss can make readiness fail and realtime/job operations unavailable, but it cannot bypass tenant authorization or erase durable MongoDB state.
