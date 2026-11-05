# Technology stack

## Required stack

- **HTML5:** page structure and forms.
- **CSS3:** responsive layout and visual states.
- **Vanilla JavaScript:** fetch-based API calls, rendering, and simple client-side validation.
- **PHP 8.1+:** same-origin JSON API, sessions, input validation, RBAC, score calculation.
- **MySQL 8+:** relational persistence with foreign keys and transactions.

## Why this is lightweight

There is no frontend framework, package manager, ORM, router dependency, or build pipeline. The browser calls a single `api.php` file and PHP uses built-in PDO/session/password APIs. The relational model is limited to four core entities: users, quizzes, questions, and attempts (with attempt answers).

## Conventions

- API requests and responses use JSON; action names are documented in `api-design.md`.
- IDs are integer primary keys. Emails are trimmed and lowercased before storage.
- Passwords use `password_hash()` and `password_verify()`; never store plaintext passwords.
- All values from requests are validated server-side and database queries use prepared statements.
- The server determines roles from configuration, not request data.
- Keep credentials in local `config.php`; do not place secrets in frontend JavaScript.
