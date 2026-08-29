# claude-course-starter

Express 4 JSON API over an in-memory store — no database, no build step.

## Commands

- `npm run dev` — start the API on :3000 with `node --watch`
- `npm test` — run the `node:test` suite in `tests/`
- `npm run lint` — ESLint; CI runs lint before tests, so lint failures block the PR

## Conventions

- Use CommonJS `require` / `module.exports`, not ESM `import` — `.eslintrc.json` parses as `script`, so an `import` fails lint, and CI lints before it tests.
- Use one router file per resource in `routes/` mounted in `server.js`, not an inline handler added to `server.js`.
- Use `db/store.js` for every read and write, not module-level state in `routes/`.
- Use a JSON `{ error: "..." }` response with 400 or 404 for bad input or a missing record, not a thrown error.
- Use supertest against the `app` exported from `server.js` in tests, not a real listening port.

## Architecture

`server.js` builds the app, mounts `/users` and `/health`, and calls `listen()` only when run directly, so tests can import `app` without binding a port. `db/store.js` is an in-memory array with a module-level id counter: data resets on every restart, and nothing may assume persistence or stable ids across runs.
