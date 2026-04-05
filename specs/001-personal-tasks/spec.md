# spec.md

## What this file is
This file defines the feature in product terms. It says what the system should do and why it should do it. It is the single source of truth for desired behavior.

**Authored by:** Human. The human defines product requirements and acceptance criteria. The agent reads this file to understand what to build — it should never modify it without explicit approval.

## When to use it
Use this file when behavior changes, when a new feature is introduced, or when you need clear acceptance criteria before design starts. This is the first feature file the agent should read.

## When not to use it
Do not use this file to describe serializers, database tables, view classes, or exact implementation steps. Those belong in `plan.md`, `data-model.md`, and `contracts/`. The spec says *what*, not *how*.

## Reads from
- `AGENTS.md`
- The product idea for the feature

## Feeds into
- `clarify.md`
- `plan.md`
- `contracts/tasks-api.md`
- `tasks.md`

> Teaching note: if a sentence starts with "the user should be able to..." or "the system must...", it probably belongs here. If it starts with "use a ModelViewSet" or "add a foreign key", it belongs in `plan.md` or `data-model.md`.

# Feature

Personal tasks for authenticated users in `TaskBox`

# Goal

Allow a signed-in user to manage a private list of personal tasks through a simple API.

# User Story

As an authenticated user, I want to list my tasks, create a new task, and mark one of my tasks as completed so that I can keep track of my own work.

# Scope

In scope:

- list only the current user's tasks
- create a task with a required title and optional description
- mark an existing task as completed

Out of scope:

- editing the title or description after creation
- deleting tasks
- shared tasks between users
- task priorities, due dates, labels, or attachments

# Functional Requirements

1. The system must provide an endpoint to list the authenticated user's tasks.
2. The system must provide an endpoint to create a task for the authenticated user.
3. The system must provide an endpoint to mark one of the authenticated user's tasks as completed.
4. A task must have a required `title`.
5. A task may have an optional `description`.
6. A new task must start with `is_completed = false`.
7. A user must never be able to list, create for, or complete another user's tasks.
8. All responses must use JSON.

# Non-Functional Requirements

- Validation errors must be explicit.
- Authentication must be required on all task endpoints.
- The API shape must stay small and easy to reason about.
- Automated tests must cover the happy path and key failure cases.

# Acceptance Criteria

## List tasks

- `GET /api/tasks/` returns `200 OK` for an authenticated user.
- The response includes only tasks that belong to the current user.
- The response does not include another user's tasks.

## Create task

- `POST /api/tasks/` with valid data returns `201 Created`.
- The created task belongs to the authenticated user.
- Missing `title` returns `400 Bad Request`.
- The client cannot choose the `user` field directly.

## Complete task

- `PATCH /api/tasks/{id}/complete/` returns `200 OK` for the owner's task.
- The response shows `is_completed = true`.
- Completing another user's task must not succeed.

# Success Definition

The feature is successful when a future implementation can satisfy all acceptance criteria without inventing extra behavior and without missing any stated requirement.

> Teaching note: success is about observable behavior, not about whether a specific class or framework feature was used. If an agent adds pagination, sorting, or extra fields "because it seemed useful", that is scope creep — even if the code is correct. The spec is a boundary, not a suggestion.
