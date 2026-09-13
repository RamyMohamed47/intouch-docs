# ADR-003: Unified Conversation Model

- **Status:** Accepted

## Context

Channels and one-to-one direct messages share activity, messages, read state, assets, search, notifications, and realtime behavior. Separate persistence models would duplicate these rules. Voice channels need different capabilities from text channels without becoming a separate tenant model.

## Decision

Use one `Conversation` collection with:

- `type: CHANNEL | DIRECT`.
- For channels, immutable `kind: TEXT | VOICE`, category, position, and `PUBLIC | PRIVATE` visibility.
- For direct messages, a stable organization-scoped participant pair.
- `ConversationParticipant` for explicit private-channel and direct-message access.

Text message endpoints reject voice-only conversations. Voice-channel DTOs expose occupancy/capacity rather than message summaries. DM calls attach immutable `CALL` messages to the same direct timeline.


## Decision Trade-offs

### Chosen: One discriminated Conversation model

**Pros**

- Channels and direct messages share authorization, activity, unread, search, and lifecycle logic.
- Text, voice, and call history can reuse consistent organization and participant relationships.
- Adding channel capabilities does not require duplicating whole persistence pipelines.

**Cons**

- Some fields and operations are valid only for particular type/kind branches.
- Services and schemas must reject invalid combinations such as message history on a voice-only channel.
- Indexes can contain sparse or discriminator-specific fields.

### Alternative: Separate Channel, DirectChat, and VoiceRoom collections

**Pros**

- Each collection can expose a narrower schema.
- Type-specific queries may appear simpler in isolation.

**Cons**

- Duplicates membership, participants, activity, deletion, search, and authorization rules.
- Cross-conversation lists and shared message/call history require extra orchestration.

## Consequences

- Shared authorization and lifecycle services avoid duplicated channel/chat logic.
- Queries remain organization-scoped and discriminated by type/kind.
- Some fields apply only to a discriminator branch and must be enforced by schemas/services.
- Channel kind cannot be changed after creation because it changes the supported domain behavior.

## Rejected Alternative

Separate Channel, DirectChat, and VoiceRoom collections were rejected because they would duplicate participation, activity, deletion, and authorization logic.
