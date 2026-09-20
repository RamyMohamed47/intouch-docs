# Roadmap

## Implemented Milestones

### Platform Foundation

- npm workspaces, strict NodeNext TypeScript, shared Zod contracts, Express API, Next.js web, and MongoDB repositories.
- Password/Google authentication, rotating sessions, email verification/reset, organizations, invitations, categories, channels, direct messages, and Socket.IO realtime behavior.

### Collaboration and Infrastructure

- Private R2 assets, attachments, avatars, logos, wallpapers, reactions, read receipts, durable notifications, and transactional mail outbox.
- Dockerized local MongoDB replica set, Redis, Mailpit, and optional Grafana LGTM.
- Redis runtime state, BullMQ jobs, structured logging, OpenTelemetry/Grafana, Sentry, readiness, and recovery/reconciliation paths.

### Web V3 Media and Intelligence

- LiveKit voice channels and audio/video direct calls.
- Camera, screen sharing, fullscreen viewing, moderation, call history, tones, and browser call notifications.
- Echo AI assistant, organization search, replies, mentions, notification preferences, and scoped mutes.
- Dedicated web and Android voice notes with private R2 storage, verified duration/codec metadata, waveform seeking, and variable-speed playback.

### Mobile V1 to V2

- Expo foundation, native authentication, workspace administration, text chat, attachments, themes, wallpapers, realtime state, and Google sign-in.
- Echo, search, replies, mentions, notification inbox/preferences/mutes, Expo push, and mobile Sentry.
- Android voice channels, direct audio/video calls, camera, screen sharing, background audio, call alerts, and media moderation.

## Next Priorities

1. Mobile V2 hardening on multiple Android devices and adverse networks.
2. iOS runtime acceptance and a decision on CallKit; evaluate Android Telecom and lock-screen actions separately.
3. Load and failure testing for Socket.IO, Redis admission, BullMQ recovery, search, push, AI quotas, and call lifecycle.
4. Deployment runbooks, alert thresholds, restore drills, and cost/capacity baselines.
5. Accessibility and UX polish across web and mobile.
6. Adding github actions CI/CD pipeline

## Later Candidates

- Offline-first mobile cache and queued sends.
- Threads, custom roles, bots, automation, and organization analytics.
- Call/media recording, transcription, SIP, E2EE, and device-audio screen sharing after privacy and cost design.

Local Docker remains infrastructure-only: the API, web app, shared watcher, and mobile Metro process run natively.
