# Sequence - Email Verification and Reset

~~~mermaid
sequenceDiagram
  actor User
  participant Client
  participant API
  participant DB as MongoDB
  participant Jobs as BullMQ/reconciler
  participant Mail as Brevo or SMTP
  User->>Client: Register or request reset
  Client->>API: POST auth action
  API->>DB: Transaction: hashed token and encrypted outbox
  API-->>Client: Non-enumerating accepted response
  Jobs->>DB: Lease committed outbox record
  Jobs->>Mail: Send link
  User->>Client: Open web/mobile link and submit token
  Client->>API: Verify email or reset password
  API->>DB: Consume token atomically
  API-->>Client: 204 No Content
~~~

Raw action tokens are delivered only to the intended link/request body. MongoDB stores token hashes; encrypted outbox content is retried after commit.
