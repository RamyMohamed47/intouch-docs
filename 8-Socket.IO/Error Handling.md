# Socket Error Handling

Handshake failures expose a sanitized object through `connect_error.data`:

~~~json
{ "code": "UNAUTHORIZED", "message": "Authentication required" }
~~~

Command events use a typed acknowledgement union:

~~~ts
{ success: true }
{ success: false, error: { code, message, retryAfterMs? } }
~~~

- Refresh credentials only for `UNAUTHORIZED`.
- Back off for `TOO_MANY_REQUESTS` using `retryAfterMs` when present.
- Treat forbidden/not-found room joins as access loss and reconcile REST state.
- Never retry a failed command indefinitely.
- Disconnect/access-revocation paths clear local typing, presence, and media state.

Internal errors, stack traces, tokens, provider identities, and tenant-private details are never emitted.
