# API Versioning

The public HTTP base path is `/api/v1`. URL versioning keeps the contract explicit in clients, logs, proxies, and OpenAPI.

## Compatibility

A new major path is required for incompatible changes such as removing operations/required fields, changing authentication transport, or altering response semantics. Backward-compatible additions may remain in v1 when they add optional request fields, new response fields that strict clients are prepared to ignore, new resources, or new error conditions.

Because InTouch clients parse strict Zod schemas, even nominally additive response changes must update web/mobile clients and shared contracts in the same repository change. API version policy does not replace coordinated contract testing.

Deprecated operations should be marked in OpenAPI, given a replacement, and retained for a documented migration window. No `/api/v2` contract currently exists.
