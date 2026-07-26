```mermaid
sequenceDiagram

    actor User

    participant Client

    participant API

    participant MongoDB

    User->>Client: Create Organization

    Client->>API: POST /organizations

    API->>MongoDB: Create Organization

    API->>MongoDB: Create Membership (Owner)

    API-->>Client: Organization Created
```