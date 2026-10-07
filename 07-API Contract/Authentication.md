# Authentication API

## Browser Flow

- `POST /auth/register` creates a pending password account and queues verification mail; it does not create a session.
- `POST /auth/verify-email`, `/resend-verification`, `/forgot-password`, and `/reset-password` manage single-use actions.
- `POST /auth/login` returns `{ user, accessToken }` and sets a rotating refresh token in an HttpOnly cookie.
- `POST /auth/refresh` rotates the cookie and returns a new access token. Refresh/logout require the allowlisted Origin and CSRF header.
- `GET /auth/oauth/google` and `GET /auth/oauth/google/callback` implement backend-owned authorization-code Google OAuth.
- `GET /auth/me` requires a Bearer access token.

## Native Mobile Flow

- `POST /auth/mobile/login` accepts the password login contract.
- `POST /auth/mobile/google` accepts a Google ID token that the API verifies for signature, issuer, expiry, email verification, and audience.
- `POST /auth/mobile/refresh` and `POST /auth/mobile/logout` accept the rotating refresh token in the request body.
- Mobile auth responses return both access and refresh credentials; the app stores only the refresh token in SecureStore.

## Security Invariants

- Access JWTs are approximately 15 minutes and contain minimal claims.
- Refresh sessions are approximately 30 days, stored as hashes, rotated on use, and revocable.
- The immediately previous refresh token is accepted for 30 seconds after rotation so a lost response can be retried; later reuse revokes the session.
- Password reset revokes all refresh sessions.
- Google provider tokens are discarded after verification.
- Google sign-in for an email that has a still-pending password account links Google and removes the unverified password.
- Login and email-action endpoints are rate limited; production counters are shared through Redis.
- Password login is limited to ten attempts per account and client address in fifteen minutes, with a higher account-wide ceiling across all addresses.
- Requests through the web proxy are rate limited by the browser address, which the proxy forwards together with a shared secret.
