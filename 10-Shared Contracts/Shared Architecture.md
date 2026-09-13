# Shared Contracts Architecture

~~~mermaid
flowchart TD
  Zod[Domain Zod schemas] --> Types[Inferred TypeScript types]
  Zod --> Runtime[Runtime parsing]
  Zod --> Tests[Contract tests]
  Zod -. coordinated .-> OpenAPI[OpenAPI 3.1]
  Types --> API[Express API]
  Types --> Web[Next.js web]
  Types --> Mobile[Expo mobile]
  Runtime --> API
  Runtime --> Web
  Runtime --> Mobile
~~~

The shared package defines transport data, not business services or persistence models. The API owns authorization and domain behavior; clients gain compile-time guidance and runtime protection without importing server internals.

Provider payloads are translated at adapters. LiveKit, Gemini, R2, Expo, Google, Brevo, MongoDB, and Redis structures do not become public DTOs accidentally.
