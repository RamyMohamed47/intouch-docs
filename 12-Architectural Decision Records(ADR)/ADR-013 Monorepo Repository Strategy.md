# ADR-013: Monorepo Repository Strategy

- **Status:** Accepted

## Context

InTouch contains four closely related workspaces:

- `apps/api`: Express and Socket.IO backend.
- `apps/web`: Next.js browser client.
- `apps/mobile`: Expo React Native client.
- `packages/shared`: strict Zod contracts consumed by every application.

API contracts, realtime events, authentication, messaging, notifications, Echo, and media lifecycle changes frequently affect more than one workspace. The project is currently maintained as one product by one development team, but API, web, and mobile are deployed as independent runtime artifacts.

The repository strategy must reduce contract drift without turning the deployable applications into one inseparable process.

## Decision

Keep InTouch in one Git repository using npm workspaces.

- Applications remain under `apps/*` and reusable public contracts under `packages/*`.
- One root lockfile pins the dependency graph.
- Root scripts coordinate formatting, linting, strict typechecking, tests, OpenAPI validation, and production builds.
- Shared-contract changes update the API and consuming web/mobile clients in the same commit or pull request.
- Railway and EAS continue to build and deploy their relevant application independently.
- Application services communicate only through public contracts and network/runtime boundaries; the monorepo does not permit clients to import API internals.

## Decision Trade-offs

### Chosen: npm-workspace monorepo

**Pros**

- Contract, API, web, mobile, tests, and documentation changes can be reviewed atomically.
- `@intouch/shared` is consumed directly without publishing an intermediate package version for every change.
- One lockfile and shared tooling reduce dependency and configuration drift.
- Cross-application refactors are searchable and testable in one checkout.
- Root validation can prove that a contract change builds against every consumer before merge.
- Local development and onboarding require cloning and installing one repository.

**Cons**

- The checkout, dependency installation, and default validation pipeline grow as applications are added.
- CI and cloud builders need workspace-aware caching, path filtering, and correct working-directory configuration.
- A shared dependency upgrade can affect multiple applications simultaneously.
- Repository-level permissions and history are less isolated than separate repositories.
- Careless imports can create accidental coupling unless package boundaries are enforced.
- Independent release/version policies require explicit deployment configuration rather than repository separation.

### Alternative: Separate repository for API, web, mobile, and shared contracts

**Pros**

- Each application has a smaller checkout, focused history, independent permissions, and isolated CI configuration.
- Teams can release and version applications without running unrelated workspace checks.
- Build contexts naturally contain only the application being deployed.

**Cons**

- Cross-client features require coordinated commits and pull requests across several repositories.
- Shared contracts must be published and versioned before consumers can update.
- API, OpenAPI, web, and mobile behavior can drift while repositories use different contract versions.
- Reproducing one complete product state requires tracking compatible revisions across repositories.
- Local development, dependency updates, and broad refactors become more operationally expensive.

### Alternative: Separate application repositories with a published shared-contract package

**Pros**

- Preserves typed contracts while allowing each application to retain an independent repository and release cadence.
- Consumers can upgrade the shared contract deliberately rather than immediately.
- Package-version compatibility can be expressed explicitly.

**Cons**

- Every contract iteration requires package publishing, version management, and consumer upgrade work.
- Clients may remain on stale contract versions even when the backend changes.
- Coordinated breaking changes need staged releases and compatibility windows.
- Tooling, lint rules, TypeScript configuration, tests, and documentation are more likely to diverge.

## Consequences

- The repository is a monorepo, but the production system is not a single deployable monolith.
- Public DTOs and event contracts belong in `@intouch/shared`; backend services, repositories, secrets, and provider adapters remain API-private.
- Changes to shared contracts must update affected API, web, mobile, OpenAPI, tests, and documentation together.
- Root `npm run check` remains the principal cross-workspace quality gate, while application-specific commands support focused development.
- Deployment systems should use workspace-specific build commands and cache configuration to avoid rebuilding unrelated outputs unnecessarily.
- A repository split should be reconsidered if independent teams, access-control requirements, release cadences, compliance boundaries, or repository scale outweigh the value of atomic cross-application changes.

## Revisit Triggers

Re-evaluate this decision if:

- Different teams independently own and release API, web, and mobile.
- Repository permissions must prevent one team from reading or modifying another application.
- CI duration remains unacceptable after workspace-aware caching and path filtering.
- Shared contracts become a stable external SDK with independent consumers and semantic versioning.
- Regulatory or organizational boundaries require physically separate source repositories.
