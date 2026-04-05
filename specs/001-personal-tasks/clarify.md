# clarify.md

## What this file is
This file records important ambiguities that were resolved before implementation planning. It exists to prevent the agent from making silent assumptions that lead to wrong implementations.

**Authored by:** Both. The agent raises questions it encounters while reading the spec (or the human anticipates them). The human provides definitive answers. The format is Q&A: the agent asks, the human resolves.

## When to use it
Use this file when the spec leaves room for multiple valid interpretations and those interpretations would change design or behavior. If two reasonable developers would read the spec differently, the ambiguity belongs here.

## When not to use it
Do not create this file just to repeat the spec. If there is no real ambiguity, skip it. A `clarify.md` that only restates existing facts adds noise without value.

## Reads from
- `spec.md`
- Questions raised during planning

## Feeds into
- `plan.md`
- `data-model.md`
- `contracts/tasks-api.md`
- `tasks.md`

> Teaching note: this file exists to stop the agent from guessing. It is cheaper to answer one question here than to unwind a wrong implementation later. Anthropic recommends an "interview-first" pattern where you have the agent ask you probing questions before it starts building.

# Resolved Questions

## Q1. Is `description` required?

No. `description` is optional when creating a task.

## Q2. Can a client set the task owner explicitly?

No. The owner is always derived from the authenticated user.

## Q3. Is `title` unique per user?

No. Multiple tasks may share the same title.

## Q4. Is there a title length limit?

Yes. `title` has a maximum length of 200 characters.

## Q5. What happens if a user tries to complete another user's task?

The operation must fail. The task should not be modified, and the API should behave as if that task is not accessible to the current user.

## Q6. Should listing tasks return completed and incomplete tasks together?

Yes. The first version returns all of the current user's tasks in one list.

## Q7. Should creating a task allow `is_completed = true` in the request?

No. New tasks always start incomplete. The client should not be able to create a task directly in the completed state.

# How To Add New Questions

When an agent or human encounters a new ambiguity:

1. Add a new `## Q{N}` entry at the end of the resolved questions section.
2. State the question clearly.
3. Provide a definitive answer.
4. If the answer changes the contract or data model, update those files too.

The goal is that this file grows incrementally as edge cases surface — not all at once.

# Notes For Future Changes

- If later versions add filtering or sorting, update `spec.md` first and then revisit the contracts.
- If ownership rules change, update this file and the contract together.
- If a bug reveals a missing clarification, add a new entry here and reference it from `debug.md`.
