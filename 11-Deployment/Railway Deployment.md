# Railway Deployment

Railway runs separate API and Next.js services from the same monorepo. They are
independently built and released even though source and shared contracts are
versioned together.

## API Service

- Build command: `npm run build:api`
- Start command: `npm run start:api`
- Health-check path: `/ready`
- Liveness path: `/health`

The service requires a transaction-capable MongoDB deployment, Redis when
distributed runtime state and BullMQ are enabled, and the production provider
configuration selected for mail, R2, search, LiveKit, Echo, push, and optional
observability.

`/ready` includes MongoDB, Redis runtime state, and background jobs. Optional
Grafana, Sentry, and third-party delivery providers are excluded so a telemetry
outage cannot remove a healthy application replica from service.

## Web Service

- Build command: `npm run build:web`
- Start command: `npm run start:web`

`BACKEND_ORIGIN` is server-only and targets the Railway API from the same-origin
REST/OAuth proxy. Browser-visible origins for Socket.IO, LiveKit, R2, and Sentry
use narrowly scoped `NEXT_PUBLIC_*` configuration.

## External Production Services

- MongoDB Atlas owns durable production data.
- Managed Redis coordinates distributed runtime state and BullMQ.
- Cloudflare R2 stores private assets.
- LiveKit Cloud transports realtime media.
- Gemini serves Echo through the API-owned provider adapter.
- Brevo or an explicitly configured SMTP provider sends transactional email.
- Expo Push and FCM V1 deliver Android notifications.
- Grafana Cloud and Sentry receive sanitized telemetry.

## Deployment Rules

- Never copy local `config.env` or `.env.local` files into Railway.
- Store production secrets in the correct Railway service environment.
- Keep web and API Sentry projects and DSNs separate.
- Point the LiveKit webhook directly at the public API integration route.
- Run one-time migrations explicitly after the compatible application release;
  do not make destructive migrations an automatic startup side effect.
- Treat application deployment success and provider delivery health as
  separate checks.

## Verification

After deployment:

1. Confirm API `/health` and `/ready` return `200`.
2. Confirm the web service can proxy an API request and restore a browser
   session.
3. Exercise a database transaction, Redis-backed realtime behavior, and one
   BullMQ-delivered operation.
4. Verify private asset upload/read, transactional email, Echo, and LiveKit only
   when those providers changed.
5. Confirm Grafana receives telemetry and Sentry release/source-map processing
   succeeds without exposing request content.
