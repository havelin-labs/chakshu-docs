---
icon: material/docker
---

# Install with Docker Compose

Chakshu runs as a few containers from one image plus Postgres. Reviews themselves run in GitHub Actions in each reviewed repository, so the server stays small.

| Service | Does |
|---|---|
| `db` | Postgres 17, data in the `pgdata` volume |
| `migrate` | Applies database migrations, then exits |
| `web` | Portal, API and GitHub webhook endpoint on port 8000 |
| `worker` | Handles webhooks and starts each queued review as a GitHub Actions run |
| `smee` | Optional, for laptops: forwards webhooks from a smee.io channel |
| `tunnel` | Optional, for laptops: a Cloudflare tunnel so Actions runners can reach Chakshu |

## 1. Get the files

```bash
git clone https://github.com/havelin-labs/chakshu.git
cd chakshu
cp .env.example .env
```

## 2. Fill in `.env`

Three values matter:

```bash
# Signs sessions and encrypts stored secrets (32+ characters). Keep it safe:
# changing it makes the stored GitHub keys and API key unreadable.
CHAKSHU_SECRET_KEY=$(python -c "import secrets; print(secrets.token_urlsafe(48))")

# Password for the bundled Postgres.
POSTGRES_PASSWORD=$(openssl rand -hex 24)

# The URL people (and GitHub) use to reach Chakshu.
CHAKSHU_BASE_URL=https://chakshu.example.com
```

The [configuration reference](../reference/configuration.md) lists the optional ones, such as `CHAKSHU_RUNNER_URL` for the address GitHub Actions runners call back to.

## 3. Start it

```bash
docker compose up -d --build --wait
docker compose ps
```

You should see `db`, `web` and `worker` healthy and `migrate` exited with code 0.

## 4. Find the setup code

```bash
docker compose logs web | grep "Setup code"
```

It looks like `Setup code: K7QX-M2PD`. The code is printed on every start until the GitHub App is connected. Continue with [Connect GitHub](connect-github.md).

## Behind a reverse proxy

Put Chakshu behind whatever already terminates TLS for you (Caddy, nginx, a cloud load balancer) and forward to port 8000. Set `CHAKSHU_BASE_URL` to the public `https://` URL. That turns on secure cookies and gives GitHub the right callback, webhook and runner addresses.

## On a laptop

GitHub has to reach Chakshu twice: webhooks, and Actions runners reporting back. Neither can reach `localhost`. Use the two Compose profiles: `smee` for webhooks (see [Turn on webhooks](connect-github.md#turn-on-webhooks)) and `tunnel` for runners (see [Set up GitHub Actions](github-actions.md#3-let-runners-reach-chakshu)).

## Upgrading

```bash
git pull
docker compose up -d --build --wait
```

Migrations run before `web` and `worker` start. Reviews in progress keep running in GitHub Actions while the server restarts; a runner that can't reach Chakshu for a moment retries. Webhooks that arrive during the upgrade wait in the database until the new worker is up.

## Checking health

```bash
curl localhost:8000/healthz   # the web process is up
curl localhost:8000/readyz    # it can reach the database
docker compose exec worker chakshu worker-health && echo worker ok
```
