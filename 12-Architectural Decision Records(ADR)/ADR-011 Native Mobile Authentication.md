# ADR-011: Native Mobile Authentication

- **Status:** Accepted

## Decision

Keep browser cookie endpoints unchanged and add dedicated native login, Google, refresh, and logout routes. Native Google sign-in supplies an ID token; the API verifies signature, issuer, expiry, verified email, and expected web-client audience before linking identity.

The Expo client stores rotating refresh credentials in SecureStore and access credentials in memory. Email verification/reset requests may select mobile delivery links with an HTTPS web fallback.


## Decision Trade-offs

### Chosen: Dedicated native auth endpoints with SecureStore refresh tokens

**Pros**

- Matches native credential storage and direct API networking.
- Reuses the same session rotation, revocation, throttling, and identity-linking services as web.
- Leaves browser HttpOnly cookie and CSRF protections unchanged.

**Cons**

- Adds public endpoint variants and mobile-specific contract tests.
- A compromised device process can access its own SecureStore credential after unlock.
- Google sign-in requires native OAuth clients, signing fingerprints, and rebuilt binaries.

### Alternative: Reuse browser cookie endpoints in React Native

**Pros**

- No additional auth routes.

**Cons**

- Cookie jars, same-origin proxying, and CSRF behavior do not map cleanly to a native app.
- Debugging refresh behavior across platforms becomes less predictable.

### Alternative: Trust Google profile data returned by the mobile client

**Pros**

- Avoids backend Google token verification.

**Cons**

- Allows forged identities and account takeover.
- Cannot enforce issuer, audience, expiry, or verified email.

## Consequences

Native clients receive an appropriate secure transport without weakening browser CSRF defenses or trusting client-provided Google identities. Separate endpoint contracts require explicit web/mobile regression tests.
