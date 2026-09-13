# ADR-001: Shared-Database Multi-Tenancy

- **Status:** Accepted

## Decision

Use one MongoDB database and shared collections. Tenant-owned records carry an `organizationId`, and services resolve the owning organization before authorizing access. Private channels and direct messages additionally require an active participant record.

Users, authentication sessions, action tokens, default wallpaper preferences, and push installations are user-scoped rather than tenant-owned. Any operation that enters an organization or conversation context still proves membership/access explicitly.


## Decision Trade-offs

### Chosen: Shared database and shared collections

**Pros**

- One schema and migration path serves every organization.
- Operational cost and connection management stay low at the current scale.
- Cross-organization features can reuse one repository and transaction model.

**Cons**

- Every query, index, cache key, event, and provider operation must preserve tenant scope.
- A missed authorization predicate can create a cross-tenant disclosure risk.
- Per-tenant restore and physical isolation are harder than database-per-tenant designs.

### Alternative: Database per organization

**Pros**

- Strong physical isolation and simpler tenant-specific export or restore.
- A tenant can be moved or scaled independently.

**Cons**

- High connection, migration, monitoring, and provisioning overhead.
- Cross-tenant operational queries and shared deployments become more complex.

## Consequences

- A single deployment can serve many organizations without database-per-tenant operations.
- Every repository query and unique index must include the correct tenant scope where applicable.
- Socket rooms, Redis keys, search results, assets, Echo context, LiveKit credentials, and notifications must not be treated as alternative authorization mechanisms.
- Cross-tenant tests are mandatory for public/private resources and provider boundaries.

Database-per-tenant isolation was rejected for the current scale because its migration, connection, and operational cost outweigh the benefit.
