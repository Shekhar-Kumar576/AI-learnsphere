# LearnSphere AI

An interactive AI/ML learning workspace with a Gemini-powered Learning Assistant, video lessons, quizzes, resource uploads, learning history, and AI/ML mock interviews.

## Stack

- **Frontend:** React 19 + TypeScript + Vite
- **Backend:** Python + FastAPI (Vercel Python Function)
- **AI:** Google Gemini `generateContent` API, called only from the backend
- **Deployment:** Vercel (static frontend + `/api/*` Python function)

## Run locally

1. Install Node.js and Python 3.11+.
2. From this project directory:

   ```bash
   npm install
   npm run dev
   ```

3. In a second terminal, install the backend dependencies and run FastAPI:

   ```bash
   python -m venv .venv
   # Windows: .venv\\Scripts\\activate
   # macOS/Linux: source .venv/bin/activate
   pip install -r requirements.txt
   pip install -r backend/requirements.txt
   ```

4. Set the Gemini key in the backend terminal before starting it:

   ```bash
   # Windows PowerShell
   $env:GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
   # macOS/Linux
   export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
   ```

   Then run `uvicorn backend.app.main:app --reload --port 8000`.

   Vite proxies `/api` requests to `http://127.0.0.1:8000` during local development.

## Deploy to Vercel

1. Push this project folder to a **new GitHub repository**. The project root is the folder containing `package.json` and `vercel.json`.
2. In Vercel, choose **Add New → Project**, import the new repository, and keep the project root at the repository root.
3. Vercel should detect Vite. Build command: `npm run build`; output directory: `dist`.
4. In **Project Settings → Environment Variables**, add:
   - `GEMINI_API_KEY` = your key from Google AI Studio
   - `GEMINI_MODEL` = `gemini-2.5-flash` (optional; a default is included)
5. Redeploy after saving the environment variables. Check `/api/health` on your Vercel domain; it should return `ok: true` and `aiConfigured: true`.

**Important:** never put the Gemini key in a `VITE_` variable or frontend code. Vercel's filesystem is ephemeral. The included SQLite database uses `/tmp` in Vercel, so server-side chat/interview history may reset on cold starts. For durable cross-deployment history, connect a hosted database before relying on it for production.

## Features

- Learning Assistant receives the active lesson notes/transcript plus extracted text notes.
- Four lesson tracks: supervised learning (included animated clip), neural networks, gradient descent, and backpropagation (curated full-length video embeds).
- Mock interview question counts: **1, 3, 5, or 7**; AI/ML-focused questions, answer-by-answer feedback, and final summary.
- Interactive practice quiz, browser-local resources and text-note extraction, history, settings, and responsive navigation.
- Anonymous browser ID separates data between browsers; this is not a real account/login system.

## Tests

```bash
pip install -r requirements.txt pytest
PYTHONPATH=backend python -m pytest backend/tests -q
npm run build
```

## API endpoints

`GET /api/health` · `POST /api/chat` · `GET /api/history` · `POST /api/interview/start` · `POST /api/interview/{id}/answer` · `POST /api/interview/{id}/finish` · `GET /api/interview/sessions` · `GET /api/interview/{id}` · `GET /api/stats` · `DELETE /api/me/data`.
