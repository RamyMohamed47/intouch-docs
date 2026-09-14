# Socket.IO

This section documents realtime authentication, room scope, delivery rules,
event contracts, reconnection, presence, and error handling.

## Reading Order

1. [[08-Socket.IO/Socket Architecture|Socket Architecture]]
2. [[08-Socket.IO/Authentication|Authentication]]
3. [[08-Socket.IO/Rooms|Rooms]]
4. [[08-Socket.IO/Socket Events|Socket Events]]
5. [[08-Socket.IO/Reconnection|Reconnection]]
6. [[08-Socket.IO/Error Handling|Error Handling]]

Socket.IO distributes authorized lifecycle facts and cache invalidations. REST
services remain authoritative for durable writes, and LiveKit carries media
and WebRTC signaling separately.
