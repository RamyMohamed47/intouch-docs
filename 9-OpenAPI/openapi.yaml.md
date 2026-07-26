openapi: 3.1.0

info:
  title: InTouch API
  version: 1.0.0
  description: |
    REST API for InTouch.

    InTouch is a multi-tenant real-time messaging platform inspired  by Discord.

servers:
  - url: http://localhost:3000/api/v1
    description: Local

  - url: https://api.intouch.app/api/v1
    description: Production