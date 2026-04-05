# AI Agent Workflow Example Repo

## What this file is
This is the entry point for a human reader. It explains what this repository demonstrates, how to read it, and why the files are organized the way they are.

**Authored by:** Human. The human writes and maintains this file. An agent may suggest structural improvements, but the human owns the content.

## When to use it
Use this file when you are new to the repository, when you want to onboard a teammate, or when you want a quick map before opening the more detailed documents.

## When not to use it
Do not use this file as the source of truth for feature behavior or implementation details. Those belong in the feature-specific files under `specs/`.

## Reads from
- The repository structure itself
- The design choice that this repo is docs-first and educational

## Feeds into
- `AGENTS.md`
- `docs/workflow-map.md`
- `specs/001-personal-tasks/spec.md`

> Teaching note: `README.md` is for humans first. It should help a person or an agent find the right document quickly, not duplicate every rule in the repository.

---

# Authorship Convention

Every file in this repository includes an **"Authored by"** line in its header. This tells you who is responsible for creating and maintaining each document. The three roles are:

| Role | Meaning |
|------|---------|
| **Human** | The human writes this file. The agent should not modify it without being asked. |
| **Agent** | The agent generates this file. The human reviews it but the agent does the heavy lifting. |
| **Both** | The human and agent collaborate. Typically one drafts and the other reviews, or each contributes different sections. The header explains the split. |

This matters because it sets expectations. When you hand a task to an agent, you should know which documents the agent is expected to produce, which ones require your input, and which ones are a conversation between the two of you.

---

# Purpose

This repository is a teaching scaffold for working with AI coding agents in a structured, repeatable way.

It does not contain application code yet.

It contains the documents you would create **before** implementation so that an AI agent can work from stable, explicit instructions instead of a long chat history. The goal is to show that the quality of agent output depends heavily on the quality of the input documents you give it.

# Example Project

The example feature is called `personal tasks`.

The fictional product is `TaskBox`, a simple Django REST API where an authenticated user can:

- list their tasks
- create a task
- mark a task as completed

# Why This Repo Exists

The goal is to make the relationship between the files obvious so that **both humans and agents** can navigate the project without guessing:

| File | Purpose | Authored by |
|------|---------|-------------|
| `AGENTS.md` | Global working rules for agents | Human |
| `spec.md` | What the feature should do | Human |
| `clarify.md` | Resolved ambiguities | Both — agent asks, human answers |
| `research.md` | Technical choices and trade-offs | Both — agent researches, human decides |
| `plan.md` | How the feature will be built | Both — agent drafts, human reviews |
| `data-model.md` | Entity definitions and rules | Both — agent drafts, human validates |
| `contracts/` | Exact interface shapes | Both — agent drafts, human approves |
| `tasks.md` | Small execution steps | Agent generates from plan, human reviews |
| `implement.md` | Agent execution discipline | Human writes rules, agent follows them |
| `quickstart.md` | Manual verification steps | Both — agent drafts, human verifies |
| `debug.md` | Incident history and lessons | Both — whoever encounters the bug documents it |

# Recommended Reading Order

Read the files in this order. Each step builds on the previous one:

| # | File | What you learn |
|---|------|----------------|
| 1 | `README.md` | The map of the whole repo |
| 2 | `AGENTS.md` | Global rules every agent session must follow |
| 3 | `docs/workflow-map.md` | How documents relate and when to use each one |
| 4 | `specs/001-personal-tasks/spec.md` | What the feature should do (product terms) |
| 5 | `specs/001-personal-tasks/clarify.md` | Resolved ambiguities before design |
| 6 | `specs/001-personal-tasks/research.md` | Technical choices and why |
| 7 | `specs/001-personal-tasks/plan.md` | Implementation design |
| 8 | `specs/001-personal-tasks/data-model.md` | Entity rules and lifecycle |
| 9 | `specs/001-personal-tasks/contracts/tasks-api.md` | Exact API shapes |
| 10 | `specs/001-personal-tasks/tasks.md` | Ordered execution checklist |
| 11 | `specs/001-personal-tasks/implement.md` | Agent execution discipline |
| 12 | `specs/001-personal-tasks/quickstart.md` | Manual verification flow |
| 13 | `specs/001-personal-tasks/debug.md` | Incident history and lessons |

> Teaching note: this order follows the logic of software work — rules, behavior, ambiguity removal, technical decisions, planning, execution, verification. It mirrors the spec-driven development methodology used by GitHub Spec Kit, Kiro, and similar frameworks.

# Minimal Workflow vs Full Workflow

Not every task needs every file. The key insight is: **use the smallest set of documents that keeps the work clear and safe.**

## Minimal workflow (4 files)

```
AGENTS.md → spec.md → plan.md → tasks.md
```

Good for:
- a small API endpoint
- a contained UI feature
- a narrow refactor
- a bug fix with clear behavior

## Full workflow (11 files)

```
AGENTS.md → spec.md → clarify.md → research.md → plan.md
→ data-model.md + contracts/ → tasks.md → implement.md
→ quickstart.md → debug.md
```

Good for:
- features that affect multiple layers (model, API, tests)
- work with non-obvious data rules or state transitions
- features with strict API contracts that other teams consume
- tasks with open technical decisions that need to be recorded
- work that may be paused and resumed by a different person or agent session

# Repository Tree

```text
.
├── AGENTS.md
├── README.md
├── docs
│   └── workflow-map.md
└── specs
    └── 001-personal-tasks
        ├── clarify.md
        ├── contracts
        │   └── tasks-api.md
        ├── data-model.md
        ├── debug.md
        ├── implement.md
        ├── plan.md
        ├── quickstart.md
        ├── research.md
        ├── spec.md
        └── tasks.md
```

# Design Principles For This Repo

1. **Keep global rules short and stable.** A giant `AGENTS.md` is harder for agents to hold in context and harder for humans to maintain. ETH Zurich research found that bloated instruction files actually reduce agent performance.
2. **Keep feature decisions close to the feature.** Everything about personal tasks lives under `specs/001-personal-tasks/`, not scattered across root-level files.
3. **One source of truth per kind of information.** If API shape lives in `contracts/`, do not duplicate it in `plan.md`. Duplication leads to drift.
4. **Separate product intent from technical design.** `spec.md` says *what*, `plan.md` says *how*. Mixing them makes both harder to review.
5. **Break implementation into small, testable steps.** Each task in `tasks.md` should map to a small diff and a clear review target.
6. **Make the next file in the chain obvious.** Every file header says what it reads from and what it feeds into.
7. **Provide verification mechanisms.** Agents perform dramatically better when they can check their own work through tests, linters, or manual verification steps.

# Important Boundaries

- This repository is intentionally docs-only. There is no Django project generated yet.
- Paths mentioned in planning documents (e.g. `src/tasks/models.py`) are examples of future implementation targets.
- Tool-specific config such as `.claude/`, `.codex/`, or `.cursor/rules/` is intentionally out of scope in this starter scaffold.

# Cross-Tool Compatibility

The workflow in this repo is **tool-agnostic**. The concepts work with any AI coding agent:

| Tool | How it discovers project rules |
|------|-------------------------------|
| **OpenAI Codex** | Reads `AGENTS.md` from the repo root downward. Supports nested overrides. |
| **Claude Code** | Reads `CLAUDE.md` (or imports `AGENTS.md` via `@AGENTS.md`). Supports a four-level hierarchy: managed policy, project, user, local. |
| **Cursor** | Reads `.cursor/rules/*.mdc` files. Supports glob-scoped rules per directory. |
| **GitHub Copilot** | Reads `.github/copilot-instructions.md` and respects `AGENTS.md`. |
| **Generic** | Any agent that reads repo files can be pointed at `AGENTS.md` or `specs/` directly. |

If your team uses Claude Code, create a `CLAUDE.md` that imports this file:
```markdown
@AGENTS.md
```

> Teaching note: the documents under `specs/` work with all tools. The only tool-specific part is how the agent discovers `AGENTS.md`.

# Sources Behind The Structure

- **OpenAI** — guidance on `AGENTS.md` scope, precedence, and using it as an index to deeper docs
- **GitHub Spec Kit** — the `specify → clarify → plan → tasks → implement` workflow
- **Anthropic** — hierarchical project memory, `CLAUDE.md` best practices, and context management
- **ETH Zurich** — research on the effectiveness (and limits) of instruction files for agents
- **Marmelab** — the "Agent Experience" framework for making codebases agent-friendly
- **Addy Osmani** — AI-augmented engineering workflow and spec-first development

Detailed links are inside `docs/workflow-map.md`.
