# Data Model

All data is stored in a single JSON file (`data/db.json`) as three arrays:
`users`, `documents`, `assignments`.

```json
{
  "users": [
    { "id": 1, "username": "alice", "password": "secret123" }
  ],
  "documents": [
    { "id": 1, "name": "NDA Agreement" }
  ],
  "assignments": [
    { "id": 1, "userId": 1, "documentId": 1, "signedAt": null }
  ]
}
```

## Entity descriptions

### `users`
The list of accounts the admin can create.

| Field | Type | Description |
|---|---|---|
| `id` | number | unique identifier |
| `username` | string | login name |
| `password` | string | password (stored as a plain string — acceptable for a test project, but never returned in API responses) |

### `documents`
A reference list of document names that need to be signed.

| Field | Type | Description |
|---|---|---|
| `id` | number | unique identifier |
| `name` | string | document name |

### `assignments`
The join table linking a user to a document — this is where it's recorded
who was assigned which document and whether it has been signed.

| Field | Type | Description |
|---|---|---|
| `id` | number | unique identifier |
| `userId` | number | reference to `users.id` |
| `documentId` | number | reference to `documents.id` |
| `signedAt` | string \| null | `null` until the document is signed; ISO date/time of signing after the user clicks "Sign" |

## Notes

- User sessions (who is currently logged in) are not stored in the file —
  this is transient in-memory state on the server (an in-memory session
  store) that resets when the server restarts. That's sufficient for a test
  project.
- One `assignments` record = one document assigned to one user. If a
  document needs to be assigned to several users, one record is created per
  user.
