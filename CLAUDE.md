# CLAUDE.md

Starter Express API for the Claude Code course — a minimal REST service used as a real codebase for practicing Claude Code setup (not a production app).

## Commands

- `npm install` — install dependencies
- `npm run dev` — start the API with auto-reload (`node --watch server.js`) on http://localhost:3000
- `npm start` — start the API without watch mode
- `npm test` — run all tests (`node --test`, using `node:test` + `supertest`)
- `npm test -- tests/users.test.js` — run a single test file
- `npm run lint` — check code style with ESLint

## Architecture

- `server.js` — entry point; builds the Express `app`, mounts routes, and only calls `app.listen` when run directly (`require.main === module`), so tests can `require("../server")` and hit routes via `supertest` without opening a real port.
- `routes/` — one file per resource (`users.js`, `health.js`), each exporting an `express.Router()` mounted in `server.js`.
- `db/store.js` — in-memory data access layer; all data is non-persistent and resets on restart. Routes call into this module rather than manipulating data directly.
- `tests/` — one test file per resource, exercising routes through `supertest` against the exported `app`.
- CI (`.github/workflows/ci.yml`) runs `npm install`, `npm run lint`, and `npm test` on every push/PR.

## Conventions

- Use `db/store.js` for data access; not the `users` array.
- Real secrets belong in `.env` (git-ignored); `.env.example` documents the shape but is never filled with real values.
