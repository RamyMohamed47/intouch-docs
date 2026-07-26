
## Overview

This document defines the payloads exchanged between clients and the server over Socket.IO.

Each event specifies:

- Direction
- Payload
- Acknowledgement (if applicable)
- Broadcast behavior
- Notes

---
## Source of Truth

The payloads documented in this file represent the conceptual contract.

During implementation, each payload will be defined as a shared Zod schema and its TypeScript type inferred automatically.

___
# Authentication

## auth:authenticate

Direction

Client → Server

Payload

```json
{
  "accessToken": "<jwt>"
}
```

Acknowledgement

```json
{
  "success": true
}
```

Error

```json
{
  "success": false,
  "error": {
    "code": "INVALID_TOKEN",
    "message": "Authentication failed."
  }
}
```

Broadcast

None

Notes

Must be called immediately after establishing the socket connection.

---

# Conversations

## conversation:join

Direction

Client → Server

Payload

```json
{
  "conversationId": "conv_123"
}
```

Acknowledgement

```json
{
  "success": true
}
```

Broadcast

None

Notes

The server verifies that the authenticated user belongs to the conversation before joining the room.

---

## conversation:leave

Direction

Client → Server

Payload

```json
{
  "conversationId": "conv_123"
}
```

Acknowledgement

```json
{
  "success": true
}
```

Broadcast

None

---

# Messages

## message:send

Direction

Client → Server

Payload

```json
{
  "conversationId": "conv_123",
  "content": "Hello everyone!"
}
```

Acknowledgement

```json
{
  "success": true,
  "messageId": "msg_456"
}
```

Broadcast

message:received

Notes

The sender receives an acknowledgement.

All participants receive a message:received event.

---

## message:received

Direction

Server → Client

Payload

```json
{
  "id": "msg_456",
  "conversationId": "conv_123",
  "sender": {
    "id": "user_001",
    "username": "Ramy"
  },
  "content": "Hello everyone!",
  "createdAt": "2026-07-26T12:00:00Z"
}
```

Broadcast

Conversation Room

---

## message:edit

Direction

Client → Server

Payload

```json
{
  "messageId": "msg_456",
  "content": "Updated message"
}
```

Acknowledgement

```json
{
  "success": true
}
```

Broadcast

message:edited

---

## message:edited

Direction

Server → Client

Payload

```json
{
  "messageId": "msg_456",
  "content": "Updated message",
  "editedAt": "2026-07-26T12:05:00Z"
}
```

Broadcast

Conversation Room

---

## message:delete

Direction

Client → Server

Payload

```json
{
  "messageId": "msg_456"
}
```

Acknowledgement

```json
{
  "success": true
}
```

Broadcast

message:deleted

---

## message:deleted

Direction

Server → Client

Payload

```json
{
  "messageId": "msg_456"
}
```

Broadcast

Conversation Room

---

# Typing

## typing:start

Direction

Client → Server

Payload

```json
{
  "conversationId": "conv_123"
}
```

Acknowledgement

None

Broadcast

typing:update

---

## typing:stop

Direction

Client → Server

Payload

```json
{
  "conversationId": "conv_123"
}
```

Acknowledgement

None

Broadcast

typing:update

---

## typing:update

Direction

Server → Client

Payload

```json
{
  "conversationId": "conv_123",
  "userId": "user_001",
  "username": "Ramy",
  "isTyping": true
}
```

Broadcast

Conversation Room

---

# Presence

## presence:update

Direction

Server → Client

Payload

```json
{
  "userId": "user_001",
  "status": "ONLINE"
}
```

Broadcast

Organization Room

Notes

Emitted whenever a user's presence changes.

---

# Errors

## error

Direction

Server → Client

Payload

```json
{
  "code": "CONVERSATION_NOT_FOUND",
  "message": "Conversation not found."
}
```

Broadcast

None

---

# Event Naming Convention

Domain:Action

Examples

- auth:authenticate
- conversation:join
- conversation:leave
- message:send
- message:received
- message:edit
- message:edited
- message:delete
- message:deleted
- typing:start
- typing:stop
- typing:update
- presence:update

---

# Design Principles

- Client events represent intentions.
- Server events represent facts.
- Commands should return acknowledgements.
- Broadcasts should target the smallest possible room.
- Payloads should contain only the data required by clients.
- Every payload should have a stable contract.