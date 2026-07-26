
## Contracts Define Communication

Every boundary in the system should have an explicit contract.

Examples

- HTTP Request
- HTTP Response
- Socket Event
- Internal Service DTO

---

## Contracts Are Stable

Business logic evolves.

Contracts should evolve carefully.

Breaking changes require versioning.

---

## Contracts Are Shared

The frontend and backend should use the exact same definitions.

---

## Contracts Are Validated

Every incoming payload must be validated before entering the application layer.

---

## Contracts Are Typed

All TypeScript types should be inferred from Zod schemas.

Zod schemas are the canonical definition of a contract. TypeScript types are always inferred from those schemas.