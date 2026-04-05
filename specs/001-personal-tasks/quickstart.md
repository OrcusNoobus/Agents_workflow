# quickstart.md

## What this file is
This file explains how a human should manually run and verify the feature. It complements automated tests by testing from a user perspective.

**Authored by:** Both. The agent drafts the verification steps based on the spec and contracts. The human runs them and confirms they work. If the steps are wrong, the human corrects them. This file should be updated whenever the contract changes.

## When to use it
Use this file after implementation exists, during QA, during onboarding, or when you want a repeatable manual verification flow. An agent should also use it during `implement.md` verification checkpoints.

## When not to use it
Do not use this file as a substitute for automated tests. It is a complement, not a replacement. Automated tests catch regressions; manual verification catches "it works but feels wrong" issues.

## Reads from
- `spec.md`
- `contracts/tasks-api.md`
- The eventual implementation

## Feeds into
- Manual QA
- Onboarding
- Confidence that the feature behaves as intended

> Teaching note: manual verification steps are valuable because they test the feature from a user perspective, not just from the internal code perspective. Anthropic's research shows agents perform dramatically better when they can verify their own work — this file gives them a way to do that beyond unit tests.

# Current Status

This repository is docs-only right now.

The steps below describe how you would verify the feature after the Django project is implemented.

# Expected Local Setup

- Docker installed
- Docker Compose available
- future `TaskBox` project generated in this repository

# Start The Future App

```bash
docker compose up --build
docker compose exec web python manage.py migrate
```

# Prepare Test Data

Create two users:

- `alice@example.com`
- `bob@example.com`

Log in or otherwise obtain authentication for each user according to the eventual project setup.

# Manual Verification Flow

## 1. Create a task as Alice

Send:

```http
POST /api/tasks/
```

Body:

```json
{
  "title": "Buy milk",
  "description": "2 bottles"
}
```

Expected result:

- status `201 Created`
- response contains the created task
- `is_completed` is `false`

## 2. List tasks as Alice

Send:

```http
GET /api/tasks/
```

Expected result:

- status `200 OK`
- Alice sees her task

## 3. Confirm Bob cannot see Alice's task

Send the same list request as Bob.

Expected result:

- status `200 OK`
- Alice's task is not present in Bob's results

## 4. Complete the task as Alice

Send:

```http
PATCH /api/tasks/{alice_task_id}/complete/
```

Expected result:

- status `200 OK`
- `is_completed` is now `true`

## 5. Confirm Bob cannot complete Alice's task

Send the complete request as Bob.

Expected result:

- the request does not succeed
- the task remains unchanged

# Maintenance Note

If the API contract changes, update this file so the manual flow stays trustworthy.
