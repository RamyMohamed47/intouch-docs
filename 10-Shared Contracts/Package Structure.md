# Shared Package Structure

`packages/shared` is organized by domain:

~~~text
ai/                 Echo requests, streams, settings, quotas
auth/               browser/mobile authentication contracts
categories/         category inputs and DTOs
chat-wallpapers/    preset and preference contracts
common/             identifiers, dates, errors, health/readiness
conversations/      channels, DMs, participants, receipts
memberships/        roles, presence, invitations
messages/           messages, mentions, replies, reactions
notifications/      inbox, preferences, mutes
organizations/      organization inputs and DTOs
push/               installation/token registration
realtime/           Socket.IO auth, acknowledgements, event maps
search/             organization search requests/results
uploads/            upload tickets, assets, attachment DTOs
users/              safe user/profile DTOs
voice/              sessions, calls, credentials, occupancy
~~~

Each domain exports runtime schemas and inferred types through package subpath exports. Build output under `dist` and local `node_modules` are generated artifacts, not source domains.
