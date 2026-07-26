
## Status

Accepted

---

## Context

InTouch is designed as a Software-as-a-Service (SaaS) application where multiple organizations (tenants) share the same infrastructure while remaining completely isolated from one another.

The application must support users belonging to multiple organizations without exposing one organization's data to another.

---

## Decision

InTouch will use a **Single Database, Shared Collections** multi-tenancy strategy.

All tenant-specific resources will contain an `organizationId` field that identifies the owning organization.

Examples include:

- Categories
- Conversations
- Messages
- Memberships

Users exist globally and are not owned by any organization.

Relationships between users and organizations are represented through the Membership collection.

---

## Architecture

```text
                User
                  │
                  │
             Membership
                  │
                  │
            Organization
                  │
      ┌───────────┴───────────┐
      │                       │
 Category                Conversation
                              │
                           Message
```

---

## Authorization Strategy

Every request that accesses organization resources must verify:

1. The requested resource belongs to the specified organization.
2. The authenticated user is a member of that organization.

This ensures complete tenant isolation.

---

## Query Pattern

Example:

```javascript
Conversation.find({
    organizationId: currentOrganizationId
})
```

Every tenant-aware query must include `organizationId`.

---

## Alternatives Considered

### Database Per Tenant

Pros

- Strong isolation
- Easier tenant backups
- Separate scaling

Cons

- Operational complexity
- Difficult migrations
- Higher infrastructure cost
- Unnecessary for the expected project scale

---

### Collection Per Tenant

Pros

- Logical separation

Cons

- Difficult to maintain
- Large number of collections
- Poor scalability

---

## Consequences

### Advantages

- Simple deployment
- Lower operational overhead
- Excellent fit for portfolio scale
- Supports thousands of organizations
- Easy horizontal scaling

### Trade-offs

- Every tenant-aware query must include `organizationId`.
- Authorization mistakes could expose tenant data if queries are not properly scoped.