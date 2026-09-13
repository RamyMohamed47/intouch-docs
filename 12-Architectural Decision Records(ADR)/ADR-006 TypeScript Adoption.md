# ADR-006: Strict TypeScript and Runtime Contracts

- **Status:** Accepted

## Decision

Use strict NodeNext TypeScript across API, web, mobile, and shared packages. Local ESM imports include `.js` specifiers intentionally. Public contracts are strict Zod schemas in `@intouch/shared`, with TypeScript types inferred from schemas.


## Decision Trade-offs

### Chosen: Strict TypeScript plus shared Zod runtime contracts

**Pros**

- API, web, and mobile share discoverable types and runtime validation.
- NodeNext catches import and boundary inconsistencies before deployment.
- Schema-derived types reduce duplicated contract definitions.

**Cons**

- Build/typecheck steps and strict migrations increase development effort.
- Zod plus OpenAPI still requires disciplined coordinated updates.
- React Native, Next.js, and Node toolchains can expose different TypeScript integration edge cases.

### Alternative: TypeScript interfaces without runtime schemas

**Pros**

- Lower runtime and authoring overhead.

**Cons**

- Untrusted JSON remains unchecked at runtime.
- Client/server types can claim safety while accepting malformed payloads.

### Alternative: JavaScript or separately handwritten client types

**Pros**

- Fast initial prototyping and fewer compiler constraints.

**Cons**

- Contract drift is detected late through runtime bugs.
- Refactors across API, web, and mobile become less reliable.

## Consequences

- API, Next.js, and Expo clients share compile-time types and runtime validation.
- OpenAPI is coordinated documentation, not an independent competing type source.
- Provider and persistence records remain private internal types and are mapped before crossing boundaries.
- CI/root checks include formatting, lint, strict typechecking, tests, OpenAPI linting, and production builds.

JavaScript-only or independently duplicated client DTOs were rejected because they allow silent contract drift.
