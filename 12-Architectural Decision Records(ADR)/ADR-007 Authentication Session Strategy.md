# ADR-007: Authentication Session Strategy

- **Status:** Accepted

## Context

InTouch serves a same-origin-proxied browser application and a native Expo client. Both need short-lived access tokens and revocable, rotating long-lived sessions, but their secure refresh transports differ.

## Decision

- Issue approximately 15-minute Bearer JWT access tokens with minimal claims.
- Persist approximately 30-day refresh sessions as hashes and rotate on every refresh.
- Browser login sets the refresh token in an HttpOnly, Secure-in-production cookie. Refresh/logout require an allowlisted Origin and CSRF header; access tokens remain in memory.
- Native mobile endpoints return the refresh token in strict response bodies. The app stores it in Expo SecureStore and sends it only to dedicated mobile refresh/logout routes.
- Password reset revokes all sessions. Google provider tokens are discarded after identity verification.


## Decision Trade-offs

### Chosen: Short-lived access JWTs with rotating stateful refresh sessions

**Pros**

- Bearer access checks remain inexpensive while refresh sessions are individually revocable.
- Rotation limits replay and password reset can revoke every device session.
- Browser HttpOnly cookies and mobile SecureStore use platform-appropriate refresh transport.

**Cons**

- Requires session persistence, rotation race handling, and serialized client refresh attempts.
- Browser refresh/logout need Origin and CSRF protection in addition to cookie settings.
- Web and native clients require separate endpoint contracts and tests.

### Alternative: Access and refresh tokens in browser local storage

**Pros**

- Simple cross-origin client implementation.

**Cons**

- JavaScript-accessible refresh credentials have greater XSS exposure.

### Alternative: Stateless long-lived JWT sessions

**Pros**

- No refresh-session database lookup or rotation state.

**Cons**

- Individual revocation and replay detection are weak without adding state back.
- A stolen long-lived token remains useful until expiry.

### Alternative: One cookie-based transport for browser and native clients

**Pros**

- One endpoint family.

**Cons**

- Native cookie behavior and CSRF assumptions are a poor fit for SecureStore-based applications.
- Direct API mobile access becomes harder to reason about and test.

## Consequences

- Refresh sessions are individually revocable and replay-resistant.
- Browser JavaScript cannot access refresh credentials.
- Native clients avoid cookie/CSRF assumptions while preserving the same rotation repository and policies.
- Client implementations must serialize refresh attempts and retry a failed protected request at most once.

LocalStorage refresh tokens, stateless refresh JWTs, and one transport forced onto both browser/native clients were rejected.
