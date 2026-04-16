# Spec: Registration

## Overview

Implement the user registration flow so new visitors can create a Spendly account. The `/register` route currently only renders a static form (GET only). This step wires up the POST handler: validates input, hashes the password with werkzeug, inserts the new user into the `users` table, and redirects on success. On failure (duplicate email, short password, missing fields) it re-renders the form with a clear error message. This is the first feature that writes user data to the database and is a prerequisite for all authenticated features.

## Depends on

- Step 01 — Database setup (`get_db()`, `init_db()`, schema in place)

## Routes

- `GET  /register` — render empty registration form — public
- `POST /register` — process registration form submission — public

## Templates

- **Modify**: `templates/register.html`
  - Form already exists and posts to `/register`; no structural changes needed
  - The `{% if error %}` block is already present — no changes required
  - No new templates needed

## Files to change

- `app.py` — convert `register()` to handle both GET and POST; add import for `request`, `redirect`, `url_for`, `session`; add `app.secret_key`
- `database/db.py` — add helper `create_user(name, email, password_hash)` that inserts a row and returns the new user id

## Files to create

- None

## New dependencies

No new dependencies.

## Rules for implementation

- No SQLAlchemy or ORMs
- Parameterised queries only — never use string formatting in SQL
- Passwords hashed with `werkzeug.security.generate_password_hash`
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- `app.secret_key` must be set before any `session` usage; use a hard-coded dev string for now (e.g. `"dev-secret-change-me"`)
- Validation order: check all fields present → password length ≥ 8 → attempt INSERT → catch duplicate email
- Catch `sqlite3.IntegrityError` to detect duplicate email; surface as `error = "An account with that email already exists."`
- After successful registration, log the user in by storing `session["user_id"]` and redirect to `/profile`
- `create_user()` in `db.py` must close the connection before returning

## Definition of done

- [ ] `GET /register` renders the form with no errors
- [ ] Submitting the form with all valid fields creates a new user row in the database
- [ ] Password is stored as a hash (not plain text) in `password_hash` column
- [ ] After successful registration the browser is redirected to `/profile`
- [ ] `session["user_id"]` is set after successful registration
- [ ] Submitting with a missing name, email, or password re-renders the form with an error message
- [ ] Submitting with a password shorter than 8 characters re-renders with an error message
- [ ] Registering with an already-used email re-renders with "An account with that email already exists."
- [ ] The demo user's email (`demo@spendly.com`) triggers the duplicate-email error if used
- [ ] App starts without errors after changes
