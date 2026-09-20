# Project Progress

## Implemented

- **Foundation:** strict TypeScript npm monorepo, layered API, repositories, MongoDB transactions, shared Zod contracts, OpenAPI, and automated checks.
- **Identity:** password verification/reset, Google browser/native auth, rotating sessions, rate limiting, and transactional mail.
- **Tenancy:** organizations, memberships, invitations, categories, public/private text and voice channels, private participants, and direct messages.
- **Messaging:** text/private attachments, cross-platform voice notes, replies, mentions, reactions, edits, redaction, typing, presence, activity, receipts, unread counts, search, and wallpapers.
- **Notifications:** durable inbox, category preferences, timed/permanent mutes, Expo push, badge reconciliation, and incoming-call alerts.
- **Media:** LiveKit audio/video calls, ten-person voice channels, camera, screen sharing, fullscreen viewing, moderation, call history, reconnect/reconciliation, and Android background audio.
- **Echo:** consented workspace/conversation Q&A, summaries, action items, composer tools, quotas, streaming, and authorized source references.
- **Clients:** production-minded Next.js web and Android-first Expo mobile through Mobile V2.0.
- **Infrastructure:** local Compose services, Redis/BullMQ, R2, health/readiness, OpenTelemetry/Grafana, Sentry, and provider-neutral adapters.

## Current Hardening Work

- Cross-device Android call/push/media testing and adverse-network recovery.
- Load/failure testing and operational threshold tuning.
- Accessibility, UX polish, and deployment/restore runbooks.

## Deferred

- Public iOS acceptance and native OS calling frameworks.
- Offline-first persistence and queued sends.
- Threads, custom roles, bots/automation, analytics, call/media recording, transcription, SIP, and E2EE.

For phase-specific acceptance and limitations, see [[14-Mobile/Mobile V1|Mobile V1]], [[14-Mobile/Mobile V1.2|Mobile V1.2]], and [[14-Mobile/Mobile V2.0|Mobile V2.0]].
