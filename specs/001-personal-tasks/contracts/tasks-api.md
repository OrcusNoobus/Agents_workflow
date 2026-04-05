# tasks-api.md

## What this file is
This file defines the public API contract for the personal tasks feature. It is the single source of truth for request and response shapes. If the code produces different output than what this file says, the code is wrong (or this file needs to be updated first).

**Authored by:** Both. The agent drafts the contract based on the spec, clarifications, and data model. The human reviews and approves. Any change to this file must be intentional — it is the interface that consumers depend on.

## When to use it
Use this file when request and response shapes must be exact, especially for APIs or integration points consumed by other teams or frontends.

## When not to use it
Do not create a separate contract file for a tiny internal-only change that has no stable public interface.

## Reads from
- `spec.md`
- `clarify.md`
- `plan.md`
- `data-model.md`

## Feeds into
- `tasks.md`
- Future serializers, views, and contract tests

> Teaching note: contracts remove ambiguity about wire format. They are especially useful because models and internal code can change while the interface must stay stable. An agent should compare its implementation output against this file before marking a task complete.

# Common Rules

- All endpoints require authentication.
- All endpoints use JSON.
- A client cannot choose the `user` field.
- A client cannot directly set `is_completed` during creation.

# Endpoint: List Tasks

## Request

`GET /api/tasks/`

## Success Response

Status: `200 OK`

```json
[
  {
    "id": 1,
    "title": "Buy milk",
    "description": "2 bottles",
    "is_completed": false,
    "created_at": "2026-04-05T10:30:00Z"
  },
  {
    "id": 2,
    "title": "Read docs",
    "description": "",
    "is_completed": true,
    "created_at": "2026-04-05T11:00:00Z"
  }
]
```

## Notes

- Only the current user's tasks appear in the list.
- Owner information is not exposed in the response in v1.

# Endpoint: Create Task

## Request

`POST /api/tasks/`

```json
{
  "title": "Buy milk",
  "description": "2 bottles"
}
```

## Success Response

Status: `201 Created`

```json
{
  "id": 1,
  "title": "Buy milk",
  "description": "2 bottles",
  "is_completed": false,
  "created_at": "2026-04-05T10:30:00Z"
}
```

## Validation Error Example

Status: `400 Bad Request`

```json
{
  "title": [
    "This field is required."
  ]
}
```

## Notes

- `user` is derived from authentication.
- `is_completed` is always `false` at creation time.

# Endpoint: Complete Task

## Request

`PATCH /api/tasks/{id}/complete/`

Request body:

```json
{}
```

## Success Response

Status: `200 OK`

```json
{
  "id": 1,
  "title": "Buy milk",
  "description": "2 bottles",
  "is_completed": true,
  "created_at": "2026-04-05T10:30:00Z"
}
```

## Access Failure Example

If the task does not belong to the current user, the operation must not succeed.

Status: `404 Not Found`

```json
{
  "detail": "Not found."
}
```

The implementation returns `404` instead of `403` to avoid revealing whether a resource exists to other users.

> Teaching note: this sentence belongs in the contract because consumers need to understand failure behavior too, not just the happy path. Documenting error responses is just as important as documenting success responses.

# Endpoint: Authentication Failure

All endpoints return the same response when the request is not authenticated.

Status: `401 Unauthorized`

```json
{
  "detail": "Authentication credentials were not provided."
}
```

> Teaching note: documenting this once here avoids repeating it in every endpoint section.
