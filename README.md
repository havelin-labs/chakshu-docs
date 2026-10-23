# Chakshu docs

Source for the documentation at https://chakshu.havelin.in/docs. The code lives in [havelin-labs/chakshu](https://github.com/havelin-labs/chakshu).

## Working on the docs

```bash
pip install -r requirements.txt
mkdocs serve          # http://127.0.0.1:8000/docs/
mkdocs build --strict # what CI runs
```

Pages are Markdown under `docs/`. The sidebar order is set in `mkdocs.yml`.

## Publishing

The site is hosted on Vercel and served at `https://chakshu.havelin.in/docs/`. `vercel.json` holds the whole setup: Vercel installs `requirements.txt`, builds with `mkdocs build --strict` into `dist/docs`, copies the 404 page to the root and redirects `/` to `/docs/`. Every push to `main` deploys to production, and every pull request gets a preview URL.

To check the Vercel build locally:

```bash
mkdocs build --strict --site-dir dist/docs
```

Write for someone setting Chakshu up for their team: say what to do and what they'll see. Mark anything not built yet with a "Planned" note so nobody goes looking for a button that doesn't exist.
