# Socket Event Types

## Client Intentions

The seven client events request room subscription, typing state, or a voice lease heartbeat. Each command validates a strict payload and returns the shared acknowledgement union.

## Server Facts

The sixteen server events announce committed message/call state, scoped activity, presence/typing, access revocation, receipts/reactions, durable notification changes, screen-share moderation, or voice occupancy.

Messages are not sent through a `message:send` event. The client creates them through REST and receives `message:created` after commit.

`message:created` and `message:updated` use the same strict `MessageDto` for
text, attachment, voice-note, and call timeline entries. Voice-note events
carry only opaque asset identity, canonical duration, and normalized waveform
metadata; they never expose object keys or signed URLs.

## Source of Truth

`packages/shared/realtime` exports handshake schemas, acknowledgements, and typed client/server event maps. [[08-Socket.IO/Socket Events|Socket Events]] documents delivery and privacy rules.
