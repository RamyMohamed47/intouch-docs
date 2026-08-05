# ADR-007: Authentication Session Strategy

- **Status:** Accepted
- **Date:** 2026-07-28

---

# Context

InTouch requires a secure, scalable authentication mechanism suitable for a modern single-page application (SPA).

The authentication strategy must:

- Support stateless access to protected resources.
- Minimize the impact of token theft.
- Allow long-lived user sessions without requiring frequent logins.
- Support future multi-device session management.
- Be compatible with browser security policies.
- Integrate cleanly with the existing layered architecture.

Several approaches were considered, including storing both access and refresh tokens in browser storage, using stateless JWTs only, and using cookie-based refresh tokens.

---

# Decision

InTouch adopts a hybrid token-based authentication strategy.

## Access Token

The access token is:

- JSON Web Token (JWT)
- Short-lived (approximately 15 minutes)
- Returned in the response body
- Sent using the `Authorization: Bearer <token>` header
- Intended to be stored in client memory only

The access token contains only the minimum required claims, primarily the user identifier (`sub`).

---

## Refresh Token

The refresh token is:

- Long-lived (approximately 30 days)
- Stored as an HttpOnly cookie
- Rotated after every successful refresh request
- Never exposed to client-side JavaScript

Recommended cookie configuration:

- HttpOnly
- Secure (production)
- SameSite=Lax (development)
- SameSite=None (cross-site production deployments)
- Path=/api/v1/auth

---

## Session Storage

Refresh tokens are stateful.

Each authenticated session is stored in the database.

Each session records information such as:

- User ID
- Hashed refresh token
- Expiration time
- Device information (future)
- IP address (future)
- Last activity timestamp

Refresh tokens are hashed before persistence.

---

## Refresh Flow

Authentication flow:

```
Login/Register

↓

Generate Access Token

↓

Generate Refresh Token

↓

Store Refresh Session

↓

Set HttpOnly Cookie

↓

Return Access Token
```

Refresh flow:

```
Client

↓

POST /auth/refresh

↓

Browser sends cookie

↓

Validate session

↓

Rotate refresh token

↓

Update session

↓

Return new access token

↓

Replace refresh cookie
```

Logout flow:

```
Client

↓

POST /auth/logout

↓

Invalidate session

↓

Clear refresh cookie
```

---

# Deployment Strategy

Production deployments should avoid relying on unrestricted third-party cookies.

The preferred deployment model is:

```
Browser

↓

Frontend

↓

API Proxy

↓

Backend API
```

This allows authentication cookies to behave as same-origin cookies from the browser's perspective while keeping the backend independently deployable.

---

# Consequences

## Advantages

- Improved protection against XSS attacks through HttpOnly refresh cookies.
- Short-lived access tokens reduce the impact of token leakage.
- Refresh token rotation limits replay attacks.
- Supports session revocation.
- Supports future "Log out from all devices" functionality.
- Supports future active session management.
- Compatible with modern browser security practices.
- Separates authentication from authorization concerns.

## Trade-offs

- Requires session persistence.
- Slightly more complex than purely stateless JWT authentication.
- Requires cookie configuration for production deployments.
- Adds refresh token lifecycle management.

These trade-offs are acceptable given the project's objective of demonstrating production-ready backend engineering practices.

---

# Alternatives Considered

## Access and Refresh Tokens in Local Storage

Rejected because JavaScript can access stored tokens, increasing exposure to XSS attacks.

---

## Stateless Refresh Tokens

Rejected because they cannot be individually revoked and make session management significantly more difficult.

---

## Session-Based Authentication

Rejected because it introduces server-side session management for every authenticated request and is less aligned with modern SPA architectures.

---

# Implementation Notes

- Authentication endpoints:
  - `POST /auth/register`
  - `POST /auth/login`
  - `POST /auth/refresh`
  - `POST /auth/logout`
  - `GET /auth/me`

- Access tokens are transmitted using the Authorization header.

- Refresh tokens are transmitted exclusively through secure HttpOnly cookies.

- Business logic resides in the Authentication Service.

- Session persistence is handled by the Authentication Repository.

- Authentication middleware validates access tokens only.

---

# References

- ADR-006: TypeScript Adoption
- OpenAPI Authentication Specification
- Shared Authentication Contracts