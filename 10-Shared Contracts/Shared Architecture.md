# Shared Contracts Architecture

## Overview

InTouch uses a shared contracts approach to define all communication between clients and the backend.

A contract describes the structure of data exchanged across application boundaries.

Examples include:

- REST request DTOs
- REST response DTOs
- Socket.IO event payloads
- Shared enums
- Validation schemas

---

# Goals

- Single source of truth
- Type safety
- Runtime validation
- Shared client/server models
- Eliminate duplicated interfaces

---

# Architecture

                Zod Schema
                     │
        ┌────────────┴────────────┐
        │                         │
 Runtime Validation      TypeScript Types
        │                         │
        └────────────┬────────────┘
                     │
        Backend & Frontend

---

# Principles

- Define once.
- Reuse everywhere.
- Never duplicate contracts.
- Keep business logic independent of transport.