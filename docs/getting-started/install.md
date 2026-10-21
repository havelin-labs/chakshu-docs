# Install with Docker Compose

Chakshu runs as three containers: Postgres, a one-shot `migrate` job, and `web`, which serves the portal and API on port 8000. A `worker` container for running reviews is planned.

## 1. Get the files

```bash
git clone https://github.com/havelin-labs/chakshu.git
cd chakshu
cp .env.example .env
```

## 2. Fill in `.env`

Two values are required:

```bash
# Signs sessions and encrypts stored secrets. Keep it safe; changing it
# makes stored secrets unreadable.
CHAKSHU_SECRET_KEY=$(python -c "import secrets; print(secrets.token_urlsafe(48))")

# Password for the bundled Postgres.
POSTGRES_PASSWORD=$(openssl rand -hex 24)
```

Set `CHAKSHU_BASE_URL` to the URL people will use, for example `https://chakshu.example.com`. The [configuration reference](../reference/configuration.md) lists everything else.

## 3. Start it

```bash
docker compose up -d --build --wait
```

Compose starts Postgres and waits for it to be healthy. Then it runs the migrations and starts `web`.

## 4. Check it

```bash
curl localhost:8000/healthz   # {"status":"ok","version":"..."}
curl localhost:8000/readyz    # {"status":"ok"} once the database answers
```

Open `http://localhost:8000` and you should see the instance status page with both checks green.

## Behind a reverse proxy

Put Chakshu behind whatever already terminates TLS for you (Caddy, nginx, a cloud load balancer) and forward to port 8000. Set `CHAKSHU_BASE_URL` to the public `https://` URL so cookies are marked secure and GitHub links come back to the right place.

## Upgrading

```bash
git pull
docker compose up -d --build --wait
```

Migrations run automatically before `web` starts.
