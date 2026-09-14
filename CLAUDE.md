# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

"Spendly" is a Flask expense-tracker web app being built incrementally as a step-by-step learning project. Most backend functionality is currently stubbed out with placeholder routes/comments marking which "Step" will implement them — check `app.py` and `database/db.py` before assuming a feature exists.

## Commands

```bash
# Run the dev server (from repo root, with venv active)
python app.py          # serves on http://127.0.0.1:5001, debug=True

# Run tests
pytest
```

There is no lint/build config in this repo (no `pyproject.toml`, `setup.cfg`, or linter). `requirements.txt` pins `flask`, `werkzeug`, `pytest`, and `pytest-flask`.

A `venv/` directory is checked into the working tree but is gitignored — don't edit files under it; treat it as the local virtualenv, not project source.

## Architecture

- **`app.py`** — single Flask app with all routes. Implemented routes render templates directly (`landing`, `register`, `login`); routes for `logout`, `profile`, and expense CRUD (`add_expense`, `edit_expense`, `delete_expense`) are placeholders returning plain strings, each tagged with the tutorial step that will implement it.
- **`database/db.py`** — intended to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (creates tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample dev data). Currently just a comment stub — not yet implemented.
- **`database/__init__.py`** — empty.
- **`templates/`** — Jinja2 templates. `base.html` defines the shared layout (nav, footer, font/CSS includes) with `title`/`head`/`content`/`scripts` blocks; `landing.html`, `login.html`, `register.html` extend it.
- **`static/css/style.css`**, **`static/js/main.js`** — shared frontend assets referenced via `url_for('static', ...)` in `base.html`.
- The SQLite DB file is expected at `expense_tracker.db` (gitignored, created at runtime once `init_db()` is implemented).

When implementing a new "Step," follow the pattern already established: wire real logic into the matching placeholder in `app.py`, and back it with `database/db.py` helpers rather than opening SQLite connections ad hoc in route handlers.
