# InTouch Documentation

This vault documents the implemented InTouch platform. Human-facing material
is organized into numbered sections. Repository-facing copies under
`90-Engineering Context` mirror canonical engineering notes from `.agents` in
the application repository.

## Current Snapshot

- npm workspace monorepo with API, Next.js web, Expo mobile, and shared Zod contracts.
- 95 documented REST operations across 70 path templates.
- 23 typed Socket.IO events: 7 client events and 16 server events.
- MongoDB transactions, Redis distributed runtime state, and BullMQ durable jobs.
- Private Cloudflare R2 assets, LiveKit audio/video/screen sharing, Gemini-powered Echo, Expo push, OpenTelemetry, Grafana, and Sentry.
- Android-first mobile client through Mobile V2.0; iOS runtime acceptance and native call-system integration remain deferred.

## Start Here

- [[02-Product/Readme|Product]]
- [[03-Architecture/Readme|Architecture]]
- [[04-Database/Readme|Database]]
- [[05-Backend/Readme|Backend]]
- [[06-Frontend/Readme|Frontend]]
- [[07-API Contract/Readme|API Contract]]
- [[08-Socket.IO/Readme|Socket.IO]]
- [[09-OpenAPI/Readme|OpenAPI]]
- [[10-Shared Contracts/Readme|Shared Contracts]]
- [[11-Deployment/Readme|Deployment]]
- [[12-Architectural Decision Records(ADR)/Readme|Architecture Decision Records]]
- [[13-Infrastructure/Readme|Infrastructure]]
- [[14-Mobile/Readme|Mobile]]

## Canonical Sources

- REST contract: `.agents/api/openapi.yaml`
- Realtime contract: `.agents/sockets/Socket Events.md` and `@intouch/shared/realtime`
- Database model: `.agents/database/ERD.md`
- Implementation guidance: `.agents/AI_CONTEXT.md`, `.agents/PROJECT_STRUCTURE.md`, and `.agents/IMPLEMENTAION_GUIDE.md`
- Mirrored repository context: [[InTouch/15-Engineering Context/Readme|Engineering Context]]

Last aligned with the repository: 2026-09-14.
