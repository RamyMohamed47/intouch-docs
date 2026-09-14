# Database Entities

MongoDB is the durable source of truth. The canonical field-level model and indexes are in [[ERD|ERD]].

## Identity and Delivery

| Entity | Responsibility |
| --- | --- |
| `User` | Public profile, password state, verification status, and external avatar fallback. |
| `LoginProvider` | Verified Google identity linked by provider subject. |
| `AuthSession` | Hashed rotating refresh session. |
| `AuthActionToken` | Hashed single-use verification/reset token. |
| `MailOutbox` | Encrypted transactional-email intent with leases and retry state. |

## Organizations and Conversations

| Entity | Responsibility |
| --- | --- |
| `Organization` | Tenant root, visibility, owner, and optional private logo asset. |
| `Membership` | Unique user/organization relationship with `OWNER | MEMBER`. |
| `Invitation` | Expiring invitation to a registered user. |
| `Category` | Ordered organization channel grouping. |
| `Conversation` | `CHANNEL | DIRECT`; channel kind is `TEXT | VOICE` and visibility is public/private. |
| `ConversationParticipant` | Explicit private-channel and direct-message access. |

## Messaging and Calls

| Entity | Responsibility |
| --- | --- |
| `Message` | `TEXT | ATTACHMENT | CALL`, caption, reply target, mention metadata, edit/redaction timestamps. |
| `MessageReaction` | One normalized emoji reaction per user/message. |
| `ConversationReadState` | Per-user high-water read position. |
| `CallSession` | Durable DM call mode, status, terminal reason, timestamps, and timeline-message link. |

Reply previews and call summaries are hydrated views, not duplicated authoritative message records.

## Preferences, Notifications, and Assets

| Entity | Responsibility |
| --- | --- |
| `Notification` | Durable invitation, DM, reaction, mention, and reply activity. |
| `NotificationPreference` | User category switches with all-enabled defaults. |
| `NotificationMute` | Timed or indefinite organization/conversation mute. |
| `PushDevice` | Encrypted Expo push token per installation. |
| `PushOutbox` | Durable ordinary push-delivery intent. |
| `CallAlertOutbox` | Short-lived incoming-call push intent. |
| `ChatWallpaperPreference` | Default or conversation-specific preset and dimming. |
| `StoredAsset` | Private R2 object ownership, purpose, lifecycle, signature, and claim state. |
| `UploadDailyUsage` | Per-user daily upload quota accounting. |

## Persistence Rules

- Multi-document invariants use MongoDB transactions and therefore require a replica set or sharded cluster.
- Soft/redacted message deletion preserves timeline ordering while removing content and attachment claims.
- Provider room IDs, Redis leases, presigned URLs, credentials, and raw refresh/action tokens are not public DTO fields.
- Existing records rely on safe defaults for later-added fields such as channel kind, mentions, replies, and notification preferences.
