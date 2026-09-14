# OpenAPI Authentication

Protected operations use the global HTTP Bearer scheme:

~~~yaml
bearerAuth:
  type: http
  scheme: bearer
  bearerFormat: JWT
~~~

Public operations explicitly set `security: []`. These include registration/login/action-token endpoints, browser Google OAuth start/callback, browser refresh/logout (cookie and CSRF protected), native mobile login/Google/refresh/logout, liveness/readiness, documentation assets, and the signed LiveKit webhook.

Browser refresh credentials are cookies and therefore are not represented as Bearer tokens. Native mobile refresh credentials are strict request-body fields. All protected organization, message, asset, search, Echo, notification, push-device, and voice routes require the access JWT.
