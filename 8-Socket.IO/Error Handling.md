# Socket Error Handling

## Philosophy

Errors should be predictable and consistent.

---

# Error Event

Server emits:

error

Example

{
    code: "CONVERSATION_NOT_FOUND",
    message: "Conversation not found."
}

---

# Common Errors

Authentication

- INVALID_TOKEN
- TOKEN_EXPIRED

Authorization

- ACCESS_DENIED

Conversation

- CONVERSATION_NOT_FOUND

Organization

- ORGANIZATION_NOT_FOUND

Messaging

- MESSAGE_NOT_FOUND

Validation

- INVALID_PAYLOAD

---

# Error Principles

- Never expose internal implementation details.
- Always return machine-readable error codes.
- Provide human-readable messages.