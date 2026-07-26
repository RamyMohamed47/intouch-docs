
## Invite Member

POST /organizations/:organizationId/invitations

---

## Accept Invitation

POST /invitations/:invitationId/accept

---

## Decline Invitation

POST /invitations/:invitationId/decline

---

## Leave Organization

POST /organizations/:organizationId/leave

---

## Remove Member

DELETE /organizations/:organizationId/members/:memberId

---

## List Members

GET /organizations/:organizationId/members

---

# Roles

- Owner
- Admin
- Member