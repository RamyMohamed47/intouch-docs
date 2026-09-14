# Presence

Presence is organization-scoped and represents whether a user has at least one valid application socket lease.

- Initial state comes from `GET /organizations/{organizationId}/members`.
- Incremental changes arrive as `presence:updated` only in shared, subscribed organization rooms.
- Multiple browser/mobile sockets count as one user presence.
- Clients renew leases every 15 seconds. Redis socket leases expire after 45 seconds if a replica dies.
- A final disconnect uses a five-second grace period before persisting and broadcasting offline state.
- Online updates use `lastSeenAt: null`; confirmed offline updates include the persisted timestamp.

Presence is not a security signal. Every protected action still verifies authorization. Production requires Redis so replicas share presence and claim expiry transitions atomically.
