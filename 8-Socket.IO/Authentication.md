# Socket Authentication

## Overview

Sockets are authenticated using JWT access tokens.

Authentication occurs immediately after establishing the socket connection.

---

# Authentication Flow

Client connects.

↓

Client emits

auth:authenticate

↓

Server validates JWT.

↓

Server associates socket with authenticated user.

↓

Socket becomes authorized.

---

# Authorization

Authentication identifies the user.

Authorization determines what resources the socket may access.

Every room join request verifies:

- Organization membership
- Conversation access

---

# Authentication Failure

Invalid authentication results in:

- Error event
- Connection termination