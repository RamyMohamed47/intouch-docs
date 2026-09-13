# Sequence - Google Authentication

~~~mermaid
sequenceDiagram
  actor User
  participant Client
  participant Google
  participant API
  participant DB as MongoDB
  alt Browser authorization-code flow
    Client->>API: GET /auth/oauth/google
    API-->>Google: Redirect with signed state
    Google-->>API: Callback authorization code
    API->>Google: Exchange and verify identity token
    API-->>Client: Redirect; refresh through HttpOnly cookie
  else Native ID-token flow
    Client->>Google: Native sign-in
    Google-->>Client: ID token
    Client->>API: POST /auth/mobile/google
    API->>API: Verify signature, issuer, expiry, audience, email
    API-->>Client: InTouch access and refresh credentials
  end
  API->>DB: Link verified provider subject and refresh avatar fallback
~~~
