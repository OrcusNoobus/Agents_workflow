# research.md

## What this file is
This file records real technical choices and why they were made. It preserves the reasoning behind decisions that would otherwise vanish into chat history.

**Authored by:** Both. The agent researches options and presents trade-offs. The human makes the final decision. Each entry should show what was considered, what was chosen, and why — so that a future reader (or agent) does not re-debate the same question.

## When to use it
Use this file when there are multiple plausible implementation options and the choice matters enough that future readers should understand it.

## When not to use it
Do not use this file for obvious or default choices that add no learning value. If there is only one reasonable option, just state it in `plan.md` and move on. Avoid turning this file into a generic technology essay.

## Reads from
- `spec.md`
- `clarify.md`
- Global stack rules in `AGENTS.md`

## Feeds into
- `plan.md`
- `tasks.md`
- Future implementation decisions

> Teaching note: this file is not "research for the sake of research". It exists to preserve decisions that would otherwise vanish into chat history. A good entry takes 2 minutes to write and saves 20 minutes of re-discussion next month.

# Decision 1: Use Focused DRF Views Instead Of A Broad ViewSet

## Options considered

Option A: `ModelViewSet`

- Pros: fast to scaffold
- Pros: convenient when full CRUD is needed
- Cons: broader than the current feature requires
- Cons: increases the chance of exposing actions before they are intended

Option B: focused class-based API views

- Pros: each endpoint does one thing
- Pros: easier to map to the feature spec
- Pros: safer for a small educational example
- Cons: more classes to add if the API grows later

## Decision

Use focused DRF views for:

- list tasks
- create task
- complete task

## Reason

This repository is teaching structure and traceability. Narrow views make it easier to connect the spec, contract, and task list to the eventual code.

# Decision 2: Store Ownership On The Task Model

## Options considered

Option A: infer task ownership indirectly through another entity

- Pros: may fit more complex product models
- Cons: unnecessary complexity for a personal task example

Option B: store a direct foreign key to the user

- Pros: simple access control
- Pros: easy to query and test
- Pros: matches the feature's personal ownership rule

## Decision

Store a direct `user` foreign key on `Task`.

# Decision 3: Use A Dedicated Complete Endpoint

## Options considered

Option A: generic update endpoint

- Pros: flexible
- Cons: allows more behavior than the spec promises

Option B: action-style endpoint for completion

- Pros: matches the exact business action
- Pros: keeps the initial contract small

## Decision

Use `PATCH /api/tasks/{id}/complete/` for the first version.

# Things Intentionally Not Decided Yet

- pagination strategy
- sorting strategy
- filtering strategy

These will be decided when a spec requires them. Do not pre-decide — premature decisions create constraints that may not fit future requirements.

> Teaching note: good research documents do not pretend every future decision is already known. Record only the decisions you actually need now. If an agent proposes a decision you have not asked for, push it back to a future `research.md` entry.
