# NexSkill AI — Career Intelligence Platform

## Project Overview
- **Name**: NexSkill AI
- **Goal**: AI-powered platform where users upload their resume and receive resume analytics, personalized learning paths, and profile improvement suggestions.
- **Tech Stack**: React + Vite + Tailwind CSS + MUI (Frontend) | Express.js (API Gateway) | Flask + LangChain + Gemini (AI Engine) | MongoDB (Database)

## 📁 Project Structure
```
nexskill-ai/
├── client/          # React + Vite + Tailwind + MUI frontend
├── server/          # Express.js API Gateway
└── ai-engine/       # Flask AI Engine (LangChain + Gemini)
```

## ✅ Completed Features
- **Landing Page** — Hero, Features, How It Works, Testimonials, Pricing CTA
- **Upload Page** — PDF drag-and-drop, target role & company inputs, loading progress
- **Dashboard** — 4 tabs: Overview (Score + Breakdown), Roadmap, Improvements, Skills
- **Pricing Page** — Free/Pro/Teams pricing tiers
- **Responsive** — Mobile + Desktop

## 🚀 Local Setup Guide

### Prerequisites
- Node.js 18+
- Python 3.10+
- MongoDB (local or Atlas)
- Gemini API Key ([get here](https://aistudio.google.com/))

### 1. Clone & Setup

```bash
git clone <repo-url>
cd nexskill-ai
```

### 2. Frontend (client)
```bash
cd client
cp .env.example .env
npm install
npm run dev    # Starts on http://localhost:5173
```

### 3. Express Server (server)
```bash
cd server
cp .env.example .env
# Edit .env — set MONGODB_URI, FLASK_URL, ALLOWED_ORIGINS
npm install
npm run dev    # Starts on http://localhost:5000
```

### 4. Flask AI Engine (ai-engine)
```bash
cd ai-engine
cp .env.example .env
# Edit .env — set GEMINI_API_KEY=your_key
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py              # Starts on http://localhost:8000
```

### 5. Test the full stack
```bash
curl http://localhost:5000/api/health
curl http://localhost:8000/health
```

## 🌩️ Deployment Instructions

### Frontend → Vercel
```bash
cd client
npm run build
npx vercel --prod
# Set env var in Vercel dashboard: VITE_API_URL=https://your-express.onrender.com
```

### Express Server → Render
1. Push to GitHub
2. New Web Service → Connect repo → Root: `server`
3. Build Command: `npm install`
4. Start Command: `npm start`
5. Set env vars: `MONGODB_URI`, `FLASK_URL`, `ALLOWED_ORIGINS`, `PORT=10000`

### Flask AI Engine → Render
1. New Web Service → Root: `ai-engine`
2. Runtime: Python 3.11
3. Build: `pip install -r requirements.txt`
4. Start: `gunicorn app:app --bind 0.0.0.0:$PORT --timeout 120`
5. Set env var: `GEMINI_API_KEY=your_key`

## 🐳 Run with Docker Compose

### 1. Prepare environment
```bash
cd webapp
cp .env.example .env
```

Edit `.env` and set:
- `GOOGLE_API_KEY` (required for Flask AI analysis)
- `MONGODB_URI` (optional; leave empty to run without persistence)

### 2. Build and run all services
```bash
docker-compose up --build
```

This starts:
- `client` at `http://localhost:5173`
- `server` at `http://localhost:3000`
- `ai-engine` at `http://localhost:5000`

Service-to-service communication is preconfigured:
- Frontend routes `/api/*` via Nginx to `server`
- Express calls Flask at `http://ai-engine:5000`

### 3. Stop containers
```bash
docker-compose down
```

To stop and remove built images as well:
```bash
docker-compose down --rmi local
```

## 🗄️ Data Architecture
- **MongoDB Schema**: `Analysis` — stores resumeText, role, company, analytics, learning_path, profile_improvement
- **AI Flow**: Resume PDF → Flask extracts text → Gemini analyzes → 3-module pipeline → JSON response → Express stores → React displays

## 🔑 API Endpoints

### Express Server (port 5000)
| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/upload` | Upload resume + get analysis |
| GET | `/api/results` | Fetch all past analyses |
| GET | `/api/results/:id` | Fetch single analysis |
| GET | `/api/health` | Health check |

### Flask AI Engine (port 8000)
| Method | Path | Description |
|--------|------|-------------|
| POST | `/analyze` | Analyze resume with Gemini |
| GET | `/health` | Health check |

## Last Updated: 2026-03-21
