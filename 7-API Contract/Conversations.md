# Conversations API

## Model

- `type: CHANNEL | DIRECT`.
- Channel `kind: TEXT | VOICE` is immutable after creation.
- Channel `visibility: PUBLIC | PRIVATE`; private channels require explicit participants.
- Direct messages are unique one-to-one conversations within an organization.

## Channel and DM Routes

- List/create channels: `/organizations/{organizationId}/conversations`.
- List/create-or-get DMs: `/organizations/{organizationId}/direct-messages`.
- Read/update/delete a conversation: `/conversations/{conversationId}`.
- Private participant administration: `/conversations/{conversationId}/participants`.
- Per-user wallpaper resolution/override: `/conversations/{conversationId}/chat-wallpaper`.
- Durable high-water receipt: `PUT /conversations/{conversationId}/read-receipt`.

Text-channel and direct-message DTOs expose message summaries, unread state, and receipts. Voice-channel DTOs expose authorized occupancy and capacity but do not expose message history/unread fields.

Text message endpoints reject voice-only channels. Voice join/call operations are documented under the `Voice` tag in [[7-API Contract/API Reference|API Reference]].
