# Database Entities

This document describes the primary entities of the InTouch database and their responsibilities.

---

# User

Represents a global account in the system.

A user exists independently of any organization and may belong to multiple organizations through the Membership collection.

## Responsibilities

- Authentication
- User profile
- Global account information
- OAuth accounts

---

# Organization

Represents a tenant (workspace).

Organizations are the core of the multi-tenant architecture. Every resource except users belongs to exactly one organization.

## Responsibilities

- Workspace information
- Members
- Categories
- Conversations
- Organization settings

---

# Membership

Represents the relationship between a User and an Organization.

This enables a many-to-many relationship between users and organizations.

A membership also stores organization-specific information about a user.

## Responsibilities

- User role
- Join date
- Membership status
- Future permissions

---

# Category

Used to organize conversations inside an organization.

Categories are optional.

A conversation may either belong to a category or exist directly under the organization.

## Responsibilities

- Organize conversations
- Improve workspace navigation

Example:

Engineering
- backend
- frontend

Social
- memes
- gaming

---

# Conversation

Represents any communication thread.

A conversation may be either:

- Channel
- Direct Message (DM)

Instead of maintaining separate collections for channels and DMs, both are represented using a single Conversation entity.

## Conversation Types

- CHANNEL
- DM

A conversation belongs to exactly one organization.

---

# Message

Represents an individual message inside a conversation.

Messages belong to exactly one conversation.

## Supported Types

- Text
- Image
- File
- Voice (future)
- System

Future features such as replies, reactions and edits will be implemented on top of this entity.

---

# Attachment

Represents a file attached to a message.

Examples include:

- Images
- Videos
- PDFs
- Documents

Files will initially be stored using Cloudinary.

---

# Notification

Represents a notification delivered to a user.

Examples:

- New message
- Mention
- Organization invite
- Emoji reaction
- System notification

This entity is planned for a future release.

---

# Entity Relationships

```text
User
    │
    ├────────────┐
    │            │
Membership       Notification
    │
Organization
    │
    ├───────────────┐
    │               │
Category       Conversation
                    │
                Message
                    │
              Attachment
```

---

# Future Entities (Not Yet Implemented)

The following entities are being considered but are intentionally excluded from the MVP.

- ConversationMember
- Role
- Permission
- Friend
- Invite
- AuditLog
- RefreshToken