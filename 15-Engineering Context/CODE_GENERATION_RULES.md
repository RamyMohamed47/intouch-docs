> [!IMPORTANT] Mirrored engineering context
> Canonical source: `.agents/CODE_GENERATION_RULES.md` in the application repository. Update the canonical file first; this vault copy is refreshed from it.

Never:
- Put business logic in controllers.
- Access Mongoose directly from controllers.
- Duplicate validation schemas.
- Use `any`.
- Create circular dependencies.
- Mix transport logic with business logic.

Always:
- Validate with Zod.
- Return typed responses.
- Use repositories for persistence.
- Throw domain errors from services.
- Follow the feature module structure.
- Write JSDoc only when it adds value.
