# Socket Authentication

Connect with the current access JWT in the handshake:

~~~ts
io(API_ORIGIN, { auth: { accessToken } });
~~~

The server verifies the JWT before `connection`, associates the socket with the authenticated user, joins the user room, and enforces per-user socket limits. Missing, invalid, or expired credentials reject the connection through `connect_error.data`.

The server disconnects sockets when their access token expires. The client refreshes through the appropriate REST browser/mobile flow, updates the socket auth payload, reconnects, then resubscribes to organization and conversation rooms and reconciles TanStack Query data.

Room joins perform fresh membership/participant authorization. A valid token alone never authorizes organization or conversation data.
