# Quiz App

A lightweight team project built with PHP, MySQL, HTML, CSS, and browser JavaScript. It supports student accounts, allowlisted teacher accounts, quiz authoring, publishing, attempts, and score review.

## Requirements

- PHP 8.1+ with PDO MySQL enabled
- MySQL 8.0+ (or a compatible MariaDB release)
- A web server that runs PHP (Apache/XAMPP is suitable)
- A modern browser

## Setup

1. Create a MySQL database, for example `quiz_app`.
2. Import [`database/schema.sql`](database/schema.sql).
3. Copy `config.example.php` to `config.php` and set database credentials. Set `teacher_emails` to a comma-separated list of teacher email addresses.
4. Serve this directory through a PHP-enabled web server. For a local PHP installation, `php -S localhost:8000` from the project directory is sufficient for development.
5. Open `http://localhost:8000/`.

Teacher emails are matched case-insensitively. A teacher must register using an address in the allowlist. Do not publish real credentials or commit a real `config.php`.

## Project map

- [`docs/architecture.md`](docs/architecture.md) — system structure and responsibilities
- [`docs/tech-stack.md`](docs/tech-stack.md) — stack choices and boundaries
- [`docs/api-design.md`](docs/api-design.md) — API actions, roles, payloads, and response shape
- [`docs/edge-cases.md`](docs/edge-cases.md) — expected failure and boundary behavior
- [`docs/testing-phases.md`](docs/testing-phases.md) — manual testing phases and acceptance checklist
- [`database/schema.sql`](database/schema.sql) — database tables and indexes
- `api.php` — session-authenticated API
- `index.html`, `styles.css`, `app.js` — browser interface

## Security notes

Passwords are stored with PHP's `password_hash`; SQL uses prepared statements; API requests validate session role and CSRF tokens; output is escaped before insertion into the page. For a public deployment, serve only over HTTPS, set secure cookie options, move secrets outside the web root, add rate limiting and account recovery, and review hosting-specific PHP configuration.

## Scope

This is a deliberately small course-project starter. It provides basic quiz authoring and multiple-choice attempts, not proctoring, timed exams, rich question types, email verification, or production-grade account recovery.
