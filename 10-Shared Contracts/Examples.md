# Shared Contract Examples

## Strict Request Schema

~~~ts
export const createMessageSchema = z.object({
  content: messageContentSchema.optional(),
  uploadIds: z.array(mongoIdSchema).max(5).optional(),
  replyToMessageId: mongoIdSchema.optional(),
  mentions: z.array(messageMentionSchema).max(25).optional(),
}).strict();

export type CreateMessageInput = z.infer<typeof createMessageSchema>;
~~~

## Shared Realtime Maps

~~~ts
socket.emit("conversation:join", { conversationId }, (ack) => {
  if (!ack.success) handleSocketError(ack.error);
});

socket.on("message:created", (message) => {
  messageCoreDtoSchema.parse(message);
});
~~~

## Boundary Parsing

The API validates request input before services and parses service output through response DTOs before `res.json` or Socket.IO emission. Web and mobile parse API responses using the same exported schemas.
