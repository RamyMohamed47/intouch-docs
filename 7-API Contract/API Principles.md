
## Overview

This document defines the API design principles followed throughout InTouch.

The API follows RESTful conventions and is versioned to support future evolution without breaking existing clients.

---

# Base URL

/api/v1

---

# Versioning

The API is versioned through the URL.

Example:

/api/v1/auth/login

Future versions may introduce:

/api/v2/

without affecting existing clients.

---

# Resource Naming

Resources use plural nouns.

Examples:

- /organizations
- /members
- /messages
- /categories
- /conversations

Avoid verbs in URLs.

Good:

POST /organizations

Bad:

POST /createOrganization

---

# HTTP Methods

GET

Retrieve resources.

POST

Create resources.

PATCH

Partially update resources.

DELETE

Delete resources.

---

# Authentication

Protected endpoints require:

Authorization: Bearer <access_token>

---

# Response Format

Successful responses

```json
{
    "success": true,
    "data": {}
}
```

Failed responses

```json
{
    "success": false,
    "error": {
        "code": "",
        "message": ""
    }
}
```

---

# Pagination

Collections should support pagination.

Example:

GET /messages?page=1&limit=50

---

# Filtering

Filtering should use query parameters.

Example:

GET /organizations?search=openai

---

# Sorting

Sorting should use query parameters.

Example:

GET /messages?sort=createdAt

---

# Authorization

Authentication identifies the user.

Authorization determines whether the user may access a resource.

Every organization-aware endpoint must verify:

- The authenticated user belongs to the organization.
- The requested resource belongs to that organization.

---

# API Philosophy

- Keep endpoints predictable.
- Keep responses consistent.
- Keep business logic out of controllers.
- Design around resources, not actions.