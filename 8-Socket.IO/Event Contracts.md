# Socket Event Contract

## Event Naming Convention

Events follow a namespaced format.

Examples:

- auth:authenticate
- conversation:join
- conversation:leave
- message:send
- message:received
- typing:start
- typing:stop
- presence:update

This improves readability and scalability.

---

# Event Directions

## Client → Server

- auth:authenticate
- conversation:join
- conversation:leave
- message:send
- message:edit
- message:delete
- typing:start
- typing:stop

---

## Server → Client

- message:received
- message:edited
- message:deleted
- typing:update
- presence:update
- error

---

# Acknowledgements

Command events should return acknowledgements.

Example

Client

message:send

↓

Server

{
    success: true,
    messageId: "..."
}

---

# Event Philosophy

Client emits intentions.

Server emits facts.

Example

Client:

message:send

Server:

message:received

The client never broadcasts directly to other clients.