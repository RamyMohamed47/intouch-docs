# InTouch Engineering Documentation

This repository is the engineering documentation vault for
[InTouch](https://github.com/RamyMohamed47/intouch-chatapp), a multi-tenant
communication platform with web and Android clients, realtime messaging,
LiveKit-powered media, and the Echo AI assistant.

The documentation covers product scope, architecture, contracts, deployment,
infrastructure, mobile releases, and the decisions behind the implementation.
It is designed to be readable both on GitHub and as an Obsidian vault.

## Platform Snapshot

- npm workspace monorepo containing an Express API, Next.js web client, Expo
  mobile client, and shared Zod contracts.
- Public/private text channels, direct messages, replies, mentions, reactions,
  attachments, cross-platform voice notes, typing, presence, unread state, and
  read receipts.
- LiveKit audio/video direct calls, ten-person voice channels, camera controls,
  Android screen sharing, moderation, and durable call history.
- Gemini-powered Echo summaries, action items, contextual answers, and composer
  transformations.
- MongoDB transactions, Redis runtime state, BullMQ jobs, private Cloudflare R2
  assets, Expo push notifications, OpenTelemetry/Grafana, and Sentry.
- 95 documented REST operations across 70 path templates and 23 typed
  Socket.IO events.

## Start Here

- [Documentation Home](01-Home/Home.md) provides the current system snapshot
  and links to every major area.
- [Product](02-Product/Readme.md) explains the vision, requirements, delivered
  scope, and roadmap.
- [System Architecture](03-Architecture/Architecture.md) describes application
  boundaries, state ownership, providers, and deployment topology.
- [Architecture Decision Records](12-Architectural%20Decision%20Records%28ADR%29/Readme.md)
  document accepted decisions, alternatives, pros, cons, consequences, and
  revisit triggers.
- [Mobile](14-Mobile/Readme.md) tracks the Expo client from its foundation to
  the current Android-first media release.

## Documentation Map

| Section | Contents |
| --- | --- |
| [01 - Home](01-Home/Readme.md) | Vault entry point and navigation |
| [02 - Product](02-Product/Readme.md) | Vision, requirements, and roadmap |
| [03 - Architecture](03-Architecture/Readme.md) | System design, principles, boundaries, and multi-tenancy |
| [04 - Database](04-Database/Readme.md) | Persisted entities and relationships |
| [05 - Backend](05-Backend/Readme.md) | API architecture, services, repositories, and workers |
| [06 - Frontend](06-Frontend/Readme.md) | Next.js web architecture and client state |
| [07 - API Contract](07-API%20Contract/Readme.md) | REST behavior grouped by resource |
| [08 - Socket.IO](08-Socket.IO/Readme.md) | Realtime architecture, events, rooms, and reconnection |
| [09 - OpenAPI](09-OpenAPI/Readme.md) | OpenAPI conventions, examples, pagination, and versioning |
| [10 - Shared Contracts](10-Shared%20Contracts/Readme.md) | Zod DTOs, event types, and validation strategy |
| [11 - Deployment](11-Deployment/Readme.md) | Railway, Expo EAS, and environment boundaries |
| [12 - ADRs](12-Architectural%20Decision%20Records%28ADR%29/Readme.md) | Architectural decisions and trade-offs |
| [13 - Infrastructure](13-Infrastructure/Readme.md) | Redis, BullMQ, observability, AI, and local infrastructure |
| [14 - Mobile](14-Mobile/Readme.md) | Mobile architecture and release history |
| [15 - Engineering Context](15-Engineering%20Context/Readme.md) | Mirrors of canonical repository-facing engineering notes |
| [Diagrams](Diagrams/High%20Level%20Architecture.md) | Architecture, data-flow, entity, and sequence diagrams |

`Garage.md`, `Progress.md`, `Journal`, and `Templates` are working areas rather
than authoritative system documentation.

## Suggested Reading Paths

### Product Overview

1. [Product Vision](02-Product/Product%20Vision.md)
2. [Requirements](02-Product/Requirements.md)
3. [Roadmap](02-Product/Roadmap.md)

### Architecture Review

1. [System Architecture](03-Architecture/Architecture.md)
2. [Architecture Principles](03-Architecture/Architecture%20Principles.md)
3. [Voice Notes](03-Architecture/Voice%20Notes.md)
4. [Database Entities](04-Database/Entities.md)
5. [ADR Index](12-Architectural%20Decision%20Records%28ADR%29/Readme.md)
6. [High-Level Architecture Diagram](Diagrams/High%20Level%20Architecture.md)

### API Consumer

1. [API Principles](07-API%20Contract/API%20Principles.md)
2. [Authentication](07-API%20Contract/Authentication.md)
3. [API Reference](07-API%20Contract/API%20Reference.md)
4. [Socket Events](08-Socket.IO/Socket%20Events.md)
5. [OpenAPI Guide](09-OpenAPI/Readme.md)

### Operations and Deployment

1. [Environment Boundaries](11-Deployment/Environment%20Boundaries.md)
2. [Railway Deployment](11-Deployment/Railway%20Deployment.md)
3. [Expo EAS](11-Deployment/Expo%20EAS.md)
4. [Infrastructure](13-Infrastructure/Readme.md)
5. [Observability](13-Infrastructure/Observability.md)

## Source of Truth

The application repository remains authoritative for executable behavior:

- REST contract: `.agents/api/openapi.yaml`
- Realtime contract: `.agents/sockets/Socket Events.md` and
  `@intouch/shared/realtime`
- Database model: `.agents/database/ERD.md`
- Engineering guidance: `.agents/AI_CONTEXT.md`, `.agents/PROJECT_STRUCTURE.md`,
  and `.agents/IMPLEMENTAION_GUIDE.md`

Files under [15 - Engineering Context](15-Engineering%20Context/Readme.md) are
mirrors of those canonical sources. Update the application repository first,
validate the change against the implementation, and then refresh the mirror.
The numbered sections in this repository are the canonical human-facing
documentation.

## Using the Vault

### GitHub

Browse from this README using the standard Markdown links. Mermaid diagrams
render directly on supported GitHub pages.

### Obsidian

1. Clone this repository.
2. Open the repository directory as an Obsidian vault.
3. Start at `01-Home/Home.md`.
4. Use backlinks and the graph view to follow cross-section relationships.

No community plugins are required to read the documentation.

## Contributing

- Verify behavior against the InTouch application repository before changing a
  technical claim.
- Prefer links to existing documents over duplicating detailed contracts.
- Keep ADR identifiers stable; add a new ADR instead of renumbering history.
- Record meaningful trade-offs, rejected alternatives, and revisit triggers in
  every ADR.
- Never commit credentials, access tokens, private URLs, or copied production
  data.
- Check links and Mermaid blocks in both GitHub and Obsidian when changing
  navigation or diagrams.

## Current Scope

The documentation is aligned through Mobile V2.0 and the cross-platform voice
notes release. Public iOS acceptance, native call-system integration,
guaranteed offline call wake, offline-first messaging, call/media recording,
transcription, and SIP remain deliberately deferred.

Last aligned with the application repository: 2026-09-21.
