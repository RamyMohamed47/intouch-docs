# InTouch Documentation

This vault documents the implemented InTouch platform. It is synchronized with the application repository, while the files under `AI Context`, `13-Infrastructure`, and `14-Mobile` mirror the canonical engineering notes in `.agents`.

## Current Snapshot

- npm workspace monorepo with API, Next.js web, Expo mobile, and shared Zod contracts.
- 95 documented REST operations across 70 path templates.
- 23 typed Socket.IO events: 7 client events and 16 server events.
- MongoDB transactions, Redis distributed runtime state, and BullMQ durable jobs.
- Private Cloudflare R2 assets, LiveKit audio/video/screen sharing, Gemini-powered Echo, Expo push, OpenTelemetry, Grafana, and Sentry.
- Android-first mobile client through Mobile V2.0; iOS runtime acceptance and native call-system integration remain deferred.

## Start Here

- [[2-Product/Product Vision|Product Vision]]
- [[2-Product/Requirements|Requirements]]
- [[2-Product/Roadmap|Roadmap]]
- [[3-Architecture/Architecture|Architecture]]
- [[4-Database/Entities|Entities]]
- [[7-API Contract/API Reference|API Reference]]
- [[8-Socket.IO/Socket Events|Socket.IO Events]]
- [[9-OpenAPI/Readme|OpenAPI]]
- [[10-Shared Contracts/Shared Architecture|Shared Contracts]]
- [[12-Architectural Decision Records(ADR)/Readme|Architecture Decision Records]]
- [[13-Infrastructure/Local Docker Infrastructure|Local Infrastructure]]
- [[13-Infrastructure/Observability|Observability]]
- [[13-Infrastructure/AI Assistant|Echo AI Assistant]]
- [[14-Mobile/Mobile V2.0|Mobile V2.0]]

## Canonical Sources

- REST contract: `.agents/api/openapi.yaml`
- Realtime contract: `.agents/sockets/Socket Events.md` and `@intouch/shared/realtime`
- Database model: `.agents/database/ERD.md`
- Implementation guidance: `.agents/AI_CONTEXT.md`, `.agents/PROJECT_STRUCTURE.md`, and `.agents/IMPLEMENTAION_GUIDE.md`

Last aligned with the repository: 2026-09-13.
