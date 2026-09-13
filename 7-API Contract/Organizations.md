# Organizations API

Organizations are tenant roots with `PRIVATE | PUBLIC` visibility, one owner, memberships, categories, conversations, assets, notification scopes, search, and Echo policy.

| Method | Endpoint | Behavior |
| --- | --- | --- |
| GET/POST | `/organizations` | List caller organizations or create one. |
| GET/PATCH/DELETE | `/organizations/{id}` | Read, owner-update, or owner-delete. |
| PUT/DELETE | `/organizations/{id}/logo` | Claim or remove a private uploaded logo. |
| POST | `/organizations/{id}/join` | Join a public organization. |
| POST | `/organizations/{id}/invitations` | Owner invites a registered user. |
| GET | `/organizations/{id}/search` | Access-filtered messages/channels/people search. |
| GET/PUT | `/organizations/{id}/ai/settings` | Read or owner-toggle Echo. |

Organization creation and deletion use transactions. Deletion coordinates child records and post-commit cleanup for Redis state, media participants, jobs, and private assets. External logo URLs are unsupported; logos use the upload/asset lifecycle.
