```mermaid
sequenceDiagram

    actor User

    participant Client

    participant API

    participant MongoDB

    User->>Client: Login

    Client->>API: POST /auth/login

    API->>MongoDB: Find user by email

    MongoDB-->>API: User

    API->>API: Verify password

    API-->>Client: Access Token + Refresh Token

    Client-->>User: Logged In
```