# OpenAPI Error Responses

Reusable error responses reference the strict shape:

~~~json
{
  "success": false,
  "error": {
    "code": "TOO_MANY_REQUESTS",
    "message": "Too many requests"
  }
}
~~~

The shared error-code enum covers generic HTTP failures plus verification, token, search, storage, service, voice, and Echo conditions. Endpoint-specific response sections document which statuses are expected.

OpenAPI examples must not invent `details`, a universal `data` envelope, or `RATE_LIMIT_EXCEEDED`; those are not current shared contracts. The active rate-limit code is `TOO_MANY_REQUESTS`.
