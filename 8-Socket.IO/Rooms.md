# Socket Rooms

## Overview

Socket.IO rooms isolate event delivery.

Rooms ensure events are only delivered to authorized users.

---

# Room Types

Organization Room

organization:{organizationId}

Conversation Room

conversation:{conversationId}

---

# Joining Rooms

Users may only join rooms for organizations they belong to.

Conversation membership must be validated before joining.

---

# Leaving Rooms

Sockets leave rooms when:

- User switches conversations
- User disconnects
- Authorization changes

---

# Multi-Connection Support

One user may have multiple socket connections.

Example

User

├── Desktop
├── Mobile
└── Browser

Each socket independently joins the required rooms.