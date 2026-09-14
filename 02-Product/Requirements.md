# Product Requirements

## Identity and Sessions

- Users can register with email/password, verify email, reset passwords, and sign in with Google.
- Browser refresh credentials remain in rotating HttpOnly cookies; native refresh credentials use the mobile token endpoints and SecureStore.
- Access tokens are short-lived Bearer JWTs. Sessions are revocable and refresh tokens are stored only as hashes.

## Organizations and Access

- Organizations are `PUBLIC` or `PRIVATE` and have exactly one `OWNER`; other memberships use `MEMBER`.
- Owners manage organization settings, logos, categories, channels, private participants, and voice-channel moderation.
- Registered users can join public organizations. Private organizations require a valid invitation.
- Authorization always verifies organization membership plus conversation participation where applicable.

## Conversations and Messaging

- Conversations are `CHANNEL` or `DIRECT`; channels have immutable `TEXT` or `VOICE` kinds.
- Text channels may be public or private. Direct conversations are one-to-one and organization-scoped.
- Messages support text, private attachments, replies, UTF-16 mention ranges, one reaction per user, editing, redacted deletion, receipts, and immutable call entries.
- Message writes use REST. Socket.IO distributes committed facts and invalidation events.

## Realtime and Notifications

- Authenticated clients receive scoped presence, typing, message, receipt, reaction, notification, call, and occupancy events.
- Redis shares leases, rooms, presence, typing, rate limits, and voice admission across API replicas.
- Durable notifications cover invitations, accepted invitations, grouped direct messages, mentions, replies, and reactions.
- Category preferences and organization/conversation mutes suppress interruption and push, not durable history or unread message state.

## Media

- Direct messages support audio and video calls with ringing, accept, decline, cancel, timeout, busy, reconnect, and end states.
- Voice channels support ten participants, microphone/camera controls, screen sharing, and owner moderation.
- InTouch owns authorization and lifecycle; LiveKit owns WebRTC media and signaling.

## Search and Echo

- Authorized users can search messages, channels, and people within the active organization.
- Echo supports workspace/conversation questions, summaries, action items, and composer transformations.
- Echo requires organization enablement and per-user consent, and never bypasses source authorization.

## Operations

- `/health` proves process liveness; `/ready` checks critical dependencies.
- Structured logs, bounded OpenTelemetry metrics/traces, Sentry errors, and Grafana dashboards avoid message content and identifiers.
- Local infrastructure is explicit and reproducible through Docker Compose; application processes remain native during development.

## Quality Constraints

- External payloads use strict shared Zod schemas.
- Business rules live in services, persistence in repositories, and transport logic in thin controllers/handlers.
- Multi-document invariants use MongoDB transactions; delayed provider work uses durable outboxes and BullMQ reconciliation.
- Web and mobile remain online-first; offline queued writes are out of scope.
