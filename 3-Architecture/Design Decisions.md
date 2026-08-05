
## Decision 1

Users are global.

Reason:

A user may belong to multiple organizations.

---

## Decision 2

Organizations are isolated tenants.

Reason:

Demonstrates SaaS architecture and multi-tenancy.

---

## Decision 3

Workspace-scoped Direct Messages.

Reason:

Users may only message other users inside the same organization.

This simplifies authorization and preserves tenant isolation.

---

## Decision 4

Discord-inspired hierarchy.

Organization

↓

Category

↓

Channel

↓

Messages

---

## Decision 5

MongoDB

Reason:

- Flexible schema
- Fast development
- Good fit for append-heavy chat messages
- Excellent Node.js ecosystem

---

## Decision 6

Single Database

Shared Collections

Tenant isolation using organizationId.

---

## Decision 7

Socket.IO

Reason:

Provides real-time communication while abstracting WebSocket complexity.

---

## Decision 8

Railway Deployment

Reason:

Simple deployment suitable for a portfolio project.

---
## Decision 9

InTouch adopts a **feature-based layered (N-Tier) architecture**. Each feature is organized into the following layers: 
``` 
 Route
   ↓
Controller
   ↓ 
Validation
   ↓ 
Service 
   ↓
Repository 
   ↓
Database 
```

 For real-time communication: 
 ```
   Socket Event
       ↓
    Validation
       ↓
    Socket Handler 
       ↓
    Service
       ↓
    Repository
       ↓
    Database
 ```
 
 Business logic resides exclusively in the Service layer, while Controllers and Socket Handlers remain thin transport adapters.
 
  ---
  
   ### Rationale 
   
   This architecture was chosen to achieve: 
   - **Separation of concerns** by assigning each layer a single responsibility. 
   - **Transport independence**, allowing the same business logic to be reused by REST endpoints, Socket.IO events, background jobs, or other interfaces. 
   - **Maintainability**, as business rules, persistence, and transport logic evolve independently. 
   - **Testability**, enabling services to be unit-tested without Express, Socket.IO, or MongoDB. 
   - **Scalability**, providing clear extension points for caching, messaging, and additional infrastructure without affecting higher layers. 
   - **Professional architecture**, reflecting patterns commonly used in production backend systems.
 --- 
   
### Advantages 

- Thin controllers and socket handlers. 
- Centralized business logic. 
- Reusable services across multiple transports. 
- Clear dependency flow. 
- Easier unit and integration testing. 
- Improved readability and maintainability.

### Trade-offs

- More files and boilerplate compared to simpler architectures. 
- Additional indirection when tracing request flow. 
- Slightly higher initial development overhead. 

 These trade-offs are considered acceptable given the project's goal of demonstrating production-quality backend engineering practices. 
 
 ---
 
  ## Alternatives Considered
  
   ### MVC 
   
   Rejected because controllers or models often accumulate business logic, reducing maintainability as the application grows. 
   
   ### Clean Architecture 
   
   Rejected because it introduces additional abstractions (entities, use cases, ports, adapters) that would add unnecessary complexity for the scope of this portfolio project.
   
   ---

## Guiding Principles 

- Controllers handle HTTP concerns only. 
- Socket Handlers handle real-time transport concerns only. 
- Services own all business logic. 
- Repositories own all database interactions. 
- Validation occurs at application boundaries.
- Dependencies flow inward; lower layers never depend on higher layers.

___
## Decision 10

InTouch requires MongoDB transaction support in all environments. Local development must use a single-node replica set. The application does not implement fallback or compensating transaction logic for standalone MongoDB deployments.