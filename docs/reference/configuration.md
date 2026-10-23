---
icon: material/cog-outline
---

# Configuration

A few things are set in the environment before Chakshu starts. Everything else is set in the portal and stored in the database, with secrets encrypted.

## Environment

| Variable | Default | Notes |
|---|---|---|
| `CHAKSHU_SECRET_KEY` | required | At least 32 characters. Signs sessions and encrypts stored secrets. |
| `POSTGRES_PASSWORD` | required | Used by the bundled Postgres container. |
| `CHAKSHU_BASE_URL` | `http://localhost:8000` | Public URL of the portal. `https://` turns on secure cookies. |
| `CHAKSHU_WEBHOOK_URL` | `<base url>/webhooks/github` | Set it to a smee.io channel when running on a laptop. |
| `CHAKSHU_RUNNER_URL` | the base URL | Where GitHub Actions runners fetch jobs and report results. Must be reachable from the internet; use the `tunnel` profile on a laptop. |
| `CHAKSHU_DATABASE_URL` | set by compose | Point at a managed Postgres in production. |
| `CHAKSHU_ADMINS` | empty | Extra admin GitHub logins, comma separated. |
| `CHAKSHU_LOG_LEVEL` | `INFO` | |
| `CHAKSHU_PORT` | `8000` | Host port compose publishes. |
| `CHAKSHU_GITHUB_URL`, `CHAKSHU_GITHUB_API_URL` | github.com | For GitHub Enterprise Server. |
| `ANTHROPIC_API_KEY` | none | Copied to repositories as their Actions secret when no Claude access is saved in **Settings**. |
| `CLAUDE_CODE_OAUTH_TOKEN` | none | A subscription token from `claude setup-token`, copied to repositories when nothing is saved in **Settings**. |

## Portal settings (admins)

| Setting | Default | Notes |
|---|---|---|
| Anthropic API key | none | Write-only. Wins when both credentials are set. Saving copies it to every enabled repository's Actions secrets. |
| Claude subscription token | none | Write-only. From `claude setup-token`; uses that person's Pro or Max limits. |
| Webhooks | off | **Connect webhooks** sets the app's webhook secret on GitHub. |
| Default model | `sonnet` | Used when a reviewer doesn't set `model`. |
| Cost cap per review | $2 | Used when a reviewer doesn't set `limits.max_usd`. |
| Monthly spend cap | none | New reviews are refused once this calendar month's (UTC) spend reaches it. Settings shows spend so far. |
| Reviewers repo name | `chakshu-reviewers` | The repo in each org that holds reviewer folders. |
| Review action | `havelin-labs/chakshu@main` | The action the review workflow runs (`owner/repo@ref`). Set through the API as `actions_action`. |

Admins can also turn reviews off for a repository on the **Repositories** page.

## Endpoints

| Path | Purpose |
|---|---|
| `GET /healthz` | Liveness. Returns status and version. |
| `GET /readyz` | Readiness. Returns 503 if the database doesn't answer. |
| `POST /webhooks/github` | GitHub webhooks. Signature-checked and stored; the worker handles them. Redeliveries are ignored. |
| `GET /auth/login`, `GET /auth/callback` | Sign in with GitHub. |
| `/api/runner/runs/{id}/...` | For GitHub Actions runners: fetch the job, call connector tools, report the result. Authenticated with the run's OIDC token, not a session. |
| `/api/*` | The portal's JSON API. State-changing calls need the session's `X-CSRF-Token`. |
| everything else | The portal. |

## Commands

The image's entry point is `chakshu`:

| Command | Does |
|---|---|
| `chakshu web` | Serve the portal, API and webhooks |
| `chakshu worker` | Handle webhooks and dispatch queued reviews to GitHub Actions |
| `chakshu worker-health` | Exit 0 if the worker loop is alive (the worker's healthcheck) |
| `chakshu migrate` | Apply database migrations |
| `chakshu action` | Do one review inside GitHub Actions (what the workflow's action runs) |
