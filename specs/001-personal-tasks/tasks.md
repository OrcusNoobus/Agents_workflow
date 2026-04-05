# tasks.md

## What this file is
This file is the execution checklist for the feature. It breaks the plan into small, ordered, reviewable tasks. Each task should map to a small diff and a clear review target.

**Authored by:** Agent generates the task list from the plan, data model, and contracts. Human reviews and may reorder, split, or remove tasks. During implementation, the agent marks tasks complete as it finishes them.

## When to use it
Use this file when an agent or developer is ready to implement and needs a clear sequence of work. This is the file the agent works through during `implement.md` execution.

## When not to use it
Do not use this file as a replacement for a spec or design. If the tasks look vague, the earlier documents are probably incomplete — go back and fix them first.

## Reads from
- `plan.md`
- `data-model.md`
- `contracts/tasks-api.md`

## Feeds into
- `implement.md`
- Future implementation work
- Progress tracking (the checkboxes show where work stands)

> Teaching note: a good `tasks.md` should let a reviewer understand what will change and in what order, without having to reverse-engineer the whole plan. If a task says only "implement feature", it is too large. Each task should be completable in a single focused session.

# Traceability Rule

Each task below exists because of something already defined earlier:

- tasks about behavior come from `spec.md`
- tasks about structure come from `plan.md`
- tasks about fields come from `data-model.md`
- tasks about wire format come from `contracts/tasks-api.md`

# Ordered Task List

- [ ] Create the `Task` model in `src/tasks/models.py` with fields and defaults from `data-model.md`.
- [ ] Create and review the migration for the `Task` model.
- [ ] Add task serializers in `src/tasks/serializers.py` that match `contracts/tasks-api.md`.
- [ ] Add a list endpoint in `src/tasks/views.py` scoped to the authenticated user.
- [ ] Add a create endpoint in `src/tasks/views.py` that assigns ownership from `request.user`.
- [ ] Add a complete endpoint in `src/tasks/views.py` for `PATCH /api/tasks/{id}/complete/`.
- [ ] Register task routes in `src/tasks/urls.py`.
- [ ] Include task routes in `src/config/urls.py`.
- [ ] Add list-visibility tests in `tests/test_tasks_api.py`.
- [ ] Add create-success and missing-title tests in `tests/test_tasks_api.py`.
- [ ] Add authentication-required tests for all task endpoints.
- [ ] Add access-control tests proving one user cannot complete another user's task.
- [ ] Run the full task API test suite.
- [ ] Update `quickstart.md` if the final implementation details differ from the current plan.

# Review Heuristics

Before marking a task complete, check:

- Does this step still match the spec?
- Does this step keep the contract stable?
- Does this step add more scope than planned?

> Teaching note: if a task says only "implement feature", it is too large. A good task should map to a small diff and a clear review target.
