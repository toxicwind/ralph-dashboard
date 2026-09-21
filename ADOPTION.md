# ADOPTION — ralph-dashboard canonical home

**Status:** adopted 2026-09-20 by Ember's pack (agent ralph-adopter). The earlier
"orphan" report was stale: this tree is a live git repo, fully synced with
`toxicwind/ralph-dashboard` on GitHub.

## Canonical home decision

**Home: `toxicwind/ralph-dashboard`** (this repo), NOT a subdir of
`toxicwind/sovereign-projects`.

Justification: this is a fork of `Endogen/ralph-dashboard` with yote-specific
fixes (durability commit `3e783a3`: discovery cache, .archive prune, psutil).
Folding it into sovereign-projects would complicate fork ancestry and upstream
sync while gaining nothing — the fork keeps its own release cadence.
`sovereign-projects` holds only the *supervision contract* (pitchfork daemon
entry), which references the live working path.

## Live topology (yote)

- Working copy: `/home/toxic/ralph-dashboard` (branch `main`, matches origin/main)
- Backend: FastAPI/uvicorn from `backend/` using `backend/.venv`
- Port: `127.0.0.1:25194` — health: `GET http://127.0.0.1:25194/api/health`
- Supervision: pitchfork daemon `sovereign/ralph-dashboard` (defined in
  `sovereign-projects` `pitchfork.toml` on main; `boot_start=true`, `retry=true`)
- Frontend: `frontend/` (Vite); build output is not served from this repo

## Durability notes

- Runtime data (sqlite/projects under `data/`) stays untracked on yote.
- Secrets: `.env` is gitignored; copy `.env.example` on fresh checkouts.
- Never restart the supervisor (pitchfork) for dashboard work — use
  `pitchfork stop/start sovereign/ralph-dashboard` from `/home/toxic/sovereign`.
