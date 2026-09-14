# Database

This section documents MongoDB-owned durable state, entity relationships,
indexes, and transaction boundaries.

## Contents

- [[04-Database/Entities|Entities]] provides the human-readable entity map.
- [[90-Engineering Context/database/ERD|Canonical ERD Mirror]] contains the
  field-level model mirrored from `.agents/database/ERD.md`.
- [[Diagrams/Entity Diagrams|Entity Diagrams]] provides visual context.

MongoDB is authoritative for durable domain data. Redis is deliberately limited
to distributed ephemeral state and BullMQ coordination; see
[[13-Infrastructure/Redis Runtime State|Redis Runtime State]].
