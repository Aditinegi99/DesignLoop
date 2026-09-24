# Deployment Guide

This app deploys as two separate services: the FastAPI backend on Render, and the React
frontend on Vercel. Both have free tiers with no card required. Do the backend first — you
need its live URL before configuring the frontend.

## Backend — Render

1. Push this repo to GitHub (see README for git commands) — make sure `backend/.env` is
   **not** in it, only `backend/.env.example`.
2. On [render.com](https://render.com), New + → Web Service → connect this repo.
3. Fill in:

| Field | Value |
|---|---|
| Name | `lld-practice-platform-backend` (or anything) |
| Language | Python 3 |
| Branch | `main` |
| Root Directory | `backend` |
| Build Command | `pip install -r requirements.txt` |
| Start Command | `uvicorn app.main:app --host 0.0.0.0 --port $PORT` |

4. Before/after creating it, go to the **Environment** tab and add:

```
GROQ_API_KEY = your_real_key_here
GROQ_MODEL = llama-3.3-70b-versatile
PYTHON_VERSION = 3.11.9
```

   `PYTHON_VERSION` matters — Render's default Python (3.14 at time of writing) doesn't have
   a prebuilt wheel for `pydantic-core`, which fails the build trying to compile it from
   source. `backend/runtime.txt` (already in this repo) pins the same version as a second
   safeguard.

5. Deploy. Once live, visit `https://<your-service>.onrender.com/api/health` and confirm it
   returns `{"status":"ok"}`. Copy that base URL — you need it in the next section.

**Known free-tier behavior:** the service sleeps after ~15 minutes idle and takes 30-60
seconds to wake on the next request. SQLite data can also reset on redeploy/restart — fine
for a demo, not for anything you need to persist.

## Frontend — Vercel

1. On [vercel.com](https://vercel.com), Add New → Project → import the same repo.
2. Root Directory: `frontend`. Vercel auto-detects Vite (Build Command `npm run build`,
   Output Directory `dist`) — leave those as-is.
3. Environment Variables → add:

```
VITE_API_URL = https://<your-render-service>.onrender.com/api
```

   (the exact URL from the backend step, with `/api` on the end — `frontend/src/api/client.js`
   already reads this variable, falling back to a relative `/api` path for local dev.)

4. Deploy. `frontend/vercel.json` (already in this repo) makes sure refreshing on a route like
   `/problem/parking-lot` or `/history` doesn't 404 — Vercel serves everything through
   `index.html` and React Router takes it from there.

## After both are live

Open the Vercel URL, pick a problem, submit something, and confirm real feedback comes back
(not the deterministic-only fallback — that would mean the Render env vars aren't wired
correctly). If something's off, check the Render service's Logs tab first; it'll show the
actual Python error rather than the generic frontend message.
