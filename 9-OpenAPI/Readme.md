# OpenAPI

The canonical OpenAPI 3.1 specification is `openapi.yaml`. It currently documents 95 REST operations over 70 path templates under `/api/v1`.

## Published Endpoints

- `/api/docs`: branded read-only Swagger UI.
- `/api/openapi.yaml`: canonical YAML served by the API/web proxy.
- `/api/openapi.json`: generated JSON form.

Swagger request execution and persisted authorization are disabled intentionally. Use the application, curl, Postman, or another client for authenticated requests.

## Ownership

Shared Zod schemas provide runtime validation and inferred TypeScript types. OpenAPI documents the same public contract for humans and external tools. A contract change updates schemas, API behavior, tests, and OpenAPI in one change.

Socket.IO events and LiveKit media are outside OpenAPI. See [[8-Socket.IO/Socket Events|Socket Events]] and [[14-Mobile/Mobile V2.0|Mobile V2.0]].

Validate the source in the application repository with `npm run openapi:lint`.
