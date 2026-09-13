# ADR-004: Verified Identity Linking

- **Status:** Accepted

## Context

One InTouch user may authenticate with password and Google. Provider email/profile data can change, while the provider subject is the stable identity key.

## Decision

- Store provider identities in `LoginProvider`, keyed by provider plus verified subject.
- Link Google to an existing user only through a provider-verified email address.
- Browser authentication uses the backend-owned authorization-code flow; native mobile supplies an ID token that the backend verifies for signature, issuer, expiry, audience, and verified email.
- Discard Google access/refresh tokens after verification. InTouch continues to issue its own access and rotating refresh sessions.
- Refresh the external Google avatar fallback on successful Google authentication unless the user has selected a private uploaded avatar; never proxy or permanently copy provider image bytes automatically.


## Decision Trade-offs

### Chosen: Verified provider subjects linked to one InTouch user

**Pros**

- Password and Google login converge on one authorization identity and session model.
- The provider subject remains stable when profile data changes.
- New providers can be represented without provider-specific top-level User fields.

**Cons**

- Linking requires careful verified-email and provider-subject conflict handling.
- Provider profile refresh rules must respect user-uploaded avatar precedence.
- Embedded provider records need compound multikey indexes and atomic updates.

### Alternative: Trust client-supplied provider identity

**Pros**

- Very little backend integration code.

**Cons**

- Allows identity spoofing and account takeover.
- Cannot prove issuer, audience, expiry, or email verification.

### Alternative: Create a separate user for every login method

**Pros**

- Avoids account-linking conflict logic.

**Cons**

- One person receives fragmented memberships, conversations, and notifications.
- Later account merging becomes risky and difficult.

### Alternative: Store Google access and refresh tokens

**Pros**

- Would enable future Google API calls without reauthorization.

**Cons**

- Adds credential storage, rotation, revocation, and breach exposure with no current product need.
- Couples InTouch sessions to Google token lifecycle.

## Consequences

- Email alone is never trusted from a client payload.
- Provider subject changes cannot silently take over another user.
- Password and Google login converge on the same InTouch user/session/authorization model.
- Additional providers can be added without adding provider-specific fields to `User`.

Storing provider tokens, trusting client-supplied Google user IDs, and maintaining separate users per login method were rejected.
