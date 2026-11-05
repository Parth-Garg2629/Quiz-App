# API design

All actions use `POST /api.php` with a JSON body containing `action`. Responses are JSON. Protected calls use the PHP session cookie; send the CSRF token returned by `register` or `login` in the `X-CSRF-Token` header for state-changing actions. `me` and `logout` need no CSRF header. Errors use `{ "error": "message" }` with an appropriate HTTP status.

## Authentication

| Action | Body | Access | Result |
|---|---|---|---|
| `register` | `email`, `password`, `name` | Public | `user`, `csrf_token`; role assigned by server allowlist |
| `login` | `email`, `password` | Public | `user`, `csrf_token` |
| `me` | none | Signed in | `user`, `csrf_token` |
| `logout` | none | Signed in | `message` |

## Quiz and attempt actions

| Action | Body | Access | Result |
|---|---|---|---|
| `list_quizzes` | none | Signed in | Published quiz summaries for students; own quizzes for teachers |
| `get_quiz` | `quiz_id` | Signed in | Quiz and questions; unpublished quiz only for its owner teacher; correct answers are never returned |
| `save_quiz` | optional `quiz_id`, `title`, `description` | Teacher | Created/updated own draft quiz |
| `save_question` | optional `question_id`, `quiz_id`, `prompt`, `options` (2–6 strings), `correct_index` | Teacher | Created/updated question in own draft quiz |
| `delete_question` | `question_id` | Teacher | Confirmation message |
| `delete_quiz` | `quiz_id` | Teacher | Confirmation message; owned quiz and associated data removed |
| `publish_quiz` | `quiz_id`, `published` boolean | Teacher | Updated publish state; publishing requires at least one valid question |
| `submit_attempt` | `quiz_id`, `answers` object mapping question ID to option index | Student | `attempt_id`, `score`, `total`, `percentage` |
| `my_results` | none | Signed in | Student's results; teachers receive their own attempt results as student if applicable |
| `quiz_results` | `quiz_id` | Teacher, owner only | Attempt summaries for that quiz |

## Status codes

- `200`: successful read/update/action
- `201`: newly created resource
- `400`: malformed JSON or invalid fields
- `401`: missing or invalid session/credentials
- `403`: role, ownership, CSRF, or published-state restriction
- `404`: resource not found or not visible to this user
- `409`: duplicate account or repeat attempt
- `500`: unexpected server error (details stay in server logs)

The implementation may return `200` for successful writes to keep the client simple; clients should use the JSON body as the source of success information.
