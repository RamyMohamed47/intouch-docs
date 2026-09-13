# Pagination

InTouch uses opaque cursor pagination for high-churn or potentially large collections.

## Common Pattern

~~~http
GET /api/v1/conversations/{conversationId}/messages?before=<opaque-or-resource-cursor>&limit=50
~~~

Responses return the collection plus `nextCursor: string | null`. A null cursor means there is no older/next page. Clients must treat cursors as opaque and must not derive offsets from them.

## Cursor-Paginated Areas

- Message history and exact-context windows.
- Direct-message lists.
- Notification inbox.
- Reaction user lists.
- Search results when one result type is paged.

The accepted cursor representation differs by resource and is defined by each OpenAPI operation. Limits are bounded, commonly 1 to 100. Small configuration and roster collections may return complete arrays and are not forced into a fake universal pagination envelope.
