# Web Architecture

The web client is a Next.js App Router application under `apps/web`. It
provides browser authentication, workspace administration, text collaboration,
voice-note recording/playback, Echo, search, notifications, private assets,
and LiveKit audio/video/screen sharing.

## Routing and Rendering

Public routes handle login, registration, Google OAuth return, verification,
and password recovery. Protected routes under `/app` cover workspaces,
channels, direct messages, search, notifications, invitations, profile, and
organization settings.

Server components provide route composition where useful. Interactive product
surfaces are client components backed by typed API modules and providers.

## API and Authentication Boundary

Browser REST and OAuth traffic uses the same-origin Next.js `/api/*` proxy to
the API service. This keeps the rotating refresh cookie first-party. Socket.IO,
LiveKit, and presigned R2 transfers connect directly to their configured
origins.

The browser keeps short-lived access credentials in memory. Refresh uses the
HttpOnly cookie and the required CSRF header. Protected content waits for
session restoration before deciding whether to render or redirect.

## State Ownership

- TanStack Query owns durable server state and cache reconciliation.
- `AuthProvider` owns browser session restoration and access-token lifecycle.
- `RealtimeProvider` merges Socket.IO facts into, or invalidates, query caches.
- `VoiceProvider` owns the navigation-persistent LiveKit room and ephemeral
  media state.
- Local component state owns transient dialogs, menus, drafts, and controls.

The client does not maintain a second durable message or presence store beside
TanStack Query.

## Security and Private Assets

The Next.js proxy applies a nonce-based Content Security Policy and explicit
permissions, referrer, and content-type headers. CSP origins are exact and
environment-driven for Socket.IO, LiveKit, Sentry, and R2. The exact
`NEXT_PUBLIC_R2_ORIGIN` is admitted to `media-src` so private signed voice-note
URLs can play without broad storage wildcards.

Private files use API-authorized short-lived URLs. Browser uploads go directly
to presigned R2 targets and are completed and claimed through the API. Provider
credentials and permanent object URLs are never exposed to the browser.

## Realtime and Media

Socket.IO carries presence, typing, receipts, message/reaction updates,
notification state, voice occupancy, and call lifecycle. LiveKit alone carries
WebRTC signaling and media. REST remains authoritative for durable writes and
recovery after missed realtime events.

## Voice Notes

The browser records voice notes only in a secure context, prefers Opus/WebM,
and falls back to AAC/MP4 where supported. Recording is foreground-only and is
disabled during a LiveKit session. The composer exposes pause, resume, cancel,
send, final-countdown, retry, and discard states without persisting a draft.

Playback requests a short-lived asset URL lazily, refreshes it once after an
access failure, and coordinates one active player across the client. Dedicated
bubbles provide waveform seeking and `1x`, `1.5x`, and `2x` speeds.

## Verification

From the monorepo root:

```bash
npm run typecheck --workspace @intouch/web
npm test --workspace @intouch/web
npm run build:web
```

Playwright is intentionally separate as `npm run test:web:e2e`.
