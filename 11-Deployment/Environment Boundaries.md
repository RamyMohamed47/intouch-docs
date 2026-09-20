# Environment Boundaries

Configuration is scoped by application and by whether it is safe to embed in a
client artifact.

| Scope | Examples | Rule |
| --- | --- | --- |
| API runtime | database, Redis, provider keys, token secrets | Server-only secrets in the API environment |
| Web server | `BACKEND_ORIGIN`, source-map upload credentials | Server/build environment only |
| Web public | `NEXT_PUBLIC_SOCKET_ORIGIN`, LiveKit URL, exact `NEXT_PUBLIC_R2_ORIGIN`, public Sentry DSN | Embedded in browser code; never secret |
| Mobile public | `EXPO_PUBLIC_API_URL`, Google web client ID, public Sentry DSN | Embedded in the application binary; never secret |
| EAS build-only | `SENTRY_AUTH_TOKEN`, `SENTRY_ORG`, `SENTRY_PROJECT` | Used during build and not exposed as public runtime configuration |
| EAS file/credentials | `GOOGLE_SERVICES_JSON`, signing key, FCM V1 service account | Managed by EAS; never committed |

## Local Files

- `apps/api/config.env` contains ignored local API configuration.
- `apps/web/.env.local` contains ignored local web configuration.
- `apps/mobile/.env.local` contains ignored local mobile public configuration.

Start from the committed example files and apply values manually. Never paste
real secret values into documentation, issues, logs, screenshots, or public
client-prefixed variables.

## Production Separation

API and web Railway services receive only the variables they require. EAS
build environments are configured separately for development, preview, and
production. Local Docker credentials and loopback URLs are development-only
and must not be reused in production.

`NEXT_PUBLIC_R2_ORIGIN` contains only the R2 HTTPS origin, without a bucket
path, object key, query string, or credentials. It is used to build the exact
web `media-src` CSP entry required for signed voice-note playback.
