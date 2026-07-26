# Socket Event Types

## Client → Server

Client events represent intentions.

Examples

- message:send
- typing:start
- conversation:join

---

## Server → Client

Server events represent facts.

Examples

- message:received
- typing:update
- presence:update

---

## Payload Definition

Every event payload originates from a Zod schema.

Example

MessageSendSchema

↓

MessageSendPayload

↓

Socket Event

---

## Acknowledgements

Command events should return typed acknowledgements.

Example

AckSuccess

AckFailure