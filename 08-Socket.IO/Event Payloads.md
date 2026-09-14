# Event Payloads

The exact schemas are exported by `@intouch/shared/realtime`; this page is an orientation, not a second contract source.

| Event family | Payload purpose |
| --- | --- |
| Room commands | Organization/conversation identifier plus typed acknowledgement. |
| Message facts | Non-personalized message core DTO or redacted tombstone after commit. |
| Presence/typing | User and scope identifiers with bounded status fields and timestamps. |
| Activity | Safe conversation summary invalidation for authorized recipients. |
| Reactions/receipts | Anonymous or recipient-safe invalidation; REST returns personalized details. |
| Notifications | Durable notification upsert/delete/read-all fact for the recipient user room. |
| Calls | Strict call DTO lifecycle and targeted incoming-call state. |
| Voice occupancy | Authorized ten-person occupancy snapshot with safe user IDs. |
| Screen moderation | Targeted request to stop one current screen share. |

Messages, reactions, receipts, calls, and notification mutations are initiated through REST. No Socket.IO payload contains access/refresh tokens, LiveKit credentials, R2 keys, presigned URLs, message search indexes, or Echo content.
