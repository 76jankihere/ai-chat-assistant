# Chat AI App

A production-ready chat workspace that blends real-time messaging with an AI writing partner. The app uses **Stream Chat** for conversations and **OpenAI** for assisted writing and research (via Tavily search), wrapped in a modern React UI.

**Live Demo:** https://ai-chat-assistant-1-sct6.onrender.com/

---

## ✨ Highlights

- **Realtime Chat** – presence, typing indicators, reactions, threads  
- **AI Writing Assistant** – drafts, rewrites, summaries, and ideas  
- **Web Search** – pulls fresh facts via Tavily when needed  
- **Polished UI** – React + Tailwind + shadcn/ui with dark mode  
- **Per-channel AI agent** – created on demand, auto-cleaned when idle  
- **JWT auth** – secure Stream Chat token generation  
- **Deploy friendly** – env-driven config for local and cloud

---

## 🧭 Structure

```
ai-chat-assistant/
├─ nodejs-ai-assistant/         # Backend (Express + TypeScript)
│  ├─ src/
│  ├─ .env.example
│  └─ package.json
├─ react-stream-ai-assistant/   # Frontend (React + Vite + TypeScript)
│  ├─ src/
│  ├─ .env.example
│  └─ package.json
└─ README.md
```

---

## 🧰 Tech Stack

**Frontend**
- React + TypeScript + Vite  
- Stream Chat React components  
- Tailwind CSS, shadcn/ui, Radix primitives  
- React Router, React Hook Form

**Backend**
- Node.js + Express (TypeScript)  
- Stream Chat server SDK  
- OpenAI SDK  
- Tavily (web search)  
- CORS, dotenv

---

## ✅ Prerequisites

- Node.js **v20+**  
- Accounts & API keys:
  - **Stream Chat**
  - **OpenAI**
  - **Tavily** (web search)

---

## 🔐 Configuration

### 1) Backend (`nodejs-ai-assistant/.env`)

Create from the example:

```bash
cd nodejs-ai-assistant
cp .env.example .env
```

Fill in:

```env
# Stream Chat (server-side)
STREAM_API_KEY=your_stream_api_key
STREAM_API_SECRET=your_stream_api_secret

# OpenAI (server-side)
OPENAI_API_KEY=your_openai_api_key

# Tavily search (server-side)
TAVILY_API_KEY=your_tavily_api_key

# Optional: lock CORS in production
FRONTEND_ORIGIN=https://ai-chat-assistant-1-sct6.onrender.com
```

### 2) Frontend (`react-stream-ai-assistant/.env`)

```bash
cd react-stream-ai-assistant
cp .env.example .env
```

Set:

```env
# Stream Chat (public key used in browser)
VITE_STREAM_API_KEY=your_stream_api_key

# Backend base URL
# Local development:
VITE_BACKEND_URL=http://localhost:3000
# Production example (replace with your backend URL):
# VITE_BACKEND_URL=https://your-backend.onrender.com
```

> Keep real `.env` files **out of git**. Commit only the `.env.example` placeholders.

---

## ▶️ Run Locally

### Backend

```bash
cd nodejs-ai-assistant
npm install
npm run dev
# -> http://localhost:3000
```

### Frontend

```bash
cd react-stream-ai-assistant
npm install
npm run dev
# -> http://localhost:8080
```

Open `http://localhost:8080`, enter a username, and start chatting.

---

## 📡 API Endpoints (Backend)

| Method | Path              | Purpose                              |
|------: |-------------------|--------------------------------------|
| GET    | `/`               | Health/status JSON                   |
| POST   | `/token`          | Create Stream Chat auth token        |
| POST   | `/start-ai-agent` | Initialize an AI agent for a channel |
| POST   | `/stop-ai-agent`  | Dispose the agent and clean up       |
| GET    | `/agent-status`   | `connected` / `connecting` / `disconnected` |

---

## 🤖 AI Agent Lifecycle

1. **Spin-up** – on demand per channel (creates agent user, joins channel)  
2. **Work** – handles messages, calls OpenAI, performs web lookups via Tavily  
3. **Idle cleanup** – background task disposes inactive agents automatically

---

## 🚀 Deploy

### Backend → Render (Web Service)

- **Root Directory**: `nodejs-ai-assistant`  
- **Build Command**: `npm ci`  
- **Start Command**: `npm run start`  
- **Environment Variables**: same as backend `.env`  
- **CORS**: set `FRONTEND_ORIGIN=https://ai-chat-assistant-1-sct6.onrender.com`

### Frontend → Render (Static Site)

- **Root Directory**: `react-stream-ai-assistant`  
- **Build Command**: `npm ci && npm run build`  
- **Publish Directory**: `dist`  
- **Env Vars**:
  - `VITE_STREAM_API_KEY=...`
  - `VITE_BACKEND_URL=https://<your-backend-web-service>.onrender.com`
- **Redirects/Rewrites**: add `/*  ->  /index.html` (Rewrite) for SPA routing

Deployed app: **https://ai-chat-assistant-1-sct6.onrender.com/**

> On free plans, Render may “sleep” after inactivity; the first request can take a few seconds while the backend wakes up.

---

## 🔒 Security Notes

- Never expose server secrets in the frontend. Only `VITE_*` vars go to the browser.  
- Lock CORS to your exact frontend origin in production.  
- Rotate keys immediately if ever leaked.  
- Validate inputs and return safe errors from all endpoints.

---

## 🧩 Troubleshooting

- **CORS errors** → Make sure `FRONTEND_ORIGIN` on the backend matches your static site URL exactly (scheme + host). Redeploy backend.  
- **Frontend still calling localhost in prod** → Set `VITE_BACKEND_URL` on the static site, then redeploy the frontend.  
- **Port 3000 already in use (local)** → Stop the existing process or change `PORT` temporarily, then rerun `npm run dev`.

---

## 🤝 Contributing

1. Create a feature branch  
2. Make focused changes with clear commits  
3. Update `.env.example` if config changes  
4. Open a PR with a short demo/screenshot

---

## 📄 License

MIT — see `LICENSE` for details.
