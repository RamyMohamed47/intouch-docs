# Frontend

This section documents the Next.js browser client, its server/client boundary,
state ownership, security headers, and realtime/media integration.

## Contents

- [[06-Frontend/Web Architecture|Web Architecture]]
- [[07-API Contract/Readme|REST API Contract]]
- [[08-Socket.IO/Readme|Socket.IO]]
- [[14-Mobile/Readme|Mobile Client]]

The browser implementation lives under `apps/web`. It consumes strict public
contracts from `@intouch/shared` rather than importing backend internals.
