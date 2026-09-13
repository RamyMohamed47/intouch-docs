# Contract Philosophy

- Every external boundary has an explicit runtime contract.
- Zod schemas are canonical for public TypeScript shapes; types are inferred, never duplicated manually.
- `@intouch/shared` is consumed by API, Next.js web, and Expo mobile.
- Inputs are strict and reject unknown keys. Outputs are parsed before delivery to serialize dates and strip persistence/provider fields.
- REST contracts, Socket.IO maps, OpenAPI components, and contract tests evolve together.
- Internal repository records and provider interfaces remain internal TypeScript types when runtime validation adds no boundary value.
- Contracts expose opaque asset/media/session identifiers and safe public summaries, never credentials, hashes, storage keys, presigned URLs beyond their intended response, or provider metadata.
