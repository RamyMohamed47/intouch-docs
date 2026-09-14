# Reconnection

Socket.IO reconnects transport, but application state restoration remains explicit.

1. Refresh the access token if necessary.
2. Reconnect with the current token.
3. Resubscribe to the active organization and conversation.
4. Re-fetch the member roster and reconcile conversation, message, notification, reaction, and receipt query families.
5. Resume an authorized voice session through REST before reconnecting LiveKit; never infer media state from Socket.IO alone.

The server does not replay an unbounded event log. MongoDB-backed REST resources are authoritative after missed events. Typing state is ephemeral and expires; presence and voice leases are repaired by Redis/provider reconciliation.

Mobile disconnects ordinary Socket.IO activity when backgrounded except where an active voice session requires its managed background lifecycle.
