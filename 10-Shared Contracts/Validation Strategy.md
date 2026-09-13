# Validation Strategy

Validation occurs at trust boundaries:

- Environment variables at process startup.
- REST params, query strings, bodies, and public responses.
- Socket.IO handshake, commands, acknowledgements, and emitted facts.
- LiveKit webhook raw-body signature plus decoded event shape.
- Provider responses before conversion into domain-safe values.
- Persisted records when mapped to public DTOs.

Services receive normalized inputs but still enforce business invariants and authorization; schema validity is not authorization. Repositories enforce persistence constraints and indexes. Tests cover accepted normalization, unknown-key rejection, malformed data, field stripping, and date serialization.
