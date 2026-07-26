
## Send Message

POST /conversations/:conversationId/messages

---

## Get Messages

GET /conversations/:conversationId/messages

Supports pagination.

Example:

?page=1&limit=50

---

## Edit Message

PATCH /messages/:messageId

---

## Delete Message

DELETE /messages/:messageId

---

# Message Types

- TEXT
- IMAGE
- FILE
- VOICE (Future)
- SYSTEM

---

# Notes

Messages belong to Conversations.

Messages are returned in chronological order.

Soft deletion may be implemented in a future release.