
## Overview

This document describes the API versioning strategy used by the InTouch REST API.

API versioning allows the backend to evolve while maintaining compatibility for existing clients.

The API follows URL-based versioning.

---

# Version Format

The API version is included in the base URL.

Current version:

```
/api/v1
```

Example:

```
GET /api/v1/organizations

GET /api/v1/conversations/{conversationId}

POST /api/v1/auth/login
```

---

# Why URL Versioning?

Several versioning strategies exist, including:

- URL versioning
- Header versioning
- Query parameter versioning
- Content negotiation

InTouch uses URL versioning because it is:

- Simple
- Explicit
- Easy to document
- Widely supported
- Easy to debug
- Well supported by OpenAPI tools

---

# Current API Version

Current version:

```
v1
```

The initial release of the API defines the contract for all MVP functionality.

---

# Breaking Changes

Breaking changes require a new API version.

Examples include:

- Removing endpoints
- Renaming endpoints
- Removing request fields
- Removing response fields
- Changing response structures
- Changing authentication requirements
- Changing resource relationships
- Changing endpoint behaviour in an incompatible way

Example:

```
v1

GET /organizations
```

↓

```
v2

GET /workspaces
```

This would require a new major version.

---

# Non-Breaking Changes

The following changes may be introduced without creating a new API version.

Examples:

- Adding new endpoints
- Adding optional request fields
- Adding optional response fields
- Adding new resources
- Improving documentation
- Adding additional error codes
- Performance improvements
- Internal implementation changes

These changes remain backward compatible.

---

# Deprecation Policy

When an endpoint is scheduled for removal:

1. The endpoint is marked as deprecated.
2. Documentation is updated.
3. A replacement endpoint is provided whenever possible.
4. The endpoint remains available for a reasonable migration period.

Example:

```yaml
deprecated: true
```

---

# OpenAPI Integration

The current API version is defined in the OpenAPI specification.

Example:

```yaml
info:
  version: 1.0.0
```

The server URL also includes the API version.

```yaml
servers:

  - url: http://localhost:3000/api/v1
```

---

# Future Versions

Future API versions may introduce:

```
/api/v2
/api/v3
```

Each version represents a new API contract.

Older versions may continue to be supported during a migration period.

---

# Version Compatibility

Clients should only rely on the documented contract for the API version they consume.

Applications using:

```
/api/v1
```

should not assume compatibility with future versions.

---

# Design Principles

The InTouch API follows these versioning principles:

- Explicit URL-based versioning
- Backward compatibility whenever possible
- Breaking changes require a new version
- Non-breaking changes remain within the current version
- Stable API contracts
- Clear migration paths for future versions