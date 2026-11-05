# Edge cases and expected behavior

## Accounts and sessions

- Email with surrounding spaces or mixed case is normalized before lookup and storage.
- Duplicate email registration returns a conflict; it never overwrites an account.
- Missing, malformed, or weak passwords are rejected; password length policy is enforced on the server.
- A teacher email is allowlisted using normalized exact email matching; near matches remain students.
- Invalid credentials return a generic message to avoid revealing whether an email exists.
- Expired session, missing CSRF token, or mismatched CSRF token rejects protected writes.

## Authorization and ownership

- A student cannot call teacher actions even if they forge frontend requests.
- A teacher cannot edit, publish, delete, or inspect results for another teacher's quiz.
- Unpublished quizzes are hidden from students; direct requests get a not-found/forbidden response.
- Correct answers are not included in quiz reads for students.

## Quiz content

- Empty title/prompt, too few or too many options, duplicate options, and invalid correct-answer indexes are rejected.
- A draft with no questions cannot be published.
- A student submitting missing, extra, or invalid question IDs is rejected; omitted answers may count as incorrect only if the API contract explicitly allows it (this starter requires one answer per question).
- Editing a published quiz is blocked until it is unpublished, avoiding score changes after attempts.
- Deleting a quiz removes its questions and attempts through database cascades; the UI should ask for confirmation.

## Attempts and data integrity

- A student may submit at most one attempt per quiz; a unique database constraint handles concurrent duplicate submissions.
- Score is calculated on the server from stored correct indexes, never trusted from the browser.
- Attempt and answer rows are inserted in one transaction; a partial write is rolled back.
- Quiz deletion during an attempt or two simultaneous submissions should fail cleanly and return a retryable error.
- Zero-question quizzes cannot be published, preventing division by zero in percentage calculation.

## Client and network

- Invalid JSON, missing action, unknown action, and oversized request bodies return a client error.
- Network failure displays an actionable error and preserves unsent form values where practical.
- All text displayed from database/user input is inserted as text, not executable HTML.
- Empty quiz/result lists show an explicit empty state.
