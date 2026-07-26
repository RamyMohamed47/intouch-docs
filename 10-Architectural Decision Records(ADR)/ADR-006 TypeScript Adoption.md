
## Status

Accepted

---

## Context

InTouch is intended to be a production-quality portfolio project that demonstrates modern backend engineering practices.

As the application grows, it will include:

- Multiple services
- Complex business logic
- Real-time communication
- Multi-tenancy
- Authentication and authorization
- Background jobs
- Shared data models

Using plain JavaScript increases the likelihood of runtime errors, inconsistent object structures, and duplicated assumptions across the codebase.

---

## Decision

The backend will be developed using **TypeScript**.

TypeScript will serve as the primary programming language for all backend code.

The project will use TypeScript's **strict mode** to maximize type safety and detect errors during development rather than at runtime.

---

## TypeScript Configuration

The project will enable:

- `"strict": true`
- `"noImplicitAny": true`
- `"strictNullChecks": true`

The goal is to catch as many issues as possible during compilation.

---

## Guiding Principles

### Simplicity First

Use the simplest type that clearly communicates intent.

Avoid unnecessary type complexity.

---

### Prefer Type Inference

Allow TypeScript to infer types whenever possible.

Avoid redundant type annotations.

Good example:

```ts
const organization = await organizationRepository.create(data);
```

Instead of:

```ts
const organization: Organization = await organizationRepository.create(data);
```

unless the explicit type improves readability.

---

### Avoid `any`

The `any` type should be avoided.

If a value is unknown, prefer:

```ts
unknown
```

and narrow the type appropriately.

---

### Interfaces for Domain Models

Interfaces will describe domain entities.

Examples include:

- User
- Organization
- Membership
- Conversation
- Message

---

### DTOs

Data Transfer Objects (DTOs) will define the shape of incoming requests.

Examples:

- CreateOrganizationDto
- RegisterUserDto
- SendMessageDto

DTOs improve validation and make service contracts explicit.

---

### Enums

Enums (or string literal unions where appropriate) will be used for fixed sets of values.

Examples:

- UserRole
- ConversationType
- MessageType
- NotificationType

---

### Shared Types

Types that are shared across multiple modules should live in a dedicated shared types directory.

This avoids duplication and keeps contracts consistent.

---

## Benefits

- Compile-time error detection.
- Better IDE support and autocomplete.
- Improved refactoring safety.
- Self-documenting code.
- Stronger service contracts.
- Better maintainability.
- Easier onboarding for future contributors.

---

## Alternatives Considered

### JavaScript

Pros

- Faster initial development.
- Less syntax.
- No compilation step.

Cons

- Runtime type errors.
- Weaker tooling.
- Less maintainable as complexity grows.

---

## Consequences

### Advantages

- Increased reliability.
- Better developer experience.
- Safer refactoring.
- More maintainable architecture.
- Professional codebase aligned with modern backend practices.

### Trade-offs

- Slightly steeper learning curve.
- Additional build step.
- More upfront effort to define types.

These trade-offs are acceptable given the project's goals.

---

## Future Considerations

As the project evolves, shared types may be extracted into a common package if the backend and frontend begin sharing contracts.

Examples include:

- API request/response DTOs
- Socket.IO event payloads
- Shared enums
- Validation schemas

For the MVP, all TypeScript types will remain within the backend project.