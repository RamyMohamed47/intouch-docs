# Error Responses

Every REST error has this strict shape:

~~~json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed"
  }
}
~~~

Common codes include `VALIDATION_ERROR`, `UNAUTHORIZED`, `FORBIDDEN`, `NOT_FOUND`, `CONFLICT`, `TOO_MANY_REQUESTS`, `EMAIL_VERIFICATION_REQUIRED`, `INVALID_OR_EXPIRED_TOKEN`, `SEARCH_UNAVAILABLE`, `STORAGE_UNAVAILABLE`, `SERVICE_UNAVAILABLE`, voice conflict/unavailability codes, and Echo consent/provider codes.

| Status | Meaning |
| --- | --- |
| 400 | Malformed body or strict contract validation failure. |
| 401 | Missing, invalid, or expired authentication. |
| 403 | Authenticated but not authorized or verification/consent required. |
| 404 | Authorized resource lookup failed. |
| 409 | State conflict, active/busy voice session, or capacity conflict. |
| 429 | Rate or quota limit. Respect `Retry-After` when supplied. |
| 500 | Sanitized unexpected failure. |
| 503 | Critical dependency or provider unavailable. |

Clients branch on `error.code`, not the human-readable message. Validation internals, provider responses, credentials, and stack traces are never returned.
