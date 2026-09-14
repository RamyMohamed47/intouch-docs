# API Reference

The canonical contract is [[09-OpenAPI/openapi.yaml|openapi.yaml]]. This generated index reflects 95 operations across 70 path templates. All paths below are relative to `/api/v1`.

## Authentication

| Method | Path | Purpose |
| --- | --- | --- |
| POST | `/auth/register` | Register a new user |
| POST | `/auth/login` | Authenticate user |
| POST | `/auth/mobile/login` | Authenticate a native mobile client |
| POST | `/auth/mobile/google` | Authenticate a native mobile client with Google |
| POST | `/auth/mobile/refresh` | Rotate a native mobile refresh token |
| POST | `/auth/mobile/logout` | Revoke a native mobile refresh session |
| POST | `/auth/verify-email` | Confirm a password account email |
| POST | `/auth/resend-verification` | Request another email-confirmation message |
| POST | `/auth/forgot-password` | Request a password-reset message |
| POST | `/auth/reset-password` | Replace a password using a reset token |
| GET | `/auth/oauth/google` | Start Google OAuth authentication |
| GET | `/auth/oauth/google/callback` | Complete Google OAuth authentication |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/logout` | Revoke the current refresh session |
| GET | `/auth/me` | Get current authenticated user |

## Uploads

| Method | Path | Purpose |
| --- | --- | --- |
| POST | `/uploads` | Create private direct-upload tickets |
| DELETE | `/uploads/{uploadId}` | Cancel an unclaimed upload |
| POST | `/uploads/{uploadId}/complete` | Verify and promote an uploaded object |
| GET | `/assets/{assetId}/access` | Create an authorized private-asset URL |

## Users

| Method | Path | Purpose |
| --- | --- | --- |
| PUT | `/users/me/avatar` | Set the caller's uploaded avatar |
| DELETE | `/users/me/avatar` | Remove the caller's uploaded avatar |
| PUT | `/users/me/push-devices/{installationId}` | Register or rotate this mobile installation's Expo push token |
| DELETE | `/users/me/push-devices/{installationId}` | Remove this mobile installation's push registration |
| GET | `/users/me/notification-preferences` | Get the caller's notification categories and active mutes |
| PUT | `/users/me/notification-preferences` | Replace the caller's notification category preferences |
| PUT | `/users/me/notification-mutes/organizations/{organizationId}` | Mute notification interruption from one workspace |
| DELETE | `/users/me/notification-mutes/organizations/{organizationId}` | Remove the caller's workspace notification mute |
| PUT | `/users/me/notification-mutes/conversations/{conversationId}` | Mute notification interruption from one conversation |
| DELETE | `/users/me/notification-mutes/conversations/{conversationId}` | Remove the caller's conversation notification mute |
| GET | `/users/me/chat-wallpaper` | Get the caller's default chat wallpaper |
| PUT | `/users/me/chat-wallpaper` | Set the caller's default chat wallpaper |

## Organizations

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/organizations` | List organizations for current user |
| POST | `/organizations` | Create organization |
| GET | `/organizations/{id}` | Get organization |
| PATCH | `/organizations/{id}` | Update organization |
| DELETE | `/organizations/{id}` | Delete organization |
| PUT | `/organizations/{id}/logo` | Set an uploaded organization logo |
| DELETE | `/organizations/{id}/logo` | Remove the uploaded organization logo |

## Memberships

| Method | Path | Purpose |
| --- | --- | --- |
| POST | `/organizations/{id}/invitations` | Invite a registered user |
| POST | `/organizations/{id}/join` | Join a public organization |
| GET | `/invitations` | List current user's pending invitations |
| POST | `/invitations/{invitationId}/accept` | Accept a pending invitation |
| DELETE | `/invitations/{invitationId}` | Decline a pending invitation |
| GET | `/organizations/{organizationId}/members` | List organization members and current presence |

## Categories

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/organizations/{organizationId}/categories` | List categories |
| POST | `/organizations/{organizationId}/categories` | Create category |
| PATCH | `/organizations/{organizationId}/categories/{categoryId}` | Update category |
| DELETE | `/organizations/{organizationId}/categories/{categoryId}` | Delete category |

## AI

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/organizations/{organizationId}/ai/settings` | Get organization AI settings and caller consent |
| PUT | `/organizations/{organizationId}/ai/settings` | Enable or disable organization AI |
| PUT | `/organizations/{organizationId}/ai/consent` | Accept the current AI data-use disclosure |
| DELETE | `/organizations/{organizationId}/ai/consent` | Revoke the caller's AI consent |
| POST | `/organizations/{organizationId}/ai/responses` | Stream an authorized AI response |

## Search

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/organizations/{organizationId}/search` | Search an organization |

## Conversations

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/organizations/{organizationId}/conversations` | List channel conversations |
| POST | `/organizations/{organizationId}/conversations` | Create channel conversation |
| GET | `/organizations/{organizationId}/direct-messages` | List the caller's direct messages in an organization |
| POST | `/organizations/{organizationId}/direct-messages` | Create or retrieve a one-to-one direct message |
| GET | `/conversations/{conversationId}` | Get conversation |
| PATCH | `/conversations/{conversationId}` | Update conversation |
| DELETE | `/conversations/{conversationId}` | Delete conversation |
| GET | `/conversations/{conversationId}/chat-wallpaper` | Get the caller's resolved wallpaper for a conversation |
| PUT | `/conversations/{conversationId}/chat-wallpaper` | Set the caller's private conversation wallpaper override |
| DELETE | `/conversations/{conversationId}/chat-wallpaper` | Reset a conversation to the caller's default wallpaper |
| GET | `/conversations/{conversationId}/participants` | List private-channel participants |
| POST | `/conversations/{conversationId}/participants` | Add a private-channel participant |
| DELETE | `/conversations/{conversationId}/participants/{userId}` | Remove a private-channel participant |

## Messages

| Method | Path | Purpose |
| --- | --- | --- |
| PUT | `/conversations/{conversationId}/read-receipt` | Advance the caller's conversation read state |
| GET | `/conversations/{conversationId}/messages` | Get message history |
| POST | `/conversations/{conversationId}/messages` | Create message |
| GET | `/conversations/{conversationId}/messages/{messageId}/context` | Get exact message context |
| GET | `/conversations/{conversationId}/messages/{messageId}/readers` | Summarize readers of the caller's channel message |
| PATCH | `/messages/{messageId}` | Edit message |
| DELETE | `/messages/{messageId}` | Delete message |
| GET | `/messages/{messageId}/reactions` | Get the caller's personalized reaction state |
| PUT | `/messages/{messageId}/reactions/me` | Set or replace the caller's reaction |
| DELETE | `/messages/{messageId}/reactions/me` | Remove the caller's reaction |
| GET | `/messages/{messageId}/reactions/users` | List users who selected an emoji reaction |

## Voice

| Method | Path | Purpose |
| --- | --- | --- |
| POST | `/conversations/{conversationId}/voice/join` | Join an authorized voice channel |
| POST | `/conversations/{conversationId}/voice/participants/{userId}/mute` | Server-mute a voice-channel participant |
| DELETE | `/conversations/{conversationId}/voice/participants/{userId}/screen-share` | Stop a voice-channel participant's current screen share |
| DELETE | `/conversations/{conversationId}/voice/participants/{userId}` | Disconnect a voice-channel participant |
| POST | `/conversations/{conversationId}/calls` | Start a direct-message audio or video call |
| GET | `/voice/sessions/me` | Get the caller's active voice session |
| DELETE | `/voice/sessions/me` | Leave the caller's active voice session |
| POST | `/voice/sessions/me/resume` | Resume the caller's authorized active voice session |
| GET | `/calls/{callId}` | Get an authorized direct-message call |
| POST | `/calls/{callId}/accept` | Accept an incoming call and receive media credentials |
| POST | `/calls/{callId}/decline` | Decline an incoming ringing call |
| POST | `/calls/{callId}/cancel` | Cancel an outgoing ringing call |
| POST | `/calls/{callId}/end` | End an accepted or active call |
| POST | `/integrations/livekit/webhook` | Receive signed LiveKit lifecycle events |

## Notifications

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/notifications` | List the caller's notifications |
| PUT | `/notifications/read-all` | Mark every caller notification as read |
| PUT | `/notifications/{notificationId}/read` | Mark one caller notification as read |

Socket.IO and LiveKit media transport are documented separately; they are not REST operations.
