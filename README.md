# Chakshu docs

Source for the documentation at https://chakshu.havelin.in/docs. The code lives in [havelin-labs/chakshu](https://github.com/havelin-labs/chakshu).

## Working on the docs

```bash
pip install -r requirements.txt
mkdocs serve          # http://127.0.0.1:8000/docs/
mkdocs build --strict # what CI runs
```

Pages are Markdown under `docs/`. The sidebar order is set in `mkdocs.yml`.

Write for someone setting Chakshu up for their team: say what to do and what they'll see. Mark anything not built yet with a "Planned" note so nobody goes looking for a button that doesn't exist.
