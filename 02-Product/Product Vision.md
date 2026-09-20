# Product Vision

InTouch is a multi-tenant communication platform for organizations that combines durable text collaboration, realtime presence, rich media calls, an AI assistant, and native mobile access.

## Product Goals

- Give organizations a single place for public/private channels and direct conversations.
- Keep authorization and tenant boundaries explicit at every REST, realtime, storage, AI, and media boundary.
- Make communication available on the web and Android without duplicating backend contracts.
- Preserve durable business state in MongoDB while Redis, BullMQ, LiveKit, and external providers remain replaceable infrastructure.
- Demonstrate production-minded engineering through tests, OpenAPI, ADRs, observability, health checks, and recovery paths.

## Implemented Experience

- Email/password and Google authentication for browser and native mobile clients.
- Organization creation, public joining, invitations, owner administration, categories, public/private text channels, voice channels, and direct messages.
- Text, attachment, and voice-note messages; replies, mentions, reactions, edits, redaction, typing, presence, unread counts, and read receipts.
- Durable in-app notifications, category preferences, timed/permanent mutes, Expo push, and incoming-call alerts.
- LiveKit-powered audio/video direct calls, ten-person voice channels, camera controls, screen sharing, moderation, call history, and reconnect handling.
- Echo AI summaries, action items, contextual answers, and composer transformations using consented, authorization-filtered context.
- Organization-wide message, channel, and people search.
- Private R2-backed attachments, avatars, and organization logos.
- Next.js web and Expo Android clients sharing strict Zod contracts.

## Deliberately Deferred

- Public iOS acceptance, CallKit, Android Telecom, lock-screen call actions, and guaranteed offline VoIP wake.
- Offline-first message storage, queued sends, and background synchronization.
- Call/media recording, transcription, SIP, end-to-end media encryption, and device-audio screen sharing.
- Threads, custom organization roles, bots, workflow automation, and organization analytics.

The current product is beyond its original MVP. Future work should deepen reliability and native platform integration rather than reclassifying already shipped capabilities as future ideas.
