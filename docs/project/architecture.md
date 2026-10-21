# Architecture

```mermaid
flowchart TB
  browser[Browser] -->|HTTPS| web
  github[GitHub App] -->|webhooks| web
  subgraph compose[docker compose]
    web["web: FastAPI API, React portal, webhooks"]
    worker["worker: review runner"]
    db[(Postgres 17)]
    web --> db
    worker --> db
  end
  worker -->|fetch code, post reviews| github
  worker -->|Claude Agent SDK| anthropic[Anthropic API]
```

One Python codebase runs as two processes from the same image: `web` (portal, API, webhooks) and `worker` (reviews). Postgres holds settings, a cache of each org's reviewers, and the `runs` table, which doubles as the job queue. There's no Redis and no message broker.

## Stack

| Part | Choice |
|---|---|
| API | Python 3.12, FastAPI, Pydantic Settings, Starlette signed-cookie sessions |
| Data | PostgreSQL 17, SQLAlchemy 2 (async, asyncpg), Alembic |
| Portal | React 19, TypeScript, Vite, Tailwind v4, shadcn/ui, TanStack Query |
| GitHub | githubkit (app, installation, OAuth and user clients; webhook verification) |
| Reviews | Claude Agent SDK |
| Secrets | Fernet, key derived with HKDF from `CHAKSHU_SECRET_KEY` |
| Packaging | Multi-stage Docker image, docker compose, signed images on GHCR |

## Code layout

```text
src/chakshu/
  api/        HTTP routes. Thin: parse, call a service, return JSON.
  handlers/   One module per GitHub webhook event.
  services/   Logic that touches the database, GitHub or the model:
              setup, users, installations, catalog, triggers, runs, queue,
              workspace, engine, publisher, checks, review_runner.
  helpers/    Pure functions, no I/O: selection, mentions, catalog parsing,
              prompt, diff, findings, workspace rules, git.
  db/         Models, session, upsert, migrations.
  core/       Settings, logging, IDs, crypto.
  worker.py   The worker loop.
frontend/src/
  pages/      One file per screen.
  components/ Layout, shared pieces and shadcn/ui components.
  lib/        API client and formatting.
```

## Data

| Table | Holds |
|---|---|
| `settings` | Portal settings; secrets encrypted |
| `installations` | GitHub App installations |
| `users` | People who sign in |
| `repos` | Repos the app can see, and whether reviews are on |
| `reviewers` | Cache of each `reviewer.yaml`, the commit it came from and any errors |
| `runs` | One reviewer × one PR × one commit; also the job queue |
| `webhook_deliveries` | Handled delivery IDs, for deduplication |

Every primary key is a [deterministic ID](../reference/ids.md).
