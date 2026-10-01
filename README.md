# EduGenie — AI-Powered Educational Assistant

EduGenie is a lightweight educational web application built with **FastAPI, HTML, CSS, JavaScript, SQLite, and Ollama**. It helps learners ask questions, understand concepts, practice with AI-generated quizzes, create learning roadmaps, and summarize educational text.

## Features

- **Ask EduGenie:** simple explanations for educational questions.
- **Quiz Generator:** creates MCQ quizzes with four options, answer checking, explanations, scores, and saved results.
- **Learning Path:** generates beginner, intermediate, and advanced study stages with activities and checkpoints.
- **Text Summarizer:** produces a concise summary and key points from pasted text.
- **Quiz History:** stores quiz results in a local SQLite database.
- Responsive interface for desktop and mobile.
- FastAPI Swagger documentation at `/docs`.

## Technology stack

- Python 3.11+
- FastAPI and Uvicorn
- Ollama local inference API
- HTML5, CSS3, vanilla JavaScript
- SQLite

## Requirements

1. Python 3.11 or newer
2. [Ollama](https://ollama.com/) installed and running
3. A compatible Ollama model downloaded (default: `llama3.2:3b`)

The model must fit the memory and performance capabilities of your computer. A cloud AI provider can be integrated by replacing the helper in `app/ai_engine.py`.

## Run locally

### 1. Download or clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/EduGenie.git
cd EduGenie
```

Or download the ZIP from GitHub and extract it.

### 2. Create and activate a virtual environment

**Windows (PowerShell):**
```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start Ollama and download the model

Install Ollama from https://ollama.com/, then run:

```bash
ollama pull llama3.2:3b
```

Ollama normally runs as a local background service. Confirm it is available before starting EduGenie. If using a different model, set `OLLAMA_MODEL` to its installed name.

### 5. Start the web application

From the project root:

```bash
uvicorn app.main:app --reload
```

Open:

- Web app: http://127.0.0.1:8000
- API documentation: http://127.0.0.1:8000/docs
- Health check: http://127.0.0.1:8000/api/health

Keep the terminal running while using the application. Stop the development server with `Ctrl+C`.

## Project structure

```text
EduGenie/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── ai_engine.py
│   └── database.py
├── static/
│   ├── css/style.css
│   └── js/app.js
├── templates/
│   └── index.html
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

## API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/` | Web interface |
| GET | `/api/health` | Service health check |
| POST | `/api/ask` | Ask an educational question |
| POST | `/api/quiz` | Generate an MCQ quiz |
| POST | `/api/learning-path` | Create a study roadmap |
| POST | `/api/summarize` | Summarize text |
| POST | `/api/quiz-result` | Save quiz score |
| GET | `/api/history` | Read saved quiz results |

Try endpoints interactively at `http://127.0.0.1:8000/docs`.

## Example API requests

Question answering:
```json
{
  "topic": "Explain the water cycle in simple English"
}
```

Quiz generation:
```json
{
  "topic": "Pythagoras theorem",
  "count": 5,
  "difficulty": "Beginner"
}
```

Learning path:
```json
{
  "topic": "SQL"
}
```

Text summarization:
```json
{
  "text": "Paste at least 10 characters of educational content here."
}
```

## GitHub submission

1. Create a new repository on GitHub, for example `EduGenie`.
2. Extract this project ZIP.
3. Open a terminal in the extracted project folder.
4. Run:

```bash
git init
git add .
git commit -m "Initial EduGenie project"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/EduGenie.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. Do not commit `.env`, API keys, virtual environments, or your local database.

## Notes and limitations

- AI-generated content can be inaccurate. Learners should verify important academic information with trusted course material.
- Quiz questions are generated dynamically; review them for correctness before using them for formal assessment.
- Quiz history is stored locally in `edugenie.db`. This starter project has no login or cloud synchronization, so it is intended for local/demo use.
- The included server uses development mode (`--reload`). Use a production deployment configuration before exposing it publicly.
- Google Fonts are loaded from an external service; the core application does not depend on them to function.
- The first AI response may take longer while the local model loads.

## Academic project description

**EduGenie** is a lightweight AI-powered educational assistant designed to make learning more accessible through generative AI. It provides question answering, simplified explanations, dynamic quiz generation, personalized learning roadmaps, and educational text summarization. FastAPI exposes the backend endpoints, a responsive HTML/CSS/JavaScript interface provides the user experience, Ollama runs a local language model, and SQLite stores quiz activity.

## License

Choose a license appropriate for your intended use before publishing the repository.


## Deploy online with Render + Gemini

This project can use Gemini in the cloud when `GEMINI_API_KEY` is set. When the key is absent, it uses Ollama locally.

1. Create a GitHub repository and upload the project files (do not upload `.env`, secrets, or `edugenie.db`).
2. Create a Gemini API key in Google AI Studio: https://aistudio.google.com/app/apikey. Review the current usage limits and pricing for your account.
3. Sign in to Render: https://render.com/ and choose **New → Blueprint**.
4. Connect your GitHub account, select your EduGenie repository, and deploy the `render.yaml` Blueprint.
5. When prompted for `GEMINI_API_KEY`, enter the key as a secret/environment variable in Render. Do not put it in GitHub or frontend code.
6. Wait for the build and deployment to finish. Open the `onrender.com` URL shown in your Render service.
7. Test the Ask, Quiz, Learning Path, Summarizer, and `/docs` pages.

### Deployment notes

- The first free-tier request may be delayed if the service has been sleeping. Free-tier availability and limits can change; check Render's current plan details.
- A local SQLite database on a free web service may be temporary and can be lost when the service restarts or redeploys. This starter stores quiz history locally; use a managed persistent database for reliable long-term online history.
- Never publish your Gemini API key. If it is exposed, revoke it and create a new one.
- This is a student/demo starter. Add authentication, abuse protection, logging, and production hardening before public use.
