# System Architecture

## Repository Shape

- `apps/api`: Express, Socket.IO, MongoDB, Redis/BullMQ, provider adapters, workers, migrations, and OpenAPI delivery.
- `apps/web`: Next.js browser client and same-origin REST/OAuth proxy.
- `apps/mobile`: Expo Router Android-first native client.
- `packages/shared`: strict Zod request, response, enum, and realtime contracts.
- `.agents`: canonical engineering, API, database, infrastructure, socket, and mobile documentation.

## Request Path

~~~mermaid
flowchart LR
  Web[Next.js web] -->|same-origin REST proxy| API[Express API]
  Mobile[Expo mobile] -->|Bearer REST| API
  Web -->|WebSocket| Socket[Socket.IO]
  Mobile -->|WebSocket| Socket
  Socket --> Services[Application services]
  API --> Services
  Workers[BullMQ workers and reconcilers] --> Services
  Services --> Repositories[Repositories and units of work]
  Repositories --> Mongo[(MongoDB replica set)]
  Services --> Redis[(Redis)]
~~~

Controllers and socket handlers never own business rules. Services coordinate policies, repositories, transactions, runtime stores, outboxes, and provider ports.

## External Boundaries

- **Cloudflare R2:** private object bytes for attachments, profile media, and voice notes; MongoDB stores ownership, lifecycle, and verified voice-note metadata.
- **LiveKit Cloud:** encrypted WebRTC signaling and media; InTouch authorizes every session and persists call lifecycle.
- **Gemini:** Echo generation behind a provider-neutral AI interface, explicit consent, and authorized context selection.
- **Brevo/SMTP:** transactional mail sent from an encrypted MongoDB outbox.
- **Expo Push/FCM:** mobile push transport driven by encrypted device tokens and durable outboxes.
- **Google:** browser OAuth authorization-code flow and native ID-token verification.
- **Grafana/OTLP and Sentry:** aggregate telemetry and sanitized operational errors.

## State Ownership

| State | Authority |
| --- | --- |
| Users, sessions, organizations, messages, calls, notifications, asset and voice-note metadata | MongoDB |
| Presence, typing, socket limits, voice admission, dedupe leases | Redis |
| Delayed/retryable job scheduling | BullMQ with MongoDB outbox/reconciliation where durability is required |
| Media tracks and transient participant transport | LiveKit |
| Browser/mobile server cache | TanStack Query |

## Deployment

Railway runs the API and Next.js services. Browser REST traffic uses the Next.js same-origin proxy; Socket.IO connects directly to the API. Mobile connects directly to the public API. Managed Atlas, Redis, R2, LiveKit, Gemini, Expo, Grafana, and Sentry remain external services.

Local development runs application processes natively and starts only MongoDB, Redis, Mailpit, and optional LGTM through Compose.
