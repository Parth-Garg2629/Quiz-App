# Architecture

## Overview

The app uses a simple browser-to-PHP-to-MySQL design. The browser renders the interface and sends JSON requests to one PHP API file. PHP validates input, checks the session and role, performs prepared PDO queries, and returns JSON. MySQL is the source of truth for users, quizzes, questions, and attempts.

```text
Browser (HTML/CSS/JS)
        | JSON + same-origin session cookie + CSRF token
        v
PHP api.php (validation, authentication, authorization, business rules)
        | PDO prepared statements
        v
MySQL (users, quizzes, questions, attempts)
```

## Components

- **Presentation:** `index.html`, `styles.css`, and `app.js`; no frontend framework or build step.
- **API:** `api.php`; routes by an `action` field, enforces authentication and role checks, and emits JSON.
- **Configuration:** `config.php`, copied from `config.example.php`; contains database connection values and teacher email allowlist.
- **Persistence:** `database/schema.sql`; foreign keys cascade quiz-owned records and indexes support common lookups.
- **Authentication:** PHP session cookie, password hashing, and a per-session CSRF token for state-changing requests.

## Roles and access

- **Student:** register/sign in, list published quizzes, submit one attempt per quiz, and view own scores.
- **Teacher:** all student capabilities plus create/edit/delete own quizzes, manage their questions, publish/unpublish, and view attempts for own quizzes.
- A user role is assigned server-side at registration. Only emails in the configured teacher allowlist receive `teacher`; the browser never chooses a role.
- Teachers only manage quizzes they own. Students cannot read unpublished quiz content or other students' answers.

## Main flows

1. Registration checks normalized email uniqueness, assigns the allowlisted role, hashes the password, and starts a session.
2. Login verifies the password and returns the current user plus CSRF token.
3. A teacher creates a draft quiz, adds multiple-choice questions with one correct answer, then publishes it.
4. A student loads published quizzes, selects answers, and submits. PHP calculates score from stored correct answers and persists an attempt and answer rows in a transaction.
5. The teacher views scores for quizzes they own; students see only their own results.

## Deliberate limits

One API file keeps the coursework easy to navigate. If the project grows, separate routing, validation, auth, and quiz logic into small PHP modules without changing the API contract. This starter does not support multiple correct answers, deadlines, timed quizzes, or password reset.
