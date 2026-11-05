# Testing phases

Use this as the team test plan. Run each phase in a local development database before demo day and record pass/fail plus any issue owner.

## Phase 1 — Setup and schema

- Import `database/schema.sql` into a clean database.
- Confirm all tables, foreign keys, unique constraints, and indexes are present.
- Configure valid and invalid database credentials and confirm a clear startup failure without leaking secrets.

## Phase 2 — Authentication

- Register a normal email and an allowlisted teacher email; verify assigned roles.
- Try duplicate, mixed-case, whitespace-padded, malformed emails and short/missing passwords.
- Sign in with correct and incorrect passwords; verify generic failure messages.
- Refresh while signed in, call `me`, log out, and verify the session is invalid afterward.

## Phase 3 — RBAC and ownership

- Call each teacher action as a student and verify rejection.
- Create two teacher accounts and verify one cannot change or inspect the other's quiz.
- Verify direct requests cannot reveal draft quizzes or correct answer indexes to students.
- Submit a state-changing request without a CSRF token and with a wrong token.

## Phase 4 — Teacher quiz lifecycle

- Create a draft, add questions, edit them, publish, unpublish, and delete.
- Try publishing an empty quiz and invalid question options/indexes.
- Verify published quiz edits are blocked until unpublished.
- Verify deletion confirmation and cascading cleanup.

## Phase 5 — Student attempt lifecycle

- Browse published quizzes, complete a correct/partially correct/incorrect attempt, and inspect score.
- Try submitting missing answers, invalid question IDs/indexes, and a second attempt.
- Verify score and percentage are server-calculated and stored answer rows match the submission.
- Verify a student sees only their own results and a teacher sees results only for owned quizzes.

## Phase 6 — Browser and error handling

- Check narrow and desktop layouts, keyboard navigation, labels, and visible validation/errors.
- Simulate API/network failure and empty lists; confirm the page remains usable.
- Enter markup-like text in names, quiz titles, and prompts; confirm it renders as text.
- Confirm the browser console has no uncaught errors and API responses contain no stack traces or credentials.

## Acceptance criteria

The project is demo-ready when all core flows pass, role/ownership restrictions hold when API calls are made directly, and no high-severity data loss or data exposure issue remains open.
