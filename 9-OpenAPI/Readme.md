# OpenAPI Specification

## Overview

This directory contains the REST API contract for **InTouch**.

The OpenAPI specification acts as the single source of truth for all HTTP endpoints exposed by the backend. It defines:

- Available endpoints
- Request and response payloads
- Authentication requirements
- Validation rules
- Error responses
- Resource schemas

The specification is implementation-agnostic and is intended to be completed before backend development begins.

---

# Design Philosophy

The API follows a **contract-first** approach.

Development workflow:

```
Requirements
    ↓
Architecture
    ↓
Database Design
    ↓
Shared Contracts (Zod)
    ↓
OpenAPI Specification
    ↓
Backend Implementation
```

The OpenAPI specification should always reflect the intended behaviour of the system rather than the current implementation.

---

# API Principles

The API follows REST principles wherever appropriate.

- Resource-oriented URLs
- Predictable HTTP methods
- Stateless authentication
- Consistent response format
- Consistent error handling
- Versioned endpoints
- JWT Bearer authentication

Realtime communication is intentionally excluded from this specification.

Socket.IO events are documented separately in:

```
07 Socket.IO/
```

---

# API Version

Current version:

```
v1
```

Base URL:

```
/api/v1
```

---

# Authentication

Protected endpoints require:

```
Authorization: Bearer <access_token>
```

Authentication methods:

- Email + Password
- Google OAuth

Future providers may be added without changing the User model through the Identity Provider architecture.

---

# Resource Modules

The API is organised by feature.

- Authentication
- Users
- Organizations
- Memberships
- Categories
- Conversations
- Messages

Each module owns its own endpoints.

---

# Response Format

Successful responses

```json
{
  "data": {}
}
```

Error responses

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed.",
    "details": []
  }
}
```

---

# Pagination

Collection endpoints support pagination.

Default strategy:

- page
- limit

Future enhancements may include cursor-based pagination.

---

# OpenAPI Version

This project targets:

```
OpenAPI 3.1
```

---

# Future Work

Future versions may include:

- Invitation API
- Presence API
- File Upload API
- Search API
- Read Receipts
- Threads
- Reactions

These features are intentionally excluded from the MVP.