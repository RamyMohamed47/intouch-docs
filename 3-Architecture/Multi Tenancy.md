
## Strategy

Single Database

Shared Collections

Tenant Isolation via organizationId.

Example:

Organizations

OpenAI

Gaming Community

University

Each document belongs to exactly one organization.

Example Message

organizationId

chatId

senderId

content

createdAt

---

## Users

Users are global.

A user may belong to multiple organizations.

This relationship is represented using the Membership collection.

User

↓

Membership

↓

Organization

This allows:

- One account
- Multiple workspaces
- Different roles per organization