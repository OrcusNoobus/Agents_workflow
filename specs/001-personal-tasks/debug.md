# debug.md

## What this file is
This file records real bugs, their symptoms, causes, fixes, and prevention steps. It is the project's institutional memory for failures.

**Authored by:** Both. Whoever encounters the bug documents it — human or agent. The format is structured (symptom, reproduction, root cause, fix, prevention) so that future readers can learn from the incident quickly.

## When to use it
Use this file when a real issue appears during implementation, testing, or operation and you want the learning to remain visible. It is also the right place to document blockers during `implement.md` execution.

## When not to use it
Do not use this file as a diary for every tiny detour. If nothing actually failed, this file can stay nearly empty. A `debug.md` with zero entries is a good sign.

## Reads from
- Real incidents
- Test failures
- Manual verification results

## Feeds into
- Better tests
- Better contracts
- Better rules in `AGENTS.md` when the lesson becomes general
- New entries in `clarify.md` when a bug reveals a missing clarification

> Teaching note: `debug.md` is about learning from failure. It is not the main place for stable requirements or rules. The value is in the prevention step — without it, this file is just a bug diary.

# How To Use This File

For each real issue, record:

- what happened
- how to reproduce it
- the root cause
- the fix
- the prevention step

# Example Incident

## Title

Unauthenticated users were able to create tasks

## Date

2026-04-05

## Symptom

`POST /api/tasks/` returned success for anonymous requests.

## Reproduction

1. Start the app.
2. Send `POST /api/tasks/` without authentication.
3. Observe a successful response instead of an auth failure.

## Root Cause

The create view did not enforce authentication permissions.

## Fix

Add the correct authentication permission to the create endpoint and ensure the queryset logic assumes an authenticated user.

## Prevention

- add an automated test for anonymous create attempts
- keep authentication rules explicit in `plan.md` and `contracts/tasks-api.md`

# Promotion Rule

If a lesson is general and likely to repeat, **promote it** out of `debug.md` into a more durable location:

| If the lesson is about... | Promote it to... |
|--------------------------|------------------|
| A missing test case | Add the test |
| A contract mismatch | Update `contracts/` |
| A missing clarification | Add an entry to `clarify.md` |
| A repository-wide pattern | Add a rule to `AGENTS.md` |
| A data model misunderstanding | Update `data-model.md` |

The incident stays here as history, but the **prevention** lives in the system that will actually catch it next time.

> Teaching note: this promotion rule stops `debug.md` from becoming a graveyard of notes that never improve the real system. A debug entry without a promotion step is incomplete.
