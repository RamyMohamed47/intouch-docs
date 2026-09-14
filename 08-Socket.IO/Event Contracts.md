# Event Contracts

All handshake, payload, acknowledgement, client-map, and server-map schemas live in `@intouch/shared/realtime`. TypeScript event maps are inferred from Zod rather than handwritten independently.

## Client Events (7)

`conversation:join`, `conversation:leave`, `organization:subscribe`, `organization:unsubscribe`, `typing:start`, `typing:stop`, and `voice:heartbeat`.

## Server Events (16)

`message:created`, `message:updated`, `message:deleted`, `membership:joined`, `conversation:access-revoked`, `presence:updated`, `typing:updated`, `read-receipt:updated`, `conversation:activity`, `channel-read-receipts:changed`, `message-reactions:changed`, `notification:changed`, `call:incoming`, `call:updated`, `screen-share:stop-requested`, and `voice-channel:occupancy-updated`.

The API parses outbound payloads before emission, which serializes dates and rejects accidental persistence/provider fields. See [[08-Socket.IO/Socket Events|Socket Events]] for exact payloads and delivery rules.
