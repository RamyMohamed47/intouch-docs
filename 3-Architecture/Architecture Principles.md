
1. Business logic lives in Services.
2. Controllers are thin.
3. Socket handlers are thin.
4. Repositories only access the database.
5. Every tenant-aware query includes organizationId.
6. Authentication is transport-independent.
7. Prefer composition over duplication.
8. Simplicity over premature optimization.
9. Make impossible states impossible