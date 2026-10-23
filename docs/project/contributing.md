---
icon: material/hand-heart-outline
---

# Contributing

The full guide lives in the code repo's [CONTRIBUTING.md](https://github.com/havelin-labs/chakshu/blob/main/CONTRIBUTING.md). The short version:

```bash
git clone https://github.com/havelin-labs/chakshu.git && cd chakshu
python -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"
cp .env.example .env
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d db   # Postgres on :5433
pytest
cd frontend && npm install && npm run dev
```

Keep pull requests to one concern. Commit subjects are lowercase and imperative, like `feat: add webhook signature check`, with a body when the reason isn't obvious.

Docs changes go to [havelin-labs/chakshu-docs](https://github.com/havelin-labs/chakshu-docs). Preview them with `mkdocs serve`.

Found a security problem? Please report it privately; see [SECURITY.md](https://github.com/havelin-labs/chakshu/blob/main/SECURITY.md).
