
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