# data-model.md

## What this file is
This file defines the important entities, fields, rules, and state transitions for the feature. It is the single source of truth for what the data looks like and how it behaves.

**Authored by:** Both. The agent drafts the entity definition based on the spec, clarifications, and plan. The human validates that the fields, rules, and lifecycle are correct. If the agent invents a field not mentioned in the spec, it should be flagged here for human approval.

## When to use it
Use this file when the shape of the data matters enough that it deserves its own source of truth — especially when fields, constraints, or state transitions could be misunderstood.

## When not to use it
Do not use this file for a trivial one-field change if `plan.md` already covers everything clearly. Skip it for features with no meaningful data structure.

## Reads from
- `spec.md`
- `clarify.md`
- `plan.md`

## Feeds into
- `contracts/tasks-api.md`
- `tasks.md`
- Future models, serializers, and tests

> Teaching note: this file helps an agent avoid inventing fields, ignoring validation rules, or forgetting entity lifecycle behavior. Without it, agents tend to add extra fields "just in case" — which is scope creep.

# Entity: Task

## Purpose

Represents one personal task owned by exactly one authenticated user.

## Fields

- `id`: integer primary key
- `user`: foreign key to the user model, required
- `title`: string, required, max length 200
- `description`: text, optional, blank allowed
- `is_completed`: boolean, required, default `false`
- `created_at`: datetime, auto-generated on creation

# Ownership Rules

- Every task belongs to exactly one user.
- A task cannot exist without an owner.
- Ownership is assigned by the server, not the client.

# Validation Rules

- `title` is required.
- `title` must not exceed 200 characters.
- `description` may be empty.
- `is_completed` begins as `false` on creation.

# Lifecycle

Initial state:

- a new task is incomplete

Allowed state change in v1:

- incomplete -> completed

Not supported in v1:

- completed -> incomplete (no undo)
- delete flow
- archived state

```
                  ┌───────────┐
  create ──────►  │ incomplete │
                  └─────┬─────┘
                        │ PATCH .../complete/
                        ▼
                  ┌───────────┐
                  │ completed  │
                  └───────────┘
```

> Teaching note: state rules belong here because they are rules about the entity itself, not just about one endpoint. The diagram above makes the lifecycle immediately visible to both humans and agents.
