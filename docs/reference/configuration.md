# Configuration

Chakshu reads a few environment variables at startup. Everything else (GitHub App credentials, API keys, defaults) is set in the portal and stored in the database, with secrets encrypted.

## Environment

| Variable | Default | Notes |
|---|---|---|
| `CHAKSHU_SECRET_KEY` | required | At least 32 characters. Signs sessions and encrypts stored secrets. |
| `POSTGRES_PASSWORD` | required | Used by the bundled Postgres container. |
| `CHAKSHU_BASE_URL` | `http://localhost:8000` | Public URL of the portal. `https://` turns on secure cookies. |
| `CHAKSHU_WEBHOOK_URL` | `<base url>/webhooks/github` | Override for smee.io when running locally. |
| `CHAKSHU_DATABASE_URL` | set by compose | Point at a managed Postgres in production. |
| `CHAKSHU_ADMINS` | empty | Extra admin GitHub logins, comma separated. |
| `CHAKSHU_WORKER_CONCURRENCY` | `2` | Reviews run at the same time. |
| `CHAKSHU_LOG_LEVEL` | `INFO` | |
| `CHAKSHU_PORT` | `8000` | Host port compose publishes. |
| `CHAKSHU_GITHUB_URL`, `CHAKSHU_GITHUB_API_URL` | github.com | For GitHub Enterprise Server. |

## Portal settings (planned)

| Setting | Default |
|---|---|
| Anthropic API key | none |
| Default model | `sonnet` |
| Max cost per review | $2 |
| Reviewers repo | `chakshu-reviewers` |

## Endpoints

| Path | Purpose |
|---|---|
| `GET /healthz` | Liveness. Returns status and version. |
| `GET /readyz` | Readiness. Returns 503 if the database doesn't answer. |
| `POST /webhooks/github` | GitHub webhooks (planned). |
| everything else | The portal. |
