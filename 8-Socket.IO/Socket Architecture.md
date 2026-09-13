# Socket.IO Architecture

Socket.IO carries realtime lifecycle facts, not durable message commands or WebRTC signaling.

~~~mermaid
flowchart LR
  Web[Web client] -->|JWT handshake| Gateway[Socket.IO gateway]
  Mobile[Mobile client] -->|JWT handshake| Gateway
  Gateway --> Policy[Membership and conversation policy]
  Gateway --> Redis[(Redis adapter and leases)]
  Services[Application services] -->|after commit| Gateway
  Gateway --> User[user rooms]
  Gateway --> Org[organization rooms]
  Gateway --> Conv[conversation rooms]
~~~

Each authenticated socket joins `user:<userId>`. Clients explicitly subscribe to authorized organization and conversation rooms. Production uses the Redis adapter so rooms and broadcasts work across API replicas.

REST creates/edits/deletes messages and reactions. Socket.IO distributes committed DTOs, scoped activity, invalidations, presence, typing, call lifecycle, and occupancy. LiveKit separately carries media and signaling.
