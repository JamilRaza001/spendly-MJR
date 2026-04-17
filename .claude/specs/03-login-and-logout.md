# Spec: Login and Logout

## Overview

Implement the login and logout flows so registered users can authenticate into Spendly and end their session. The `/login` route is currently a stub that only renders a form (GET only). This step wires up the POST handler: looks up the user by email, verifies the password with werkzeug, stores `session["user_id"]` on success, and redirects to `/profile`. The `/logout` route clears the session and redirects to the landing page. Together, these two routes close the authentication loop started by Step 02 and are a prerequisite for all protected pages.

## Depends on

- Step 01 — Database setup (`get_db()`, `init_db()`, `users` table in place)
- Step 02 — Registration (`create_user()`, `session["user_id"]` convention established)

## Routes

- `GET  /login`  — render the login form — public
- `POST /login`  — process login form, validate credentials, set session — public
- `GET  /logout` — clear session and redirect to landing page — logged-in

## Database changes

No database changes.

## Templates

- **Modify:** `templates/login.html`
  - Form must POST to `/login`
  - Must include `name` and `email` fields rendered correctly
  - Must render `{% if error %}` block to display validation messages
- **Modify:** `templates/base.html`
  - Navbar should conditionally show Login/Register links when logged out, and Logout link when logged in (check `session.get("user_id")`)

## Files to change

- `app.py` — convert `login()` to handle GET and POST; implement `logout()` to clear session and redirect
- `database/db.py` — add helper `get_user_by_email(email)` that returns the user row or `None`
- `templates/login.html` — add POST action, error block
- `templates/base.html` — update navbar with session-aware links

## Files to create

- None

## New dependencies

No new dependencies.

## Rules for implementation

- No SQLAlchemy or ORMs
- Parameterised queries only — never use string formatting in SQL
- Passwords verified with `werkzeug.security.check_password_hash`
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- Never store the plain-text password anywhere; only compare via `check_password_hash`
- If email not found OR password does not match, show the same generic error: "Invalid email or password." (do not reveal which field was wrong)
- After successful login, store `session["user_id"]` and redirect to `/profile`
- `logout()` must call `session.clear()` (or `session.pop("user_id", None)`) then redirect to `url_for("landing")`
- `get_user_by_email()` in `db.py` must close the connection before returning

## Definition of done

- [ ] `GET /login` renders the login form with no errors
- [ ] Submitting valid credentials sets `session["user_id"]` and redirects to `/profile`
- [ ] The demo user (`demo@spendly.com` / `demo123`) can log in successfully
- [ ] Submitting an email that does not exist shows "Invalid email or password."
- [ ] Submitting a correct email with wrong password shows "Invalid email or password."
- [ ] Submitting with empty email or password re-renders the form with an error message
- [ ] `GET /logout` clears the session and redirects to the landing page (`/`)
- [ ] After logout, `session["user_id"]` is no longer set
- [ ] Navbar shows Login and Register links when no user is logged in
- [ ] Navbar shows Logout link when a user is logged in
- [ ] App starts without errors after changes
