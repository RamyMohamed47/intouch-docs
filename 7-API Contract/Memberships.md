# Memberships and Invitations API

InTouch has two membership roles: `OWNER` and `MEMBER`. There is no `ADMIN` role and no role-change endpoint.

## Routes

- `GET /organizations/{organizationId}/members`: safe member roster plus `ONLINE | OFFLINE` presence.
- `POST /organizations/{id}/invitations`: owner invites an existing registered user by email.
- `GET /invitations`: caller's pending invitations.
- `POST /invitations/{invitationId}/accept`: atomically create membership and consume invitation.
- `DELETE /invitations/{invitationId}`: decline a pending invitation.
- `POST /organizations/{id}/join`: join a public organization.

Private organizations require an invitation. Private conversations additionally require participant access. Ownership transfer, arbitrary role changes, and a general member-removal route are not part of the current public API.
