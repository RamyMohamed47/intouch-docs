# ADR-009: Private Asset Storage

- **Status:** Accepted

## Decision

Store attachment, avatar, and organization-logo bytes in a private Cloudflare R2 bucket. MongoDB stores ownership and lifecycle metadata. Clients upload directly through short-lived content-bound presigned PUT URLs, then the API verifies signatures and promotes/claims objects. Reads require authorization and return short-lived presigned GET URLs.

Object keys, credentials, ETags, and durable public URLs never appear in public DTOs. Cleanup is asynchronous and idempotent through BullMQ or leased polling.


## Decision Trade-offs

### Chosen: Private R2 with direct presigned transfer and API-controlled metadata

**Pros**

- Large bytes bypass application servers while reads and claims remain authorized.
- R2 is S3-compatible and avoids permanent public URLs.
- MongoDB transactions can bind asset ownership to messages, users, and organizations.

**Cons**

- Requires CORS, signature verification, quotas, lifecycle states, and abandoned-object cleanup.
- Presigned URLs are bearer capabilities during their short lifetime.
- Client upload and API completion are a multi-step workflow that must handle partial failure.

### Alternative: Proxy every upload and download through the API

**Pros**

- Central authorization and simpler browser CORS behavior.

**Cons**

- Consumes API bandwidth, memory, and connection capacity for large files.
- Makes horizontal scaling and download performance more expensive.

### Alternative: Public object URLs

**Pros**

- Very simple rendering and caching.

**Cons**

- Revoked organization/conversation access cannot reliably revoke copied URLs.
- Private attachments, avatars, and logos become discoverable bearer resources.

### Alternative: Managed transformation-first media service

**Pros**

- Rich image transformations and CDN tooling out of the box.

**Cons**

- Adds vendor-specific asset semantics and cost for general files.
- Still requires private authorization and durable ownership metadata.

## Consequences

Application servers avoid proxying large bytes while retaining authorization and quota control. The design requires explicit CORS, signature verification, abandoned-upload cleanup, and transaction-aware claims.
