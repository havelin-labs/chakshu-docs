# Roadmap

Work lands in small pull requests, one concern each. Each item links to its tracking issue in [havelin-labs/chakshu](https://github.com/havelin-labs/chakshu/issues).

## Done

- **Scaffold.** FastAPI app, settings, Dockerfile, docker compose, CI.
- **Database.** Postgres schema, Alembic migrations, deterministic IDs, encrypted settings, `/readyz`.
- **Portal shell.** React + TypeScript portal served by FastAPI, with light and dark mode.

## Next

1. **GitHub App setup.** One-time setup code, manifest flow to create the app, installation sync.
2. **Sign in with GitHub.** OAuth login, sessions, CSRF protection, admin roles.
3. **Reviewer catalog.** Sync and validate the reviewers repo; list reviewers in the portal.
4. **Webhooks and triggers.** Signature check, redelivery dedupe, `@org/chakshu` mentions, review requests, automatic runs.
5. **Queue and worker.** Runs table as a queue, `worker` service in compose, retries and cancellation.
6. **Review engine.** Workspace builder and the Claude Agent SDK run with read-only tools.
7. **Publisher.** Diff line validation, repeat-finding skip, one review and one check run per reviewer.
8. **Portal pages.** Pull requests, reviewers, runs with cost and progress, settings.
9. **First release.** Signed image on GHCR, upgrade notes, a template reviewers repo.

## Later

- Incremental review of new commits only
- Jira and other context through MCP connectors
- Other engines (OpenAI, Copilot SDK)
- GitLab and Bitbucket
