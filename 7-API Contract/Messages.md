# Messages API

## Types and Creation

Message types are `TEXT`, `ATTACHMENT`, and immutable `CALL`. Clients create text/attachment messages through `POST /conversations/{conversationId}/messages` with optional content, up to five completed upload IDs, a same-conversation reply target, and validated mention ranges.

- Content is 1 to 4,000 characters when supplied.
- Mention ranges are ordered, non-overlapping UTF-16 offsets and must match an authorized conversation member.
- Replies are immutable after creation and hydrate a bounded safe preview without N+1 lookups.
- Call messages are created by call lifecycle services and cannot be edited, deleted, reacted to, or carry attachments.

## History and Context

- History uses `before=<messageId>&limit=1..100` and returns `nextCursor`.
- Exact search navigation uses `GET /conversations/{conversationId}/messages/{messageId}/context`.
- Edits use `PATCH /messages/{messageId}`; deletion uses `DELETE` and returns a redacted tombstone through realtime updates.

## Reactions and Receipts

- `GET /messages/{messageId}/reactions` returns personalized aggregates.
- `PUT/DELETE /messages/{messageId}/reactions/me` sets/replaces/removes one normalized emoji reaction.
- Reactor identities are cursor-paginated and authorization-filtered.
- Conversation read state advances monotonically. Direct peers receive read updates; channel reader identities are available only to the message sender through the bounded readers endpoint.

Durable writes are REST-only. Socket.IO emits post-commit message facts and reaction/receipt invalidations.
