# Messages API

## Types and Creation

Message types are `TEXT`, `ATTACHMENT`, `VOICE_NOTE`, and immutable `CALL`. Clients create normal or voice-note messages through `POST /conversations/{conversationId}/messages`.

Normal messages accept optional content, up to five completed ordinary upload IDs, a same-conversation reply target, and validated mention ranges. Voice notes accept exactly one promoted `VOICE_NOTE` upload ID and an optional same-conversation reply target.

- Content is 1 to 4,000 characters when supplied.
- Mention ranges are ordered, non-overlapping UTF-16 offsets and must match an authorized conversation member.
- Replies are immutable after creation and hydrate a bounded safe preview without N+1 lookups.
- Voice-note uploads are conversation-scoped, last from 1,000 to 300,000 milliseconds, are limited to 5 MB, and carry exactly 64 integer waveform peaks from 0 to 100.
- Accepted audio is AAC in M4A/MP4 or Opus in WebM. The API verifies container signature, MIME, codec, absence of video, size, and canonical duration before promotion.
- Voice notes cannot contain content, mentions, ordinary attachments, or edits. They retain replies, reactions, receipts, unread state, notifications, and authorized redaction.
- Message DTOs expose a nullable `voiceNote` object with opaque `assetId`, canonical `durationMs`, and waveform peaks. The audio asset is excluded from generic `attachments`.
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
