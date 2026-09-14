# Document Signing Tracker

A small test web project for code validation: tracks which users have
signed which documents.

## Idea

- The **admin** enters users and document names into the system, and also
  assigns who needs to sign which document.
- A **user** logs into the site (login/password) and sees the list of
  documents assigned to them.
- When the user clicks "Sign", the date and time of signing appears next to
  the document.

This is deliberately a maximally simple project with no production security
requirements, no build tooling, and no ORM — just enough to validate the
basic logic.

## Tech stack

- **Backend:** Node.js + Express
- **Data storage:** a single JSON file on disk (no database)
- **Frontend:** plain HTML/CSS/JS, no frameworks, no build step
- **Auth:** a simple login + password form (plain string comparison, no
  bcrypt/JWT — acceptable for a test project)

## Project structure (planned)

```
test-demo/
├── package.json
├── server.js            # Express app and all routes
├── lib/
│   └── db.js              # reads/writes the JSON file
├── data/
│   └── db.json             # users, documents, assignments
└── public/
    ├── login.html
    ├── dashboard.html
    ├── admin.html
    └── style.css
```

## Roles and pages

- `login.html` — login form (username + password).
- `dashboard.html` — the user's document list: a "Sign" button, or the
  signing date if the document is already signed.
- `admin.html` — the admin page: add users, add documents, assign documents
  to users. No separate admin authentication — just a page in the same app.

## Documentation

- [docs/DATA_MODEL.md](docs/DATA_MODEL.md) — data structure (users,
  documents, assignments/signatures).
- [docs/API.md](docs/API.md) — description of the server routes
  (endpoints).

## Manual verification scenario

1. The admin opens the admin page, adds a user and a document, and assigns
   the document to the user.
2. The user logs in with their username/password and sees the document in
   the list with the status "not signed".
3. The user clicks "Sign" — the status changes to the date and time of
   signing.
4. On reopening the page, the signing date persists (data is stored in the
   JSON file, not just in memory).
