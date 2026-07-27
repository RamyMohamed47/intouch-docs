
## Overview

Many InTouch API endpoints return collections of resources.

To improve performance and reduce response size, collection endpoints use pagination instead of returning all resources in a single response.

Pagination is applied consistently across the API.

---

# Pagination Strategy

The InTouch REST API uses **offset-based pagination**.

Clients specify the desired page and page size using query parameters.

Example:

```
GET /organizations?page=1&limit=20
```

---

# Query Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| page | integer | No | 1 | The page number to retrieve. |
| limit | integer | No | 20 | The maximum number of items returned per page. |

Constraints:

- `page` must be greater than or equal to **1**.
- `limit` must be greater than or equal to **1**.
- `limit` should not exceed **100**.

Requests that violate these constraints return:

```
400 Bad Request
```

---

# Response Format

Paginated responses follow a consistent structure.

```json
{
  "data": [
    ...
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 125,
    "totalPages": 7
  }
}
```

---

# Pagination Fields

| Field | Type | Description |
|-------|------|-------------|
| page | integer | Current page number. |
| limit | integer | Maximum number of items returned. |
| total | integer | Total number of matching resources. |
| totalPages | integer | Total number of available pages. |

---

# Example Request

```
GET /api/v1/conversations?page=2&limit=25
```

---

# Example Response

```json
{
  "data": [
    {
      "id": "conv_123",
      "name": "general"
    },
    {
      "id": "conv_124",
      "name": "backend"
    }
  ],
  "pagination": {
    "page": 2,
    "limit": 25,
    "total": 73,
    "totalPages": 3
  }
}
```

---

# Endpoints Supporting Pagination

Pagination is supported by all collection endpoints.

Examples include:

- `GET /organizations`
- `GET /organizations/{organizationId}/members`
- `GET /organizations/{organizationId}/categories`
- `GET /organizations/{organizationId}/conversations`
- `GET /conversations/{conversationId}/messages`

Single-resource endpoints do not support pagination.

---

# Invalid Requests

Invalid pagination parameters result in a validation error.

Example:

```
GET /organizations?page=-1&limit=500
```

Response:

```
400 Bad Request
```

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed.",
    "details": [
      {
        "field": "page",
        "message": "Page must be greater than or equal to 1."
      }
    ]
  }
}
```

---

# OpenAPI Integration

Reusable pagination parameters are defined in the OpenAPI specification.

```yaml
components:
  parameters:

    Page:

    Limit:
```

Paginated response schemas include a reusable pagination object.

```yaml
components:
  schemas:

    Pagination:
```

This ensures pagination is documented consistently across all collection endpoints.

---

# Future Enhancements

The MVP uses offset-based pagination.

Future versions of the API may introduce cursor-based pagination for endpoints containing large or frequently changing datasets, such as messages.

Cursor-based pagination would improve performance and consistency when navigating real-time data.

The introduction of cursor-based pagination would be considered a breaking API change and would require a new API version or an alternative endpoint contract.

---

# Design Principles

The pagination strategy follows these principles:

- Consistent across all collection endpoints.
- Uses standard query parameters.
- Returns pagination metadata with every paginated response.
- Provides predictable response structures.
- Supports future evolution without affecting the MVP.