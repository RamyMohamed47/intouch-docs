# Socket Rooms

| Room | Admission | Purpose |
| --- | --- | --- |
| `user:<userId>` | Automatic after JWT handshake | Recipient-specific notifications, call events, and inactive-conversation activity. |
| `organization:<organizationId>` | Explicit subscribe plus current membership | Presence, membership, public-channel activity, and authorized workspace invalidation. |
| `conversation:<conversationId>` | Explicit join plus current channel/DM access | Message facts, typing, receipts, reactions, and active-conversation lifecycle. |

Private-channel and direct-message delivery additionally verifies participant access. Actor exclusion is user-aware across all of an actor's sockets where required, not merely the socket that initiated an action.

Rooms are transport routing constructs, not authorization caches or durable subscriptions. Clients rejoin them after reconnect; the server rechecks policy each time.
