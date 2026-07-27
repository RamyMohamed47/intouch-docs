
## Overview

This document defines the standard error response format used by the InTouch REST API.

Every endpoint returns errors using a consistent structure to simplify error handling for API consumers.

Unless otherwise stated, all endpoints follow the response format described in this document.

---

# Error Response Format

All error responses use the following structure:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed.",
    "details": []
  }
}
```

## Fields

| Field | Type | Description |
|--------|------|-------------|
| code | string | Machine-readable error identifier |
| message | string | Human-readable error description |
| details | array | Additional information about the error (optional) |

---

# Validation Errors

Validation errors occur when the request payload, query parameters, or path parameters fail validation.

Example:

**HTTP 400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed.",
    "details": [
      {
        "field": "email",
        "message": "Invalid email address."
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters."
      }
    ]
  }
}
```

---

# Authentication Errors

Authentication errors occur when a request cannot be authenticated.

**HTTP 401 Unauthorized**

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authentication is required."
  }
}
```

Possible causes:

- Missing access token
- Invalid access token
- Expired access token

---

# Authorization Errors

Authorization errors occur when the authenticated user does not have permission to perform the requested operation.

**HTTP 403 Forbidden**

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have permission to perform this action."
  }
}
```

Examples:

- Not an organization member
- Insufficient role
- Resource access denied

---

# Resource Not Found

Returned when the requested resource does not exist.

**HTTP 404 Not Found**

```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "The requested resource was not found."
  }
}
```

---

# Resource Conflict

Returned when a request conflicts with the current state of the system.

**HTTP 409 Conflict**

Example:

```json
{
  "error": {
    "code": "EMAIL_ALREADY_EXISTS",
    "message": "An account with this email already exists."
  }
}
```

Other examples:

- Organization name already exists
- User is already a member
- Identity provider already linked

---

# Rate Limiting

Returned when the client exceeds the allowed request rate.

**HTTP 429 Too Many Requests**

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again later."
  }
}
```

---

# Internal Server Error

Returned when an unexpected server error occurs.

**HTTP 500 Internal Server Error**

```json
{
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "An unexpected error occurred."
  }
}
```

Internal implementation details are never exposed.

---

# Standard HTTP Status Codes

| Status | Meaning |
|---------|---------|
| 200 | Request successful |
| 201 | Resource created |
| 204 | Request successful, no response body |
| 400 | Validation or malformed request |
| 401 | Authentication failed or missing |
| 403 | Permission denied |
| 404 | Resource not found |
| 409 | Resource conflict |
| 429 | Rate limit exceeded |
| 500 | Unexpected server error |

---

# Error Codes

The API uses machine-readable error codes.

Common error codes include:

| Code | Description |
|------|-------------|
| VALIDATION_ERROR | Request validation failed |
| UNAUTHORIZED | Authentication required |
| FORBIDDEN | Permission denied |
| NOT_FOUND | Resource not found |
| EMAIL_ALREADY_EXISTS | Duplicate email address |
| MEMBER_ALREADY_EXISTS | User is already a member |
| ORGANIZATION_NOT_FOUND | Organization does not exist |
| CONVERSATION_NOT_FOUND | Conversation does not exist |
| MESSAGE_NOT_FOUND | Message does not exist |
| RATE_LIMIT_EXCEEDED | Too many requests |
| INTERNAL_SERVER_ERROR | Unexpected server error |

Clients should rely on `code` rather than `message` when implementing application logic.

---

# OpenAPI Integration

Common error responses are defined in `openapi.yaml` under:

```yaml
components:
  responses:
```

Endpoints reference these reusable responses instead of redefining them.

Example:

```yaml
responses:
  "400":
    $ref: "#/components/responses/ValidationError"

  "401":
    $ref: "#/components/responses/Unauthorized"

  "404":
    $ref: "#/components/responses/NotFound"
```

---

# Design Principles

The InTouch API follows these principles for error handling:

- Consistent response structure
- Standard HTTP status codes
- Machine-readable error codes
- Human-readable messages
- Reusable OpenAPI response definitions
- No leakage of internal implementation details