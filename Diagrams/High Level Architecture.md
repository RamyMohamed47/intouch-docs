# High-Level Architecture

~~~mermaid
flowchart TB
  Web[Next.js web] -->|same-origin REST proxy| API[Express API]
  Web -->|Socket.IO| API
  Mobile[Expo mobile] -->|REST and Socket.IO| API
  API --> Mongo[(MongoDB)]
  API --> Redis[(Redis and BullMQ)]
  API --> R2[Cloudflare R2]
  API --> LiveKit[LiveKit Cloud]
  API --> Gemini[Gemini]
  API --> Mail[Brevo or SMTP]
  API --> Push[Expo Push / FCM]
  API --> OTLP[Grafana OTLP]
  Web --> Sentry[Sentry]
  Mobile --> Sentry
  API --> Sentry
  Web -->|media| LiveKit
  Mobile -->|media| LiveKit
  Web -->|presigned bytes| R2
  Mobile -->|presigned bytes| R2
~~~

MongoDB is durable authority. Redis is distributed ephemeral state and scheduling. External providers receive only the minimum authorized data needed for their boundary.
