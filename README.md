 # EcoFood

## Overview ✅
EcoFood is a two-part application:

- **Backend:** Node.js + Express API (default port 4005)
- **Frontend:** React + Vite single-page app served via Nginx in production

This repository includes Docker tooling to run the full stack locally and for simple deployments.

---

## Table of Contents
1. Getting Started
2. Prerequisites
3. Local Development (Backend & Frontend)
4. Docker & Docker Compose
5. Environment Variables & Secrets
6. Running Tests & Utilities
7. Troubleshooting
8. Security & Best Practices
9. CI / Deployment Notes
10. Contact / Contributing

---

## 1. Getting Started 🛠️
1. Clone the repo:
   - `git clone <repo_url>`
2. Install dependencies:
   - Backend: `cd backend && npm install`
   - Frontend: `cd frontend && npm install`

---

## 2. Prerequisites ⚙️
- Node.js 18+ / 20 recommended
- Docker & Docker Compose (for containerized runs)
- MongoDB (local or via Docker)
- Optional: Mail testing (MailHog), Grok/OpenAI API key for AI features

---

## 3. Local Development 🚀

### Backend
- Copy env template locally:
  - PowerShell: `Copy-Item backend\.env.example backend\.env`
  - Bash: `cp backend/.env.example backend/.env`
- Fill in `backend/.env` (see Section 5).
- Start dev server:
  - `cd backend && npm run dev` (uses `nodemon`)
- Production start:
  - `npm start` (runs `node server.js`)

### Frontend
- Copy env:
  - `cp frontend/.env.example frontend/.env` and set `VITE_API_URL` if needed.
- Dev server:
  - `cd frontend && npm run dev` (Vite on port 5173)
- Build:
  - `npm run build`
- Preview:
  - `npm run preview`

---

## 4. Docker & Docker Compose 🐳
- Quick (one command):
  - `docker compose up --build`
- What it runs:
  - `mongo` (mongo:6) — data volume `ecofeed_mongo_data`
  - `backend` — built from `backend/Dockerfile`, listens on 4005
  - `frontend` — multi-stage built and served by `nginx` on port 80
  - `mailhog` (dev) — SMTP `1025`, UI `8025`
- In Docker Compose, the backend should use:
  - `MONGODB_URI=mongodb://mongo:27017/ecofood`

---

## 5. Environment Variables & Secrets 🔐
- Do NOT commit real secrets. Use `.env.example` as a template.
- Important backend vars (in `backend/.env`):
  - `MONGODB_URI` — e.g., `mongodb://localhost:27017/ecofood` or `mongodb://mongo:27017/ecofood` (compose)
  - `JWT_SECRET` — strong random string
  - `PORT` — default `4005`
  - `GROK_API_KEY` — optional AI key
  - `EMAIL_USER`, `EMAIL_PASS` — optional email creds
  - `NODE_ENV` — `development` or `production`
- Frontend build envs (prefixed with `VITE_`):
  - `VITE_API_URL` — API endpoint used at build time

> Tip: Generate `JWT_SECRET` with Node:
> `node -e "console.log(require('crypto').randomBytes(48).toString('hex'))"`

---

## 6. Running Tests & Utilities ✅
- The repo includes helper scripts (e.g., `test-chat.js`, `test-grok.js`). Run them with:
  - `node test-chat.js` (from repo root if applicable)
- Add or extend automated tests as needed.

---

## 7. Troubleshooting & Common Issues 🐞
- Backend logs:
  - `docker compose logs -f backend` or run locally and watch console
- DB connection errors:
  - Ensure Mongo URL matches environment (docker vs local)
- Frontend 404s in SPA:
  - Confirm `nginx.conf` includes SPA fallback (`try_files $uri $uri/ /index.html`)
- Mail not received:
  - Use MailHog UI at http://localhost:8025 for local SMTP testing

---

## 8. Security & Best Practices ⚠️
- Never commit `.env` or secrets. `.gitignore` already excludes `.env`.
- Use CI vaults/secret managers in production (AWS Secrets Manager, GitHub Actions secrets, etc.)
- Keep dependencies updated (e.g., upgrade `multer` if a security release appears)
- Monitor and rotate API keys if exposed

---

## 9. CI / Deployment Notes 🔁
- Recommended CI steps:
  1. Build and test both frontend and backend
  2. Build Docker images
  3. Push images to registry (ECR/GCR/ACR/Docker Hub)
  4. Deploy via your orchestrator (Kubernetes / ECS / Docker Swarm / simple VM)

- For frontend env flexibility, either:
  - Provide build-time `VITE_API_URL`, or
  - Use Nginx proxying to backend for runtime routing

---

## 10. Contributing & Contacts 🤝
- Open issues and PRs are welcome. Please follow repository contribution guidelines.
- For urgent security issues, rotate keys and open a private issue with maintainers.

---

### Quick Commands Summary 📌
- Local dev backend: `cd backend && npm run dev`
- Local dev frontend: `cd frontend && npm run dev`
- Build frontend: `cd frontend && npm run build`
- Docker: `docker compose up --build`
- Mailhog UI: http://localhost:8025

---

*This `README.md` was created locally as requested and has NOT been committed to the repository.*
