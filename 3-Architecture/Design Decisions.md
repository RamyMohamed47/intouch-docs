# Design Decisions

This page summarizes active decisions. Detailed rationale lives in [[12-Architectural Decision Records(ADR)/Readme|ADRs]].

| Area | Decision |
| --- | --- |
| Tenancy | One MongoDB database with explicit `organizationId` scoping and centralized authorization. |
| Domain model | One `Conversation` model for `CHANNEL` and `DIRECT`; channels have immutable `TEXT` or `VOICE` kind. |
| Layers | Thin controllers/socket handlers, service-owned business logic, repository-owned persistence. |
| Contracts | Strict Zod schemas in `@intouch/shared`; OpenAPI is maintained alongside them. |
| Browser sessions | Short-lived Bearer access token plus rotating HttpOnly refresh cookie and CSRF protection. |
| Native sessions | Dedicated mobile endpoints return rotating refresh credentials for SecureStore; no browser cookie semantics. |
| Durable state | MongoDB is authoritative and transaction-capable. |
| Distributed state | Redis stores ephemeral leases, limits, presence, typing, and voice admission; production fails closed if unavailable. |
| Jobs | BullMQ schedules work while MongoDB outboxes/reconcilers preserve durable intent. |
| Assets | Private Cloudflare R2 with direct presigned upload and authorized short-lived reads. |
| Media | LiveKit transports media/signaling; InTouch owns authorization, capacity, lifecycle, history, and moderation. |
| AI | Echo uses authorized bounded context, explicit enablement/consent, quotas, and a provider-neutral Gemini adapter. |
| Realtime | REST performs durable writes; Socket.IO distributes scoped committed facts and invalidations. |
| Observability | Readiness is independent of optional telemetry; logs, metrics, traces, and errors are sanitized. |
| Mobile | Expo Router Android-first app shares contracts but keeps native session and media lifecycles in dedicated providers. |
