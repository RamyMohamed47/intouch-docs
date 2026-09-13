# ADR-002: MongoDB Persistence

- **Status:** Accepted

## Decision

Use MongoDB through Mongoose repositories as the durable application database. Production uses Atlas; local development uses a single-node Docker replica set. Transactions protect multi-document invariants such as organization creation/deletion, invitation acceptance, message/asset claims, notifications, and outbox writes.

MongoDB stores durable domain and delivery intent. Redis remains ephemeral runtime state, BullMQ schedules work, and provider systems never replace MongoDB authority.


## Decision Trade-offs

### Chosen: MongoDB with Mongoose repositories and transactions

**Pros**

- Document shapes fit conversation, message, notification, and provider metadata well.
- Replica-set transactions protect multi-document domain invariants.
- Atlas and the local replica-set workflow provide compatible production/development behavior.

**Cons**

- Correctness depends on indexes and disciplined repository scoping rather than relational foreign keys.
- Transactions require a replica set or sharded cluster and add operational constraints.
- Highly relational reporting can require denormalization or aggregation work.

### Alternative: PostgreSQL

**Pros**

- Strong relational constraints, joins, and mature analytical querying.
- Transactions and referential integrity are first-class defaults.

**Cons**

- Would require a different persistence model and migration of the established document-oriented codebase.
- Rapidly evolving nested DTO metadata may require more joins or JSON columns.

### Alternative: Redis or provider state as primary storage

**Pros**

- Low-latency reads for ephemeral activity.

**Cons**

- Unsuitable as the sole authority for durable messages, sessions, calls, assets, and notifications.
- Provider outages or eviction could destroy business state.

## Consequences

- The API refuses a standalone MongoDB deployment because required transactions need a replica set or sharded cluster.
- Schemas, indexes, bounded queries, idempotent migrations, and repository abstractions are part of application correctness.
- High-churn collections use cursor pagination and purpose-built indexes rather than unbounded scans.
