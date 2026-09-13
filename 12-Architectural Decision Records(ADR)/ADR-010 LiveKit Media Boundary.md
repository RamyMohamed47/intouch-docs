# ADR-010: LiveKit Media Boundary

- **Status:** Accepted

## Decision

Use LiveKit Cloud for encrypted WebRTC media and signaling behind a media-provider interface. InTouch remains authoritative for membership, capacity, one-session admission, call lifecycle/history, moderation, credentials, and cleanup.

Provider room and participant identities are opaque. Five-minute initial-connect tokens carry only required media grants. Signed raw-body webhooks and periodic reconciliation update Redis/MongoDB state because webhook delivery is retryable, not guaranteed.


## Decision Trade-offs

### Chosen: Managed LiveKit behind an InTouch media-provider interface

**Pros**

- Provides production WebRTC routing, TURN, reconnection, camera, and screen tracks without building an SFU.
- The provider interface keeps controllers/services independent of LiveKit-specific APIs.
- InTouch retains access policy, call history, capacity, and moderation authority.

**Cons**

- Introduces provider cost, internet dependency, webhook reconciliation, and SDK/native-build complexity.
- Application and provider participant state can temporarily diverge.
- Standard WebRTC transport encryption is not application-level E2EE.

### Alternative: Self-host a WebRTC SFU/TURN stack

**Pros**

- Maximum infrastructure control and potential cost benefits at large scale.

**Cons**

- Substantial networking, scaling, regional routing, security, and on-call burden.
- Distracts from application authorization and collaboration features.

### Alternative: Pure peer-to-peer browser signaling

**Pros**

- Minimal media-provider cost for two-person calls.

**Cons**

- Poor fit for ten-person channels, restrictive networks, moderation, and mobile reliability.
- Requires custom signaling and TURN operations anyway.

### Alternative: Expose LiveKit identities directly as application identity

**Pros**

- Less mapping and session-store code.

**Cons**

- Leaks application identifiers/PII and delegates authorization semantics to provider rooms.
- Makes revocation and provider replacement harder.

## Consequences

Audio, video, and screen sharing can evolve without implementing an SFU or exposing application identities to the provider. InTouch must reconcile out-of-order events and revoke participants after access/lifecycle changes.
