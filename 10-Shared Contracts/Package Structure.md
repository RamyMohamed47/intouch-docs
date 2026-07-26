
## Overview

The implementation repository will contain a shared package that defines contracts used by both the backend and frontend.

The package is organized **by feature (domain)** rather than by artifact type.

This keeps related schemas, types, enums, and utilities together, making the codebase easier to navigate and maintain as it grows.

---

## Package Structure

packages/

    shared/

        auth/
            register.schema.ts
            login.schema.ts
            auth.types.ts

        organizations/
            create-organization.schema.ts
            organization.types.ts

        conversations/
            create-conversation.schema.ts
            conversation.types.ts

        messages/
            send-message.schema.ts
            message.types.ts

        socket/
            events.ts
            event-maps.ts

        common/
            pagination.schema.ts
            api-response.schema.ts

---

## Principles

- Organize by feature.
- Keep schemas close to their related types.
- Keep shared utilities in a common module.
- Avoid organizing solely by file type.