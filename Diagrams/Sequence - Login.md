# Sequence - Password Login

~~~mermaid
sequenceDiagram
  actor User
  participant Client
  participant API
  participant Redis
  participant DB as MongoDB
  User->>Client: Submit credentials
  Client->>API: POST browser or mobile login
  API->>Redis: Apply shared IP rate limit
  API->>DB: Apply hashed account throttle and load user
  API->>API: Verify password and email status
  API->>DB: Store hashed rotating refresh session
  alt Browser
    API-->>Client: user + access token + HttpOnly cookie
  else Mobile
    API-->>Client: user + access token + refresh token
  end
~~~
