
## Initial Connection

Client

↓

Connect

↓

Authenticate

↓

Join Organization Rooms

↓

Join Conversation Room

↓

Ready

---

# Sending a Message

Client

↓

message:send

↓

Socket Handler

↓

Message Service

↓

Message Repository

↓

MongoDB

↓

Broadcast

message:received

---

# Typing Indicator

Client

↓

typing:start

↓

Server

↓

Broadcast

typing:update

↓

Clients display typing indicator

---

# Disconnect

Socket disconnects

↓

Server removes socket

↓

If no sockets remain

↓

Presence becomes Offline

↓

Broadcast presence:update