# Socket Event Types

## Client Intentions

The seven client events request room subscription, typing state, or a voice lease heartbeat. Each command validates a strict payload and returns the shared acknowledgement union.

## Server Facts

The sixteen server events announce committed message/call state, scoped activity, presence/typing, access revocation, receipts/reactions, durable notification changes, screen-share moderation, or voice occupancy.

Messages are not sent through a `message:send` event. The client creates them through REST and receives `message:created` after commit.

## Source of Truth

`packages/shared/realtime` exports handshake schemas, acknowledgements, and typed client/server event maps. [[8-Socket.IO/Socket Events|Socket Events]] documents delivery and privacy rules.
