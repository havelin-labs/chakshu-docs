# Install with Docker Compose

Chakshu runs as four containers from one image plus Postgres:

| Service | Does |
|---|---|
| `db` | Postgres 17, data in the `pgdata` volume |
| `migrate` | Applies database migrations, then exits |
| `web` | Portal, API and GitHub webhook endpoint on port 8000 |
| `worker` | Picks up queued reviews and runs them |

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

The [configuration reference](../reference/configuration.md) lists the optional ones, such as worker concurrency and Bedrock/Vertex.

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

Put Chakshu behind whatever already terminates TLS for you (Caddy, nginx, a cloud load balancer) and forward to port 8000. Set `CHAKSHU_BASE_URL` to the public `https://` URL. That turns on secure cookies and gives GitHub the right callback and webhook addresses.

## Upgrading

```bash
git pull
docker compose up -d --build --wait
```

Migrations run before `web` and `worker` start. A review that was running when the worker stopped is picked up again automatically.

## Checking health

```bash
curl localhost:8000/healthz   # the web process is up
curl localhost:8000/readyz    # it can reach the database
docker compose exec worker chakshu worker-health && echo worker ok
```
