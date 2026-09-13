# Socket Flow

## Connection

~~~mermaid
sequenceDiagram
  participant Client
  participant Socket as Socket.IO
  participant Redis
  Client->>Socket: connect(auth.accessToken)
  Socket->>Socket: verify JWT and limits
  Socket->>Redis: create socket/presence lease
  Socket-->>Client: connected
  Client->>Socket: organization:subscribe
  Socket->>Socket: verify membership
  Socket-->>Client: typed acknowledgement
~~~

## Durable Message

~~~mermaid
sequenceDiagram
  participant Client
  participant API
  participant MongoDB
  participant Socket as Socket.IO
  Client->>API: POST /conversations/{id}/messages
  API->>MongoDB: transaction: message, claims, notifications
  MongoDB-->>API: commit
  API-->>Client: 201 message DTO
  API->>Socket: message:created after commit
~~~

Typing and room subscriptions are client socket commands. Messages, reactions, receipts, calls, and notification mutations remain REST commands.
