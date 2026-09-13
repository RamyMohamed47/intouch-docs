# Data Flow Diagram - Level 0

~~~mermaid
flowchart LR
  User((User)) -->|auth, workspaces, chat, calls, Echo| InTouch[InTouch Platform]
  InTouch -->|application data| Mongo[(MongoDB)]
  InTouch -->|runtime leases and jobs| Redis[(Redis)]
  InTouch -->|private assets| R2[Cloudflare R2]
  InTouch -->|media credentials and lifecycle| LiveKit[LiveKit]
  InTouch -->|authorized AI context| Gemini[Gemini]
  InTouch -->|mail and push| Delivery[Brevo/SMTP/Expo]
  InTouch -->|sanitized telemetry| Monitoring[Grafana and Sentry]
~~~
