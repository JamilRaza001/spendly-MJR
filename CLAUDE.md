# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Spendly** — a Flask + SQLite expense tracker. This is a step-by-step teaching project (Campus X course) where students implement features incrementally. Several routes in `app.py` and `database/db.py` are stubs with comments indicating which step fills them in.

## Commands

```bash
# Activate virtual environment (Windows)
.venv\Scripts\activate

# Run the dev server (port 5001, debug mode)
python app.py

# Or via Flask CLI
flask run --port 5001 --debug

# Run all tests
pytest

# Run a single test file
pytest tests/test_auth.py

# Run a single test by name
pytest -k "test_login"
```

## Architecture

### Entry point

`app.py` — creates the Flask app and registers all routes. Currently has live routes for landing, register, login, terms, privacy, and stub routes (returning plain strings) for logout, profile, and expenses CRUD.

### Database layer

`database/db.py` — students implement three functions here:
- `get_db()` — returns a SQLite connection with `row_factory = sqlite3.Row` and `PRAGMA foreign_keys = ON`
- `init_db()` — creates all tables using `CREATE TABLE IF NOT EXISTS`
- `seed_db()` — inserts sample data

The SQLite file `expense_tracker.db` is gitignored and lives at the project root.

### Templates

All templates extend `templates/base.html`. Base includes the global navbar, footer, `static/css/style.css`, and `static/js/main.js`. Page-specific CSS is loaded via `{% block head %}`. The landing page has its own `static/css/landing.css`.

### Static assets

- `static/css/style.css` — global styles (navbar, footer, shared components)
- `static/css/landing.css` — landing page only
- `static/js/main.js` — vanilla JS; students add to this as features are built

## Step-by-step build order (course context)

The stub comments in the code reference these steps:
1. Database setup (`database/db.py`)
2. Registration with password hashing
3. Login / logout with Flask sessions
4. User profile page
5–6. Expense list and dashboard
7. Add expense form
8. Edit expense
9. Delete expense
