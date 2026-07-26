
## Overview

Validation occurs before business logic executes.

Invalid data never reaches the service layer.

---

## Validation Library

Zod

---

## Validation Responsibilities

Validate

- HTTP requests
- Socket payloads
- Query parameters
- Path parameters
- Environment variables

---

## Service Layer

Services assume validated input.

Services do not repeat validation already performed by the transport layer.

---

## Principle

Validate at the boundary.