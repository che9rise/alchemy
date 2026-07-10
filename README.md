# Alchemy 🧪

Full-stack chatbot platform for creating and chatting with custom AI characters. Built with React/Type
Script, C#/.NET, and a Python AI service.

<!-- TODO: add a screenshot or GIF of the chat UI here once Phase 2 is done -->

## Architecture

```
┌─────────────┐      ┌──────────────────┐      ┌──────────────────┐      ┌──────────┐
│  React/TS   │ ───► │  ASP.NET Core    │ ───► │  Python AI       │ ───► │ DeepSeek │
│  client     │      │  Web API         │      │  service (FastAPI)│      │   API    │
└─────────────┘      └──────────────────┘      └──────────────────┘      └──────────┘
                      characters, chat          prompt construction,
                      history (EF Core +        RAG retrieval,
                      SQLite)                   LLM calls
```

- The **C# API** is the system of record — it owns characters, chat history, and persistence, and is t
he only thing the client talks to.
- The **Python service** owns everything AI: building system prompts from character definitions, retri
eval-augmented generation (RAG), and calls to the LLM.
- The **DeepSeek API key never leaves the Python service**, and the client never talks to the LLM dire
ctly.

## Tech stack

| Layer | Tech |
|---|---|
| Frontend | React, TypeScript, Vite, Tailwind CSS |
| Backend API | ASP.NET Core Web API (.NET), EF Core, SQLite |
| AI service | Python, FastAPI, OpenAI SDK (DeepSeek-compatible), ChromaDB |
| Infra | Docker Compose, GitHub Actions CI |

## Features

- [ ] Create, edit, and delete custom AI characters (a character = name, avatar, personality → system
prompt)
- [ ] Multi-turn chat with conversation memory
- [ ] Streaming responses (token-by-token, relayed through both services as Server-Sent Events)
- [ ] Character knowledge documents via RAG — upload a "lore" file, chunks are embedded and stored in
ChromaDB, relevant passages retrieved into the prompt at chat time
- [ ] Tool use via LLM function calling
- [ ] Persistent chat history

## Running locally

Prerequisites: Node 20+, .NET 8 SDK, Python 3.11+

```bash
# 1. Client
cd client
npm install
npm run dev

# 2. API
cd server
dotnet run

# 3. AI service
cd ai
python -m venv .venv
.venv\Scripts\activate        # Windows
pip install -r requirements.txt
uvicorn main:app --reload
```

Create `ai/.env` with your DeepSeek API key (never committed):

```
DEEPSEEK_API_KEY=sk-...
```

<!-- TODO: replace with `docker compose up` once Phase 7 lands -->

## Roadmap

| Phase | Goal | Status |
|---|---|---|
| 0 | Project skeleton (client / server / ai) | 🔨 in progress |
| 1 | Health-check chain wired end to end | ⬜ |
| 2 | Plain chat through both services to DeepSeek | ⬜ |
| 3 | Character CRUD + chat-as-character | ⬜ |
| 4 | Streaming responses | ⬜ |
| 5 | RAG: character knowledge documents | ⬜ |
| 6 | Function calling | ⬜ |
| 7 | Docker, CI, tests, deploy | ⬜ |

## License

[MIT](LICENSE)