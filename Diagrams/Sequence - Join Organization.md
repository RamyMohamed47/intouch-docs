# Sequence - Join Organization

~~~mermaid
sequenceDiagram
  actor User
  participant Client
  participant API
  participant DB as MongoDB transaction
  participant Socket as Socket.IO
  alt Public organization
    Client->>API: POST /organizations/{id}/join
    API->>DB: Verify visibility and create MEMBER membership
  else Invitation
    Client->>API: POST /invitations/{invitationId}/accept
    API->>DB: Verify recipient, consume invitation, create membership/notifications
  end
  DB-->>API: Commit
  API-->>Client: 201 { membership }
  API->>Socket: membership:joined and notification changes
  Client->>Socket: organization:subscribe
  Socket-->>Client: authorized acknowledgement
~~~
