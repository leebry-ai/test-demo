# API (server routes)

All routes return JSON. The `/api/me/*` routes require an active session
(the user must be logged in). The admin routes (`/api/admin/*`) have no
separate authorization — access to them is not restricted, since this is a
test project with no role-separation requirements.

## Authentication

| Method | Path | Description |
|---|---|---|
| POST | `/api/login` | Body `{username, password}` — a plain comparison against the data in `users`. On a match, creates a session (`req.session.userId`); otherwise returns `401`. |
| POST | `/api/logout` | Ends the user's session. |
| GET | `/api/session` | Returns the currently logged-in user (`{id, username}`) or `401` if there is no session. Used by the user page on load. |

## User dashboard

| Method | Path | Description |
|---|---|---|
| GET | `/api/me/documents` | List of documents assigned to the logged-in user, with the document name and `signedAt`. `401` if not logged in. |
| POST | `/api/me/documents/:assignmentId/sign` | Marks the document as signed: records `signedAt = now` (ISO string). `403` if the document is assigned to a different user; `404` if the record doesn't exist. |

## Admin panel

| Method | Path | Description |
|---|---|---|
| GET | `/api/admin/users` | List of all users **without the password field**. |
| POST | `/api/admin/users` | Body `{username, password}` — creates a new user. |
| GET | `/api/admin/documents` | List of all documents. |
| POST | `/api/admin/documents` | Body `{name}` — creates a new document. |
| GET | `/api/admin/assignments` | List of all assignments with the username, document name, and signing status. |
| POST | `/api/admin/assignments` | Body `{userId, documentId}` — creates a new document assignment for a user with `signedAt: null`. |

## Static files

`login.html`, `dashboard.html`, `admin.html`, and `style.css` are served as
plain static files from the `public/` folder.
