# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Expense tracker web app built with Flask and SQLite. Educational project for learning full-stack development.

## Architecture

```
expense-tracker/
├── app.py              # Flask app with all routes — no blueprints
├── database/
│   ├── __init__.py
│   └── db.py           # SQLite helpers (currently empty)
├── templates/
│   ├── base.html       # Shared layout — all templates extend this
│   ├── landing.html
│   ├── register.html
│   ├── login.html
│   ├── terms.html
│   └── privacy.html
├── static/
│   ├── css/
│   │   └── style.css   # Global styles
│   └── js/
│       └── main.js     # Vanilla JS only
└── requirements.txt
```

## Where Things Belong

- **New routes** → `app.py` only, no blueprints
- **DB logic** → `database/db.py` only, never inline in routes
- **New pages** → new `.html` file extending `base.html`
- **Page-specific styles** → new `.css` file, not inline `<style>` tags

## Code Style

- **Python**: PEP 8, snake_case for all variables and functions
- **Templates**: Jinja2 with `url_for()` for every internal link — never hardcode URLs
- **Route functions**: one responsibility only — fetch data, render template, done
- **DB queries**: always use parameterized queries (`?` placeholders) — never f-strings in SQL
- **Error handling**: use `abort()` for HTTP errors, not bare `return "error string"`

## Tech Constraints

- Flask only — no FastAPI, no Django, no other web frameworks
- SQLite only — no PostgreSQL, no SQLAlchemy ORM, no external DB
- Vanilla JS only — no React, no jQuery, no npm packages
- No new pip packages — work within `requirements.txt` as-is unless explicitly told otherwise
- Python managed by uv only — no system Python assumed. All commands use `uv run` or activate `.venv`
- Python 3.10+ assumed — f-strings and match statements are fine

## Commands

```bash
# Setup (uv manages Python + venv)
uv venv
.venv\Scripts\activate      # Windows
uv pip install -r requirements.txt

# Run dev server (port 5001)
uv run app.py

# Run all tests
uv run pytest

# Run a specific test file
uv run pytest tests/test_foo.py

# Run a specific test by name
uv run pytest -k "test_name"

# Run tests with output visible
uv run pytest -s
```

## Implemented vs Stub Routes

| Route                      | Status                              |
|----------------------------|-------------------------------------|
| `GET /`                    | Implemented — renders landing.html  |
| `GET /register`            | Implemented — renders register.html |
| `GET /login`               | Implemented — renders login.html    |
| `GET /terms`               | Implemented — renders terms.html    |
| `GET /privacy`             | Implemented — renders privacy.html  |
| `GET /logout`              | Stub — Step 3                       |
| `GET /profile`             | Stub — Step 4                       |
| `GET /expenses/add`        | Stub — Step 7                       |
| `GET /expenses/<id>/edit`  | Stub — Step 8                       |
| `GET /expenses/<id>/delete`| Stub — Step 9                       |

**Do not implement a stub route unless the active task explicitly targets that step.**

## Warnings and Things to Avoid

- Never use raw string returns for stub routes once a step is implemented — always render a template
- Never hardcode URLs in templates — always use `url_for()`
- Never put DB logic in route functions — it belongs in `database/db.py`
- Never install new packages mid-feature without flagging it — keep `requirements.txt` in sync
- Never use JS frameworks — the frontend is intentionally vanilla
- `database/db.py` is currently empty — do not assume helpers exist until the step that implements them
- FK enforcement is manual — SQLite foreign keys are off by default; `get_db()` must run `PRAGMA foreign_keys = ON` on every connection
- The app runs on port **5001**, not the Flask default 5000 — don't change this
