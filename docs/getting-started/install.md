---
icon: material/docker
---

# Install with Docker Compose

Chakshu runs as a few containers from one published image, `ghcr.io/havelin-labs/chakshu`, plus Postgres. The image is built for amd64 and arm64. Reviews themselves run in GitHub Actions in each reviewed repository, so the server stays small.

| Service | Does |
|---|---|
| `db` | Postgres 17, data in the `pgdata` volume |
| `migrate` | Applies database migrations, then exits |
| `web` | Portal, API and GitHub webhook endpoint on port 8000 |
| `worker` | Handles webhooks and starts each queued review as a GitHub Actions run |
| `smee` | Optional, for laptops: forwards webhooks from a smee.io channel |
| `tunnel` | Optional, for laptops: a Cloudflare tunnel so Actions runners can reach Chakshu |

## 1. Get the files

Each [release](https://github.com/havelin-labs/chakshu/releases) has a `compose.yaml` pinned to that version and an example `.env`. Nothing to clone or build:

```bash
mkdir chakshu && cd chakshu
curl -fsSLO https://github.com/havelin-labs/chakshu/releases/latest/download/compose.yaml
curl -fsSL https://github.com/havelin-labs/chakshu/releases/latest/download/env.example -o .env
```

## 2. Fill in `.env`

Three values matter:

```bash
# Signs sessions and encrypts stored secrets (32+ characters). Keep it safe:
# changing it makes the stored GitHub keys and API key unreadable.
CHAKSHU_SECRET_KEY=$(openssl rand -base64 48)

# Password for the bundled Postgres.
POSTGRES_PASSWORD=$(openssl rand -hex 24)

# The URL people (and GitHub) use to reach Chakshu.
CHAKSHU_BASE_URL=https://chakshu.example.com
```

The [configuration reference](../reference/configuration.md) lists the optional ones, such as `CHAKSHU_RUNNER_URL` for the address GitHub Actions runners call back to.

## 3. Start it

```bash
docker compose up -d --wait
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

Download the newer `compose.yaml` and start again. Your `.env` stays as it is.

```bash
curl -fsSLO https://github.com/havelin-labs/chakshu/releases/latest/download/compose.yaml
docker compose up -d --wait
```

To pin a version without downloading, set `CHAKSHU_VERSION=0.1.0` in `.env`.

Migrations run before `web` and `worker` start. Reviews in progress keep running in GitHub Actions while the server restarts; a runner that can't reach Chakshu for a moment retries. Webhooks that arrive during the upgrade wait in the database until the new worker is up.

## Checking health

```bash
curl localhost:8000/healthz   # the web process is up
curl localhost:8000/readyz    # it can reach the database
docker compose exec worker chakshu worker-health && echo worker ok
```

## Verifying the image

Release images are signed with [cosign](https://docs.sigstore.dev/cosign/) by the repository's release workflow, with no long-lived key:

```bash
cosign verify ghcr.io/havelin-labs/chakshu:0.1.0 \
  --certificate-identity-regexp '^https://github.com/havelin-labs/chakshu/\.github/workflows/release\.yml@refs/tags/v' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com
```

Each image also carries an SBOM and build provenance.

## From source

To build the image yourself, clone the repository, copy `.env.example` to `.env`, and run `docker compose up -d --build --wait`. Upgrade with `git pull` and the same command.
