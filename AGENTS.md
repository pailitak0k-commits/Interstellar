# Base44 Dev Environment

## What this is
Interstellar — a Node.js/Express web proxy (v6.0.0). Serves static frontend from `static/` plus proxy runtime assets (ultraviolet, scramjet, epoxy, bare-mux, libcurl) and a wisp WebSocket handler for `upgrade` requests.

## Running it
- `docker compose -f docker-compose.base44.yml up -d` — starts the app on host port **3000** (container port 8080).
- Uses `node:22-bookworm-slim` with the source bind-mounted at `/app`. `node_modules` lives in a tmpfs to avoid host collisions.
- Deps installed at startup via **pnpm 10.33.0** (corepack). Uses `--no-frozen-lockfile` because the committed `pnpm-lock.yaml` has a `patchedDependencies` config mismatch that breaks frozen installs.
- Runs `node --watch src/server.js` for live reload on file changes (Node's built-in watcher, no nodemon needed).

## Secrets
None required. `config.js` has `challenge: false` (no password protection). No external API keys — the only outbound calls are to `raw.githubusercontent.com` for game assets (`/gh-games/*`).

## Notes
- The repo's own `Dockerfile` builds a production image (`NODE_ENV=production`, `COPY . .`) — do NOT use it for dev; it freezes the source.
- `pnpm-lock.yaml` is stale vs `package.json`'s `patchedDependencies`; if you regenerate the lockfile cleanly, switch back to `--frozen-lockfile`.
- Healthcheck: `GET /` returns 200.
