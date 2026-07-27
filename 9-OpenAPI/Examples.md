# API Examples

## Overview

This document provides practical examples of common interactions with the InTouch REST API.

The examples complement the OpenAPI specification by demonstrating typical request and response flows.

All examples assume the current API version:

```
/api/v1
```

---

# Register

## Request

```http
POST /api/v1/auth/register
Content-Type: application/json
```

```json
{
  "username": "ramy",
  "email": "ramy@example.com",
  "password": "SecurePassword123!"
}
```

## Response

**201 Created**

```json
{
  "data": {
    "id": "usr_123",
    "username": "ramy",
    "email": "ramy@example.com",
    "createdAt": "2026-07-27T12:00:00Z"
  }
}
```

---

# Login

## Request

```http
POST /api/v1/auth/login
Content-Type: application/json
```

```json
{
  "email": "ramy@example.com",
  "password": "SecurePassword123!"
}
```

## Response

**200 OK**

```json
{
  "data": {
    "user": {
      "id": "usr_123",
      "username": "ramy",
      "email": "ramy@example.com"
    },
    "accessToken": "<jwt-access-token>"
  }
}
```

---

# Get Current User

## Request

```http
GET /api/v1/auth/me
Authorization: Bearer <access_token>
```

## Response

**200 OK**

```json
{
  "data": {
    "id": "usr_123",
    "username": "ramy",
    "email": "ramy@example.com"
  }
}
```

---

# Create Organization

## Request

```http
POST /api/v1/organizations
Authorization: Bearer <access_token>
Content-Type: application/json
```

```json
{
  "name": "Backend Study Group",
  "description": "A place to discuss backend engineering."
}
```

## Response

**201 Created**

```json
{
  "data": {
    "id": "org_456",
    "name": "Backend Study Group",
    "ownerId": "usr_123"
  }
}
```

---

# List Organizations

## Request

```http
GET /api/v1/organizations?page=1&limit=20
Authorization: Bearer <access_token>
```

## Response

**200 OK**

```json
{
  "data": [
    {
      "id": "org_456",
      "name": "Backend Study Group"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 1,
    "totalPages": 1
  }
}
```

---

# Create Conversation

## Request

```http
POST /api/v1/organizations/org_456/conversations
Authorization: Bearer <access_token>
Content-Type: application/json
```

```json
{
  "categoryId": "cat_001",
  "name": "general",
  "type": "CHANNEL"
}
```

## Response

**201 Created**

```json
{
  "data": {
    "id": "conv_789",
    "name": "general",
    "type": "CHANNEL"
  }
}
```

---

# Send Message

## Request

```http
POST /api/v1/conversations/conv_789/messages
Authorization: Bearer <access_token>
Content-Type: application/json
```

```json
{
  "content": "Hello, everyone!"
}
```

## Response

**201 Created**

```json
{
  "data": {
    "id": "msg_001",
    "conversationId": "conv_789",
    "senderId": "usr_123",
    "content": "Hello, everyone!",
    "createdAt": "2026-07-27T12:30:00Z"
  }
}
```

---

# Validation Error

## Response

**400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed.",
    "details": [
      {
        "field": "email",
        "message": "Invalid email address."
      }
    ]
  }
}
```

---

# Unauthorized

## Response

**401 Unauthorized**

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authentication is required."
  }
}
```

---

# Forbidden

## Response

**403 Forbidden**

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have permission to perform this action."
  }
}
```

---

# Resource Not Found

## Response

**404 Not Found**

```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "The requested resource was not found."
  }
}
```

---

# Related Documentation

- API Architecture.md
- Authentication.md
- Error Responses.md
- Pagination.md
- Versioning.md
- openapi.yaml