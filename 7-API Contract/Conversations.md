
## Create Conversation

POST /organizations/:organizationId/conversations

Conversation Types:

- CHANNEL
- DM

---

## Get Conversations

GET /organizations/:organizationId/conversations

---

## Get Conversation

GET /conversations/:conversationId

---

## Update Conversation

PATCH /conversations/:conversationId

---

## Delete Conversation

DELETE /conversations/:conversationId

---

# Notes

Channels may belong to Categories.

DMs do not belong to Categories.

Users may only create DMs with members of the same organization.