
## Define Once

```ts
export const MessageSendSchema = z.object({
  conversationId: z.string(),
  content: z.string().min(1).max(2000)
});
```

---

## Infer Type

```ts
export type MessageSendPayload =
    z.infer<typeof MessageSendSchema>;
```

---

## Backend Validation

```ts
const payload =
    MessageSendSchema.parse(data);
```

---

## Frontend Validation

```ts
MessageSendSchema.parse(data);
```

---

## Socket.IO

```ts
socket.emit(
    "message:send",
    payload
);
```

---

## Benefits

- One schema
- One type
- One validation rule
- Zero duplication