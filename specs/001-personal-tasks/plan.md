# plan.md

## What this file is
This file is the technical implementation plan for the feature. It turns the product spec into a concrete design that can be executed by an agent or developer.

**Authored by:** Both. The agent drafts the plan based on the spec, clarifications, and research decisions. The human reviews, adjusts, and approves. The plan should be stable before tasks are generated from it.

## When to use it
Use this file when the work touches multiple files, layers, or concerns and you need a stable technical plan before coding. If the agent would need to make architectural decisions while coding, those decisions should be made here first.

## When not to use it
Do not use this file for tiny changes where the implementation is obvious and a task list alone would be enough.

## Reads from
- `AGENTS.md`
- `spec.md`
- `clarify.md`
- `research.md`

## Feeds into
- `data-model.md`
- `contracts/tasks-api.md`
- `tasks.md`
- Future code changes

> Teaching note: if `spec.md` says *what* must happen, `plan.md` says *how* the repository will be shaped so that it can happen. A good plan is specific enough that two different agents would produce similar code from it.

# Feature

Personal tasks for authenticated users in `TaskBox`

# Implementation Summary

The future implementation will add a `Task` model owned by a user, expose three authenticated API endpoints, and add automated tests that prove users can only act on their own tasks.

# Assumed Future File Targets

These files do not exist yet in this scaffold. They are the future code paths this plan expects:

- `src/tasks/models.py`
- `src/tasks/serializers.py`
- `src/tasks/views.py`
- `src/tasks/urls.py`
- `src/config/urls.py`
- `tests/test_tasks_api.py`

> Teaching note: it is fine for a plan to name future files. The important part is to make clear that they are planned targets, not present files.

# Technical Design

## Data layer

- Add a `Task` model with ownership, required title, optional description, completion status, and creation timestamp.
- Enforce the title length limit of 200 characters.
- Default `is_completed` to `false`.

## API layer

- Add an authenticated list endpoint for the current user's tasks.
- Add an authenticated create endpoint that always assigns the owner from `request.user`.
- Add an authenticated complete endpoint that only affects the current user's task.

## Access control

- Every query must be scoped to the authenticated user.
- A task owned by another user must not be writable or readable through these endpoints.

## Testing layer

- Add tests for list visibility by owner.
- Add tests for successful creation.
- Add tests for missing title validation.
- Add tests for authentication requirements.
- Add tests that another user's task cannot be completed.

# Design Constraints

- The implementation must not expose unused CRUD actions.
- The implementation must not allow client-controlled ownership.
- The implementation must match the API contract exactly.
- The implementation must stay aligned with the spec and not add extra product features.

# Risks

- Accidentally exposing the `user` field as writable.
- Forgetting to scope queryset access by `request.user`.
- Allowing generic update behavior that exceeds the spec.

# Validation Checklist

Before generating `tasks.md` from this plan, verify:

- [ ] `spec.md` and `contracts/tasks-api.md` describe the same behavior.
- [ ] `data-model.md` supports the access rules in the spec.
- [ ] `tasks.md` covers all planned work without missing test coverage.
- [ ] Future implementation paths are explicit and unambiguous.
- [ ] No decision in this plan contradicts a resolved question in `clarify.md`.
- [ ] The plan does not add scope beyond what the spec requires.
