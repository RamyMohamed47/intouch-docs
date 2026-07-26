
## Overview

InTouch uses Socket.IO to provide real-time communication between clients and the server.

Socket.IO complements the REST API by handling real-time events such as messaging, typing indicators, and presence updates.

REST remains responsible for request-response operations, while Socket.IO is responsible for live synchronization.

---

# Design Principles

- REST handles CRUD operations.
- Socket.IO handles real-time events.
- Business logic is shared between REST controllers and Socket handlers.
- Socket handlers should remain thin.
- Services contain business logic.
- Repositories access MongoDB.

---

# High-Level Flow

Client

↓

Socket.IO Connection

↓

Authentication

↓

Socket Handler

↓

Application Service

↓

Repository

↓

MongoDB

---

# Socket Lifecycle

1. Client establishes a WebSocket connection.
2. Client authenticates using a JWT.
3. Server validates the token.
4. Socket joins appropriate rooms.
5. Client sends and receives events.
6. Socket disconnects.
7. Presence is updated.

---

# Architectural Decisions

- A user may have multiple simultaneous socket connections.
- Socket handlers never directly access MongoDB.
- Authorization is performed before joining rooms.
- Every command should support acknowledgements.