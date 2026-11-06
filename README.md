# Quiz App

A web-based quiz platform built for the Web Development lab. Teachers create and publish multiple-choice quizzes, and students take them and review their scores.

The stack is plain PHP, MySQL, HTML, CSS, and vanilla JavaScript, with no frameworks or build step.

## Features

**Students**
- Register and log in
- Browse published quizzes
- Take a quiz and submit answers
- Review past attempts and scores

**Teachers**
- Register using an allowlisted email address
- Create and edit quizzes
- Publish quizzes to make them available to students
- View student attempts and scores

## Tech Stack

| Layer    | Technology                          |
|----------|-------------------------------------|
| Frontend | HTML, CSS, JavaScript (no frameworks) |
| Backend  | PHP 8.1+ (PDO)                      |
| Database | MySQL 8.0+ or compatible MariaDB    |
| Server   | Apache (e.g. XAMPP) or PHP built-in server |

## Requirements

- PHP 8.1 or later with the PDO MySQL extension enabled
- MySQL 8.0 or later (or a compatible MariaDB release)
- A web server that can run PHP
- A modern browser

## Installation

1. Create a MySQL database, for example `quiz_app`.

   ```sql
   CREATE DATABASE quiz_app CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```

2. Import the schema.

   ```bash
   mysql -u <user> -p quiz_app < database/schema.sql
   ```

3. Create your local configuration file.

   ```bash
   cp config.example.php config.php
   ```

   Open `config.php` and set the database credentials. Set `teacher_emails` to a comma-separated list of the email addresses that may register as teachers.

4. Start a PHP-enabled server. For local development, the built-in server is enough:

   ```bash
   php -S localhost:8000
   ```

5. Open `http://localhost:8000/` in your browser.

## Teacher Accounts

Teacher access is controlled by an allowlist. A user can register as a teacher only if their email appears in `teacher_emails` in `config.php`. Matching is case-insensitive. Any other address registers as a student.

`config.php` contains credentials and must never be committed. Make sure it is listed in `.gitignore`.

## Project Structure

```
.
├── api.php               Session-authenticated API
├── index.html            Application page
├── app.js                Client-side logic
├── styles.css            Styles
├── config.example.php    Configuration template
├── database/
│   └── schema.sql        Tables and indexes
└── docs/
    ├── architecture.md   System structure and responsibilities
    ├── tech-stack.md     Technology choices and boundaries
    ├── api-design.md     API actions, roles, payloads, response format
    ├── edge-cases.md     Expected failure and boundary behavior
    └── testing-phases.md Manual test phases and acceptance checklist
```

## Documentation

Design and testing notes are in the `docs/` folder. Start with `architecture.md` for an overview, then `api-design.md` if you are working on the API or client.

## Security

- Passwords are hashed with PHP's `password_hash`.
- All SQL uses prepared statements.
- Each API request checks the session role and a CSRF token.
- Output is escaped before it is inserted into the page.

Before any public deployment, the following still need to be done:

- Serve the site over HTTPS only and set secure cookie options.
- Move `config.php` and other secrets outside the web root.
- Add rate limiting and an account recovery flow.
- Review the PHP configuration of the hosting environment.

## Scope and Limitations

This is a course project, so the scope is intentionally narrow. It supports basic quiz authoring and multiple-choice attempts. It does not include:

- Proctoring or timed exams
- Question types other than multiple choice
- Email verification
- Production-grade account recovery

