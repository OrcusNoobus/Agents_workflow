# Workflow Map

## What this file is
This file explains how the documents relate to each other and how work moves from idea to implementation. It is the architectural guide for the documentation system itself.

**Authored by:** Human writes the structure. Agent may suggest improvements after using the workflow in practice.

## When to use it
Use this file when you want to understand the system as a whole, when you need to decide which files to create for a task, or when you want to explain the workflow to another person or agent.

## When not to use it
Do not use this file as the source of truth for feature requirements or API details. It is a map, not a specification.

## Reads from
- `README.md`
- `AGENTS.md`
- The feature package under `specs/001-personal-tasks/`

## Feeds into
- Better file selection for future tasks
- Consistent use of `specs/` folders
- Onboarding for humans and agents

> Teaching note: this file answers "why does this structure exist?" and "which document comes next?" That is different from "what does the feature do?"

# Core Idea

Good agent workflows separate **durable knowledge** from **temporary conversation**.

Chat messages vanish between sessions. Documents persist. The quality of agent output depends on the quality of the documents you give it — not the length of your chat history.

This repository uses three layers:

```
Layer 1: Global Rules          → AGENTS.md
Layer 2: Feature Truth         → specs/<feature>/*
Layer 3: Verification & Learning → quickstart.md, tests, debug.md
```

# Minimal Workflow

Use the minimal workflow when the task is small, obvious, and low-risk.

Required files:

- `AGENTS.md`
- `spec.md`
- `plan.md`
- `tasks.md`

## Example use cases

- add a small endpoint
- fix a contained validation bug
- add one field to an existing serializer

# Full Workflow

Use the full workflow when the task has non-obvious behavior, cross-file impact, or real design choices.

Recommended files:

- `AGENTS.md`
- `spec.md`
- `clarify.md`
- `research.md`
- `plan.md`
- `data-model.md`
- `contracts/`
- `tasks.md`
- `implement.md`
- `quickstart.md`
- `debug.md`

## Example use cases

- a new feature touching model, API, and tests
- a feature with role or permission rules
- an integration with strict request and response shapes
- work that another agent may continue later

# Relationship Between Files

The relationship is directional. Each file reads from the ones above it and feeds into the ones below it:

```
                    AGENTS.md
                       │
                       ▼
                    spec.md          ← Human defines intent
                       │
                       ▼
                   clarify.md        ← Both resolve ambiguity
                       │
                       ▼
                  research.md        ← Both explore options
                       │
                       ▼
                    plan.md          ← Both design implementation
                     │   │
                     ▼   ▼
           data-model.md  contracts/ ← Both define structures
                     │   │
                     ▼   ▼
                   tasks.md          ← Agent generates checklist
                       │
                       ▼
                  implement.md       ← Human sets execution rules
                       │
                       ▼
                  quickstart.md      ← Both write verification
                       │
                       ▼
                   debug.md          ← Both record incidents
```

## Why this direction matters

Each step builds on the previous one. If you skip a step, the documents below it will be weaker:

- **`AGENTS.md`** gives stable global boundaries. Without it, every session reinvents conventions.
- **`spec.md`** defines the intended behavior. Without it, the agent guesses what to build.
- **`clarify.md`** removes guesswork before design starts. Without it, the agent makes silent assumptions.
- **`research.md`** records real technical choices. Without it, another agent might re-debate the same options.
- **`plan.md`** turns the feature into an implementation design. Without it, the agent generates code structure on the fly.
- **`data-model.md`** and **`contracts/`** make important structures explicit. Without them, field names and API shapes drift.
- **`tasks.md`** turns the plan into small execution chunks. Without it, the agent tries to do everything at once.
- **`implement.md`** controls execution discipline. Without it, the agent may add scope or skip verification.
- **`quickstart.md`** lets a person verify the result manually. Without it, you rely entirely on automated tests.
- **`debug.md`** preserves lessons from real failures. Without it, the same bugs recur.

# Source Of Truth Rules

Each kind of truth should live in one main place:

- Product intent lives in `spec.md`.
- Resolved ambiguity lives in `clarify.md`.
- Technical trade-offs live in `research.md`.
- Implementation structure lives in `plan.md`.
- Entity and state rules live in `data-model.md`.
- API shape lives in `contracts/`.
- Execution order lives in `tasks.md`.

> Teaching note: if an agent changes API output, the contract must change. If an agent changes desired behavior, the spec must change. This is how you avoid documentation drift.

# When To Create Each File

Create `spec.md` when the task changes behavior.

Create `clarify.md` when the prompt leaves important questions open.

Create `research.md` when there are meaningful technical options.

Create `plan.md` when the task touches multiple files or layers.

Create `data-model.md` when entities, state, or validation rules matter.

Create `contracts/` when interface shape must be exact.

Create `tasks.md` almost always, unless the task is so small that a plan would only have one step.

Create `implement.md` when you want a reusable execution runbook for the agent.

Create `quickstart.md` when the feature benefits from repeatable manual verification.

Create `debug.md` when you hit a real bug worth remembering.

# When Not To Create Each File

Skip `clarify.md` if the requirements are already explicit.

Skip `research.md` if the implementation path is obvious and there are no real trade-offs.

Skip `data-model.md` if the feature has no meaningful data structure beyond what already exists in `plan.md`.

Skip `contracts/` if there is no public interface to protect.

Skip `implement.md` for tiny tasks where `tasks.md` is enough.

Skip `quickstart.md` for very small internal changes with no useful manual flow.

Skip `debug.md` if nothing actually failed.

# Example Trace For This Repo

This repository's example feature follows this exact chain:

1. `specs/001-personal-tasks/spec.md` says an authenticated user can list, create, and complete personal tasks.
2. `specs/001-personal-tasks/clarify.md` resolves open questions like title length and ownership behavior.
3. `specs/001-personal-tasks/research.md` explains why the example uses focused DRF views instead of a broad viewset.
4. `specs/001-personal-tasks/plan.md` describes the future implementation shape.
5. `specs/001-personal-tasks/data-model.md` defines the `Task` entity.
6. `specs/001-personal-tasks/contracts/tasks-api.md` fixes the endpoint shapes.
7. `specs/001-personal-tasks/tasks.md` turns the design into small, ordered tasks.
8. `specs/001-personal-tasks/implement.md` tells the agent how to execute those tasks.
9. `specs/001-personal-tasks/quickstart.md` explains how a human would verify behavior.
10. `specs/001-personal-tasks/debug.md` demonstrates how to document a bug after implementation.

# Why `AGENTS.md` Stays Short

Multiple sources — OpenAI, Anthropic, and ETH Zurich research — converge on the same advice: keep your top-level instruction file short and use it as an index into deeper documents.

That matters because a giant rule file:

- is harder for an agent to hold in context (LLM context is finite)
- is harder to maintain (changes get lost in noise)
- tends to mix stable rules with temporary decisions
- can actually reduce agent performance (ETH Zurich found auto-generated instruction files caused a 3% drop in success rate and 20%+ increase in costs)

This repository follows that advice:

- root-level durable rules stay in `AGENTS.md` (under 200 lines)
- feature-level truth stays under `specs/`
- each file has a narrow, well-defined purpose

> Teaching note: Anthropic recommends asking yourself for every line in your instruction file: "Would removing this cause the agent to make mistakes?" If not, cut it.

# Tips For Making Your Codebase Agent-Friendly

Beyond the document structure, these practices help agents work more effectively (based on the Marmelab "Agent Experience" framework):

1. **Testability is the highest-leverage investment.** Agents perform dramatically better when they can verify their own work. Comprehensive test suites are more valuable than detailed instructions.
2. **Use standard patterns.** Agents recognize industry conventions (Django, DRF, Factory pattern, etc.) and work better with them than with custom abstractions.
3. **Keep files focused.** Split large files into focused modules. LLM context is finite — smaller, well-named files are easier for agents to navigate.
4. **Use complete, descriptive names.** `TaskListView` is better than `TLV`. Agents search by name — make your code discoverable.
5. **Add cross-references.** When code in one file depends on logic in another, mention the connection in a comment.

# How This Compares To Other Frameworks

| Framework | Phases | Key difference |
|-----------|--------|---------------|
| **This repo** | spec → clarify → research → plan → model/contract → tasks → implement → verify → debug | Explicit authorship roles, educational focus |
| **GitHub Spec Kit** | specify → plan → tasks → implement | Lighter, no separate clarify/research/model steps |
| **Addy Osmani workflow** | brainstorm → spec → chunk → context → review → commit | Emphasizes iterative review and small commits |
| **Anthropic recommended** | explore (plan mode) → plan → implement → commit | Built into Claude Code's native modes |

All of these share the same core insight: **separate intent (stable) from implementation (flexible)**, and give the agent structured documents instead of long chat histories.

# External References

- AGENTS.md open format (Linux Foundation): `https://agents.md/`
- GitHub — how to write a great `AGENTS.md`: `https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/`
- ETH Zurich — research on instruction file effectiveness: `https://www.infoq.com/news/2026/03/agents-context-file-value-review/`
- OpenAI Codex — custom instructions with `AGENTS.md`: `https://developers.openai.com/codex/guides/agents-md`
- Anthropic — Claude Code best practices: `https://code.claude.com/docs/en/best-practices`
- Anthropic — hierarchical project memory: `https://code.claude.com/docs/en/memory`
- GitHub Spec Kit: `https://github.com/github/spec-kit`
- Marmelab — Agent Experience framework: `https://marmelab.com/blog/2026/01/21/agent-experience.html`
- Addy Osmani — AI-augmented engineering workflow: `https://addyosmani.com/blog/ai-coding-workflow/`
- Agent Trace (Cursor/Cognition AI) — authorship tracking: `https://cognition.ai/blog/agent-trace`
