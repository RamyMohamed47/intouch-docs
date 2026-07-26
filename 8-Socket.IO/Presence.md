
## Overview

Presence indicates whether a user is currently online.

---

# States

- Online
- Offline

Future

- Away
- Do Not Disturb
- Invisible

---

# Presence Updates

Presence changes occur when:

- User connects
- User disconnects

The server broadcasts updates to relevant organization members.

---

# Design Notes

Presence is calculated from active socket connections.

A user remains online while at least one active socket exists.

Only when the final socket disconnects does the user become offline.