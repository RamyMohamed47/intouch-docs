
## Status

Accepted

---

## Context

InTouch exposes multiple entry points into the system.

These include:

- REST API
- Socket.IO
- Future background workers
- Future scheduled jobs
- Future CLI scripts

Each entry point may trigger the same business operations.

Examples include:

- Sending messages
- Creating organizations
- Inviting members
- Updating profiles

Duplicating business logic across controllers, socket handlers, and workers would increase maintenance costs and introduce inconsistencies.

---

## Decision

Business logic will reside exclusively within the Application Service layer.

Controllers and Socket.IO handlers will act only as transport adapters.

Their responsibilities are limited to:

- Receiving requests or events
- Validating input
- Calling the appropriate service
- Returning responses or emitting events

Repositories are responsible only for data persistence.

Business rules must never be implemented inside repositories.

---

## Architecture

```text
                    Client
                       │
          ┌────────────┴────────────┐
          │                         │
      REST API               Socket.IO
          │                         │
          └────────────┬────────────┘
                       │
             Application Services
                       │
                Repositories
                       │
                  MongoDB
```

---

## Responsibilities

### Controllers

- Parse HTTP requests
- Validate input
- Invoke services
- Return HTTP responses

Controllers must not contain business logic.

---

### Socket Handlers

- Receive socket events
- Validate payloads
- Invoke services
- Emit socket events

Socket handlers must not contain business logic.

---

### Application Services

Application Services contain the core business rules of the application.

Examples include:

- Authentication
- Organization management
- Membership management
- Conversation management
- Messaging

Services orchestrate repositories and enforce business rules.

---

### Repositories

Repositories abstract data persistence.

Responsibilities include:

- Querying MongoDB
- Saving documents
- Updating documents
- Deleting documents

Repositories must remain free of business logic.

---

## Benefits

- Single source of truth for business logic.
- Consistent behavior across REST and Socket.IO.
- Easier testing.
- Improved maintainability.
- Easier integration of future transports.

---

## Alternatives Considered

### Business Logic in Controllers

Pros

- Simple for very small applications.

Cons

- Logic duplicated across transports.
- Difficult to maintain.
- Difficult to test.

---

### Business Logic in Socket Handlers

Pros

- Direct event handling.

Cons

- Tight coupling.
- Duplicate logic.
- Poor separation of concerns.

---

## Consequences

### Advantages

- Clear separation of responsibilities.
- Reusable business logic.
- Easier future expansion.
- Cleaner architecture.

### Trade-offs

- Introduces an additional application layer.
- Slightly more boilerplate than directly accessing models.