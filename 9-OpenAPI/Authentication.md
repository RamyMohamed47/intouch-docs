
## Overview

This document describes how clients authenticate with the InTouch REST API.

The API uses JWT Bearer Authentication for protected endpoints. Authentication requirements are defined in the OpenAPI specification using the `bearerAuth` security scheme.

---

# Authentication Scheme

The API uses the following HTTP authentication scheme:

```
Authorization: Bearer <access_token>
```

Example:

```
GET /api/v1/organizations

Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

---

# Security Scheme

The OpenAPI specification defines a global security scheme:

```yaml
components:
  securitySchemes:

    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

All protected endpoints inherit this security requirement.

---

# Public Endpoints

The following endpoints do not require authentication.

| Method | Endpoint |
|----------|----------|
| POST | /auth/register |
| POST | /auth/login |
| POST | /auth/refresh |

These endpoints override the global security requirement using:

```yaml
security: []
```

---

# Protected Endpoints

Unless explicitly documented otherwise, all endpoints require a valid Bearer token.

Examples include:

- GET /auth/me
- GET /organizations
- POST /organizations
- GET /conversations/{conversationId}
- POST /conversations/{conversationId}/messages

---

# Authentication Responses

| Status | Meaning |
|----------|----------|
| 200 | Authentication successful |
| 201 | User successfully registered |
| 400 | Invalid request |
| 401 | Authentication failed |
| 403 | Authenticated but not authorised |

---

# Token Usage

Clients must include the access token in the `Authorization` header for every protected request.

The server validates the token before processing the request.

---

# OpenAPI Integration

Authentication is defined globally within `openapi.yaml`.

Protected endpoints inherit the global security configuration.

Public endpoints disable authentication explicitly.

This approach avoids duplicating security definitions across the specification.

---

# Related Documentation

- API Architecture.md
- Error Responses.md
- Versioning.md
- openapi.yaml