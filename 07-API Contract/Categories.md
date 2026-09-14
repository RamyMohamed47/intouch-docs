# Categories API

Categories are ordered channel groupings owned by an organization.

| Method | Endpoint | Behavior |
| --- | --- | --- |
| GET | `/organizations/{organizationId}/categories` | List categories visible to a current member. |
| POST | `/organizations/{organizationId}/categories` | Owner creates a category. |
| PATCH | `/organizations/{organizationId}/categories/{categoryId}` | Owner updates name/position. |
| DELETE | `/organizations/{organizationId}/categories/{categoryId}` | Owner deletes when domain constraints permit. |

Category IDs used for channel creation must belong to the same organization. Category authorization is always derived from the organization, never from the ID alone.
