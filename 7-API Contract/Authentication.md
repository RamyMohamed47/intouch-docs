
## Register

POST /api/v1/auth/register

Creates a new user account.

---

## Login

POST /api/v1/auth/login

Authenticates an existing user.

Returns:

- Access Token
- Refresh Token

---

## Google Login

GET /api/v1/auth/google

Starts the OAuth flow.

---

## Google Callback

GET /api/v1/auth/google/callback

OAuth callback endpoint.

---

## Refresh Token

POST /api/v1/auth/refresh

Returns a new access token.

---

## Logout

POST /api/v1/auth/logout

Invalidates the current refresh token.

---

## Current User

GET /api/v1/auth/me

Returns the authenticated user's profile.

---

# Authentication Rules

- Email is globally unique.
- JWT protects API endpoints.
- Refresh Tokens issue new Access Tokens.
- OAuth accounts are linked by email.