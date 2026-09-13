# API Principles

- Base path: `/api/v1`.
- OpenAPI 3.1 is the canonical HTTP documentation; shared Zod schemas are the runtime source of truth.
- Protected routes use short-lived Bearer access tokens.
- Browser refresh/logout use an HttpOnly cookie, allowed Origin, and `X-CSRF-Protection: 1`; native mobile auth uses explicit refresh-token request bodies.
- Successful responses use domain-specific top-level objects such as `{ user }`, `{ message }`, or `{ conversations, nextCursor }`; there is no universal `data` envelope.
- Errors always use `{ success: false, error: { code, message } }`.
- Destructive success normally returns `204 No Content`.
- High-churn collections use opaque cursor pagination. Bounded configuration/roster collections may return complete arrays.
- Unknown input keys are rejected by strict schemas.
- Idempotent create-or-get, lifecycle transitions, outbox jobs, and webhooks tolerate retries.
- Authorization is resource-based and tenant-scoped; identifiers never grant access by themselves.
- REST owns durable mutations. Socket.IO is not an alternative message-write API.

See [[7-API Contract/API Reference|API Reference]], [[9-OpenAPI/Readme|OpenAPI]], and [[8-Socket.IO/Socket Events|Socket.IO Events]].
