
## Purpose

DTOs define data crossing application boundaries.

DTOs are not domain models.

---

## REST DTOs

Examples

- RegisterRequest
- LoginRequest
- CreateOrganizationRequest

---

## Socket DTOs

Examples

- MessageSendPayload
- TypingPayload
- ConversationJoinPayload

---

## Response DTOs

Examples

- UserResponse
- ConversationResponse
- MessageResponse

---

## Rule

Every DTO originates from a Zod schema.

Types are inferred using:

z.infer<typeof Schema>