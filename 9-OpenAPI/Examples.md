# API Examples

Examples are abbreviated; the canonical schemas and status codes remain in `openapi.yaml`.

## Browser Login

~~~http
POST /api/v1/auth/login
Content-Type: application/json

{"email":"member@example.com","password":"correct horse battery staple"}
~~~

~~~json
{
  "user": { "id": "...", "displayName": "Member", "email": "member@example.com" },
  "accessToken": "<short-lived-jwt>"
}
~~~

The response also sets the rotating HttpOnly refresh cookie.

## Create a Text Channel

~~~http
POST /api/v1/organizations/{organizationId}/conversations
Authorization: Bearer <access-token>
Content-Type: application/json

{"categoryId":"<mongo-id>","name":"general","kind":"TEXT","visibility":"PUBLIC"}
~~~

## Send a Reply with a Mention

~~~http
POST /api/v1/conversations/{conversationId}/messages
Authorization: Bearer <access-token>
Content-Type: application/json

{
  "content": "@Ramy please review this",
  "replyToMessageId": "<message-id>",
  "mentions": [{"userId":"<user-id>","start":0,"end":5}]
}
~~~

## Cursor Page

~~~http
GET /api/v1/conversations/{conversationId}/messages?before=<message-id>&limit=50
Authorization: Bearer <access-token>
~~~

~~~json
{"messages":[],"nextCursor":null}
~~~

## Error

~~~json
{"success":false,"error":{"code":"FORBIDDEN","message":"Access denied"}}
~~~
