# Multi-Tenancy

InTouch uses a shared-database, shared-collection model. Tenant-owned records carry `organizationId`; nested resources are resolved through their organization before authorization.

## Authorization Rules

1. Authenticate the user.
2. Resolve the organization from the route or owning resource.
3. Verify an active membership.
4. Apply role policy: `OWNER` or `MEMBER`.
5. For private channels and direct messages, verify a current `ConversationParticipant` record.
6. Only then read, mutate, issue asset/media credentials, or emit scoped realtime data.

Client-supplied organization IDs are never sufficient authorization. Repositories accept scoped inputs, services enforce policy, and provider adapters receive only already-authorized operations.

## Scoped Resources

- Organizations, memberships, invitations, categories, conversations, participants, messages, reactions, read states, call sessions, notifications, mutes, assets, search documents, and AI context.
- Redis keys and BullMQ jobs use environment/application prefixes and opaque IDs; they do not replace tenant checks.
- Socket.IO rooms use `organization:<id>`, `conversation:<id>`, and `user:<id>`, but room admission is always server-authorized.

## Global/User-Scoped Records

Users, authentication sessions, action tokens, default wallpaper preferences, and push installations are user-scoped rather than tenant-owned. Operations that refer to an organization or conversation still prove access before returning data.

## Deletion and Revocation

Organization, membership, private-access, and conversation changes revoke affected sockets, media participants, Redis leases, and private-asset access after the durable mutation commits. Cleanup is idempotent and retried when providers are unavailable.
