# Shared Contracts

This section documents `@intouch/shared`, the workspace package containing
strict Zod schemas and inferred TypeScript types shared by the API, web, and
mobile applications.

## Reading Order

1. [[10-Shared Contracts/Shared Architecture|Shared Architecture]]
2. [[10-Shared Contracts/Contract Philosophy|Contract Philosophy]]
3. [[10-Shared Contracts/Package Structure|Package Structure]]
4. [[10-Shared Contracts/Validation Strategy|Validation Strategy]]
5. [[10-Shared Contracts/DTO Strategy|DTO Strategy]]
6. [[10-Shared Contracts/Event Types|Event Types]]
7. [[10-Shared Contracts/Examples|Examples]]

Shared contracts define public transport shape. They do not contain application
services, persistence models, provider clients, or client-specific UI state.
