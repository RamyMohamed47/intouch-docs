
## Status

Accepted

---

## Context

The application supports two forms of communication:

- Channels
- Direct Messages (DMs)

An initial design considered separate entities for Channels and Chats.

This introduced unnecessary duplication because both represent conversations that contain messages.

---

## Decision

InTouch will model all communication using a single `Conversation` entity.

A conversation may represent either:

- CHANNEL
- DM

The conversation type is determined by the `type` field.

---

## Conversation Types

### CHANNEL

Represents a channel inside an organization.

Examples:

- #general
- #backend
- #announcements

Channels may optionally belong to a Category.

---

### DM

Represents a private conversation between members of the same organization.

DMs do not belong to a Category.

Users may only create DMs with other members of their current organization.

---

## Architecture

```text
Organization
        │
        │
 Category (optional)
        │
        │
 Conversation
    │         │
CHANNEL      DM
        │
        │
     Message
```

---

## Benefits

Using a single Conversation entity:

- Eliminates duplicated collections.
- Simplifies querying.
- Reduces application logic.
- Provides a consistent model for messaging.
- Makes future conversation types easy to introduce.

---

## Query Examples

Retrieve all conversations for an organization:

```javascript
Conversation.find({
    organizationId: organizationId
})
```

Retrieve all messages for a conversation:

```javascript
Message.find({
    conversationId: conversationId
})
```

---

## Alternatives Considered

### Separate Channel and Chat Collections

Pros

- Clear distinction between concepts.

Cons

- Duplicate business logic.
- Duplicate message handling.
- Additional service layer complexity.
- Harder to maintain.

---

## Future Considerations

Additional conversation types may be introduced without changing the overall architecture.

Examples:

- Announcement
- Support Ticket
- AI Conversation

These can be represented by extending the `type` field rather than creating new collections.

---

## Consequences

### Advantages

- Simpler data model
- Easier maintenance
- Unified messaging pipeline
- Better extensibility

### Trade-offs

- Some fields are only relevant to specific conversation types.
- Application logic must validate behavior based on the conversation type.