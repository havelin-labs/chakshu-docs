# Configuration

A few things are set in the environment before Chakshu starts. Everything else is set in the portal and stored in the database, with secrets encrypted.

## Environment

| Variable | Default | Notes |
|---|---|---|
| `CHAKSHU_SECRET_KEY` | required | At least 32 characters. Signs sessions and encrypts stored secrets. |
| `POSTGRES_PASSWORD` | required | Used by the bundled Postgres container. |
| `CHAKSHU_BASE_URL` | `http://localhost:8000` | Public URL of the portal. `https://` turns on secure cookies. |
| `CHAKSHU_WEBHOOK_URL` | `<base url>/webhooks/github` | Set it to a smee.io channel when running on a laptop. |
| `CHAKSHU_DATABASE_URL` | set by compose | Point at a managed Postgres in production. |
| `CHAKSHU_ADMINS` | empty | Extra admin GitHub logins, comma separated. |
| `CHAKSHU_WORKER_CONCURRENCY` | `2` | Reviews each worker runs at the same time. |
| `CHAKSHU_LOG_LEVEL` | `INFO` | |
| `CHAKSHU_PORT` | `8000` | Host port compose publishes. |
| `CHAKSHU_GITHUB_URL`, `CHAKSHU_GITHUB_API_URL` | github.com | For GitHub Enterprise Server. |
| `ANTHROPIC_API_KEY` | none | Used when no key is saved in **Settings**. |
| `CLAUDE_CODE_USE_BEDROCK`, `AWS_REGION` | none | Run reviews through Amazon Bedrock (AWS credentials from the usual environment variables). |
| `CLAUDE_CODE_USE_VERTEX` | none | Run reviews through Google Vertex AI. |

## Portal settings (admins)

| Setting | Default | Notes |
|---|---|---|
| Anthropic API key | none | Write-only. The page shows whether it's set and its last characters. |
| Default model | `sonnet` | Used when a reviewer doesn't set `model`. |
| Cost cap per review | $2 | Used when a reviewer doesn't set `limits.max_usd`. |
| Monthly spend cap | none | New reviews are refused once this calendar month's (UTC) spend reaches it. Settings shows spend so far. |
| Reviewers repo name | `chakshu-reviewers` | The repo in each org that holds reviewer folders. |

Admins can also turn reviews off for a repository on the **Repositories** page.

## Endpoints

| Path | Purpose |
|---|---|
| `GET /healthz` | Liveness. Returns status and version. |
| `GET /readyz` | Readiness. Returns 503 if the database doesn't answer. |
| `POST /webhooks/github` | GitHub webhooks. Signature-checked and stored; the worker handles them. Redeliveries are ignored. |
| `GET /auth/login`, `GET /auth/callback` | Sign in with GitHub. |
| `/api/*` | The portal's JSON API. State-changing calls need the session's `X-CSRF-Token`. |
| everything else | The portal. |

## Commands

The image's entry point is `chakshu`:

| Command | Does |
|---|---|
| `chakshu web` | Serve the portal, API and webhooks |
| `chakshu worker` | Run queued reviews |
| `chakshu worker-health` | Exit 0 if the worker loop is alive (the worker's healthcheck) |
| `chakshu migrate` | Apply database migrations |
