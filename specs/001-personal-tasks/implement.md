# implement.md

## What this file is
This file is a runbook for how an AI agent or developer should execute the tasks. It controls execution discipline — not what to build, but how to behave while building it.

**Authored by:** Human writes the execution rules. The agent reads and follows them. If the agent discovers a rule that should be added (e.g., "always run tests after modifying views"), it proposes it; the human approves.

## When to use it
Use this file when implementation begins and you want the execution style itself to be disciplined and repeatable. It is especially valuable when work may be paused and resumed by a different agent session.

## When not to use it
Do not use this file to replace the task list or the spec. It describes execution behavior, not the feature design.

## Reads from
- `AGENTS.md`
- `tasks.md`
- `plan.md`

## Feeds into
- Consistent implementation behavior
- Safer progress tracking
- Easier pause-and-resume work

> Teaching note: `tasks.md` says *what* to do. `implement.md` says *how to behave* while doing it. Without this file, each agent session may work in a different style — some skipping tests, some adding scope, some forgetting to update docs.

# Execution Rules

1. Work through `tasks.md` in order unless a strong reason is documented.
2. Do not skip validation-related tasks.
3. After completing a task, mark it complete in `tasks.md`.
4. If an implementation detail conflicts with the spec, stop and update the docs before continuing.
5. If a real blocker appears, record it clearly instead of improvising around it.

# Scope Control

- Do not add extra endpoints.
- Do not add delete or update behavior.
- Do not add pagination, sorting, or filtering in v1.
- Do not expose the task owner in responses unless the contract changes first.

# Verification Rules

After major milestones, verify:

- the model still matches `data-model.md`
- the response shapes still match `contracts/tasks-api.md`
- the tests still cover the acceptance criteria

# Pause And Resume Rule

If work pauses mid-implementation:

- update `tasks.md` to show current progress
- note any active blocker in `debug.md` if it is a true issue
- leave enough context that another agent can resume without guessing

# Verification Checkpoints

Pause and verify at these milestones (do not skip):

1. **After model + migration:** Run `pytest` to confirm no breakage. Check that the model matches `data-model.md`.
2. **After all three endpoints:** Run the full test suite. Compare response shapes against `contracts/tasks-api.md`.
3. **After all tests pass:** Walk through `quickstart.md` manually (or simulate the steps) to confirm end-to-end behavior.
4. **Before marking the feature complete:** Re-read `spec.md` and confirm every acceptance criterion is met.

# Completion Rule

Stop only when one of these is true:

- all tasks are complete, all tests pass, and manual verification confirms expected behavior
- a real blocking issue prevents further progress and has been documented clearly in `debug.md`
