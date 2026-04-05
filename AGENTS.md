# AGENTS.md

## What this file is
This file contains the global rules for how an AI coding agent should work in this repository. It is the top-level rulebook that applies to every session.

**Authored by:** Human. The human writes and maintains global rules. The agent reads and follows them. If an agent discovers a rule that should be global, it proposes it here; the human approves.

## When to use it
Use this file at the start of every session that touches this repository. Read it before doing anything else.

## When not to use it
Do not turn this file into a full design document or feature spec. If a rule is specific to one feature, put it in that feature's folder under `specs/`. Keep this file under 200 lines — agents lose adherence to long instruction files.

## Reads from
- Stable team preferences
- Stable repository-wide conventions
- The decision to keep this repo docs-first at this stage

## Feeds into
- Every future implementation task
- `docs/workflow-map.md`
- All files under `specs/`

> Teaching note: `AGENTS.md` should stay short enough that an agent can reliably keep it in context. Research from ETH Zurich shows that bloated instruction files actually reduce agent performance. Think of it as the law of the land, not the full history book.

# Cross-Tool Note

This file follows the open `AGENTS.md` format supported by Codex, Copilot, and many other tools. If you use Claude Code, create a `CLAUDE.md` that imports it: `@AGENTS.md`. If you use Cursor, reference it from `.cursor/rules/`. The content is tool-agnostic.

# Repository Purpose

This repository teaches a structured, repeatable workflow for working with AI coding agents.

The current stage is documentation only.

There is no application code yet. The documents describe a future Django REST API example called `TaskBox`.

# Tech Stack For The Example

- Python 3.12
- Django 5.x
- Django REST Framework
- PostgreSQL 16
- Docker Compose
- Pytest

> Teaching note: the stack belongs here because it is global. If one feature uses a special library with special constraints, that belongs in `research.md` or `plan.md` for that feature.

# Mandatory Working Rules

- Use `python`, `pytest`, and Docker commands as documented in this repository.
- Prefer explicit, simple solutions over broad abstractions. Three similar lines of code is better than a premature abstraction.
- Keep business rules out of views when implementation begins. Views should delegate to models or service functions.
- Keep request and response shapes aligned with the contract documents. If the code drifts from the contract, update the contract first.
- Do not invent behavior that is not present in the feature spec. Scope creep from agents is the most common failure mode.
- Verify your own work. Run tests after every meaningful change. If a manual verification step exists in `quickstart.md`, use it.

# Safety Boundaries

Always:
- Read the relevant spec and contract before modifying feature code.
- Run existing tests before and after your changes.
- Commit in small, reviewable increments.

Never:
- Delete files unless explicitly asked.
- Run destructive Git commands (force push, hard reset).
- Add dependencies without documenting the reason in the relevant feature docs.
- Silently change public API behavior without updating the contract.
- Treat `debug.md` as a substitute for tests or contracts.
- Skip authentication or permission checks to "make things work faster".

# Definition Of Done

A feature is complete only when **all** of the following are true:

- [ ] The implementation matches `spec.md` — no missing behavior, no extra behavior.
- [ ] Ambiguous points are resolved in `clarify.md` or the spec itself.
- [ ] Technical choices are reflected in `plan.md`.
- [ ] Request and response shapes match `contracts/` exactly.
- [ ] All items in `tasks.md` are marked complete.
- [ ] Automated tests pass (both new tests and existing suite).
- [ ] Manual verification steps in `quickstart.md` are still valid.
- [ ] No unresolved blockers remain in `debug.md`.

# Preferred Document Flow

Follow this order unless the task is truly trivial:

1. `spec.md`
2. `clarify.md`
3. `research.md`
4. `plan.md`
5. `data-model.md`
6. `contracts/`
7. `tasks.md`
8. `implement.md`
9. `quickstart.md`
10. `debug.md` when real issues appear

> Teaching note: not every task needs every file. Use the smallest set that keeps the work clear and safe.

# Project Map

- `README.md` explains the repository and reading order.
- `docs/workflow-map.md` explains relationships between files.
- `specs/` contains feature-scoped documentation.
- `specs/001-personal-tasks/` contains the complete example feature package.

# Minimal Workflow Rule

For small work, the minimum acceptable document set is:

- `AGENTS.md`
- `spec.md`
- `plan.md`
- `tasks.md`

# File Ownership By Intent

Each file owns exactly one kind of truth. Do not duplicate information across files — that is how documentation drift starts.

| File | Owns | Authored by |
|------|------|-------------|
| `spec.md` | Desired behavior | Human |
| `clarify.md` | Resolved ambiguities | Both — agent asks, human answers |
| `research.md` | Technical trade-offs | Both — agent researches, human decides |
| `plan.md` | Implementation design | Both — agent drafts, human reviews |
| `data-model.md` | Entity rules | Both — agent drafts, human validates |
| `contracts/` | Interface shape | Both — agent drafts, human approves |
| `tasks.md` | Execution order | Agent generates, human reviews |
| `implement.md` | Agent execution discipline | Human writes, agent follows |
| `quickstart.md` | Manual verification steps | Both — agent drafts, human tests |
| `debug.md` | Incident history | Both — whoever hits the bug writes it up |

> Teaching note: if the same fact appears in too many files, they will drift apart. Pick one source of truth for each kind of information.

# Commands For The Future Example Project

These commands are examples for the future codebase that this scaffold describes:

- `docker compose up --build`
- `docker compose exec web python manage.py migrate`
- `docker compose exec web pytest`

Because this repository is docs-only right now, these commands are not meant to run yet.

# External References

- AGENTS.md open format (Linux Foundation): `https://agents.md/`
- GitHub — how to write a great `AGENTS.md`: `https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/`
- OpenAI Codex — custom instructions with `AGENTS.md`: `https://developers.openai.com/codex/guides/agents-md`
- Anthropic — Claude Code best practices: `https://code.claude.com/docs/en/best-practices`
- Anthropic — hierarchical project memory: `https://code.claude.com/docs/en/memory`
- GitHub Spec Kit: `https://github.com/github/spec-kit`
- Marmelab — Agent Experience framework: `https://marmelab.com/blog/2026/01/21/agent-experience.html`
