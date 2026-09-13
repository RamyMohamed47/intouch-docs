# Sequence - Create Organization

~~~mermaid
sequenceDiagram
  actor User
  participant Client
  participant API
  participant DB as MongoDB transaction
  participant Socket as Socket.IO
  User->>Client: Submit organization
  Client->>API: POST /api/v1/organizations
  API->>API: Validate JWT and strict input
  API->>DB: Create organization and OWNER membership
  DB-->>API: Commit
  API-->>Client: 201 { organization }
  API->>Socket: publish post-commit membership/activity facts
~~~
