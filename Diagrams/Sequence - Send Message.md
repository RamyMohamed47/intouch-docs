# Sequence - Send Message

~~~mermaid
sequenceDiagram
  actor Sender
  participant Client
  participant API
  participant DB as MongoDB transaction
  participant Socket as Socket.IO
  participant Jobs as BullMQ
  Client->>API: POST /conversations/{id}/messages
  API->>API: Validate membership, participants, reply, mentions, uploads
  API->>DB: Create message, claim assets, create notifications/outbox
  DB-->>API: Commit
  API-->>Client: 201 { message }
  API->>Socket: message:created and notification/activity facts
  API->>Jobs: Schedule eligible push delivery
~~~

Socket.IO never receives a `message:send` command; it distributes post-commit facts only.
