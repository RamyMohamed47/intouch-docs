# InTouch API Reference

> Version: v1
>
> Base URL: `/api/v1`

---

# Authentication

| Method | Endpoint | Description | Auth |
|----------|----------|-------------|------|
| POST | /auth/register | Register a new account | ❌ |
| POST | /auth/login | Login using email/password | ❌ |
| GET | /auth/google | Start Google OAuth | ❌ |
| GET | /auth/google/callback | Google OAuth callback | ❌ |
| POST | /auth/refresh | Refresh access token | ❌ |
| POST | /auth/logout | Logout current session | ✅ |
| GET | /auth/me | Get current user | ✅ |

---

# Organizations

| Method | Endpoint | Description | Auth |
|----------|----------|-------------|------|
| POST | /organizations | Create organization | ✅ |
| GET | /organizations | List my organizations | ✅ |
| GET | /organizations/{organizationId} | Get organization | ✅ |
| PATCH | /organizations/{organizationId} | Update organization | ✅ |
| DELETE | /organizations/{organizationId} | Delete organization | ✅ |

---

# Memberships

| Method | Endpoint | Description | Auth |
|----------|----------|-------------|------|
| POST | /organizations/{organizationId}/invitations | Invite member | ✅ |
| POST | /invitations/{invitationId}/accept | Accept invitation | ✅ |
| POST | /invitations/{invitationId}/decline | Decline invitation | ✅ |
| POST | /organizations/{organizationId}/leave | Leave organization | ✅ |
| DELETE | /organizations/{organizationId}/members/{memberId} | Remove member | ✅ |
| GET | /organizations/{organizationId}/members | List members | ✅ |

---

# Categories

| Method | Endpoint | Description | Auth |
|----------|----------|-------------|------|
| POST | /organizations/{organizationId}/categories | Create category | ✅ |
| GET | /organizations/{organizationId}/categories | List categories | ✅ |
| PATCH | /categories/{categoryId} | Update category | ✅ |
| DELETE | /categories/{categoryId} | Delete category | ✅ |

---

# Conversations

| Method | Endpoint | Description | Auth |
|----------|----------|-------------|------|
| POST | /organizations/{organizationId}/conversations | Create channel or DM | ✅ |
| GET | /organizations/{organizationId}/conversations | List conversations | ✅ |
| GET | /conversations/{conversationId} | Get conversation | ✅ |
| PATCH | /conversations/{conversationId} | Update conversation | ✅ |
| DELETE | /conversations/{conversationId} | Delete conversation | ✅ |

---

# Messages

| Method | Endpoint | Description | Auth |
|----------|----------|-------------|------|
| POST | /conversations/{conversationId}/messages | Send message | ✅ |
| GET | /conversations/{conversationId}/messages | List messages | ✅ |
| PATCH | /messages/{messageId} | Edit message | ✅ |
| DELETE | /messages/{messageId} | Delete message | ✅ |

---

# Response Format

## Success

```json
{
  "success": true,
  "data": {}
}
```

## Error

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Resource not found."
  }
}
```

---

# Authentication

Protected endpoints require:

Authorization: Bearer <access_token>

---

# Pagination

```
?page=1&limit=50
```

---

# Version

Current Version: v1