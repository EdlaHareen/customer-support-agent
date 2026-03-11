# 🤖 AI Customer Support Copilot

An AI-powered copilot that helps customer support agents draft empathetic, context-aware replies — powered by **LangChain**, **Groq (Llama 3.1)**, **Mem0**, and **ChromaDB**.

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-0.134-009688?logo=fastapi)
![Streamlit](https://img.shields.io/badge/Streamlit-1.54-FF4B4B?logo=streamlit)
![LangChain](https://img.shields.io/badge/LangChain-1.2-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## ✨ Features

- **AI Draft Generation** — Automatically generates context-aware draft replies using Groq's Llama 3.1 model
- **RAG Knowledge Base** — Searches company FAQ documents (stored in ChromaDB) to ground AI responses in real information
- **Customer Memory** — Remembers past interactions per customer and company using Mem0, so the AI improves over time
- **Tool-Augmented Agent** — The AI can call tools to look up customer plans, SLAs, and open ticket counts before drafting
- **Human-in-the-Loop** — Agents review, edit, accept, or discard drafts before anything is sent
- **Streamlit Dashboard** — A clean web UI for managing tickets, viewing AI context, and probing customer memory

---

## 🏗️ Architecture

```
┌──────────────────────┐        HTTP/JSON        ┌──────────────────────────────┐
│  Streamlit Frontend  │ ◄─────────────────────► │  FastAPI Backend             │
│  (app.py)            │                         │  (main.py)                   │
│                      │                         │                              │
│  • Create tickets    │                         │  ├── API Routers             │
│  • View drafts       │                         │  ├── Services                │
│  • Accept/discard    │                         │  │   ├── SupportCopilot (AI) │
│  • Memory probe      │                         │  │   ├── DraftService        │
└──────────────────────┘                         │  │   └── KnowledgeService    │
                                                 │  ├── Integrations            │
                                                 │  │   ├── Mem0 (Memory)       │
                                                 │  │   ├── ChromaDB (RAG)      │
                                                 │  │   └── Tools (Plan/Load)   │
                                                 │  └── SQLite Database         │
                                                 └──────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.11
- [uv](https://docs.astral.sh/uv/) (Python package manager)
- API Keys:
  - **GROQ_API_KEY** — Get one free at [console.groq.com](https://console.groq.com)
  - **GOOGLE_API_KEY** — Get one at [aistudio.google.com](https://aistudio.google.com)

### Setup

```bash
# Clone the repo
git clone https://github.com/EdlaHareen/customer-support-agent.git
cd customer-support-agent

# Install dependencies
uv sync

# Create .env file with your API keys
cat > .env << EOF
GROQ_API_KEY=your_groq_api_key_here
GOOGLE_API_KEY=your_google_api_key_here
EOF
```

### Run

Start both servers in separate terminals:

```bash
# Terminal 1: Backend (FastAPI)
uv run python main.py

# Terminal 2: Frontend (Streamlit)
uv run streamlit run app.py
```

Then open **http://localhost:8501** in your browser.

### First Steps

1. Click **"Ingest Knowledge Base"** in the sidebar to index the FAQ documents
2. Create a ticket with a customer email, subject, and description
3. The AI will auto-generate a draft reply
4. Review, edit, and accept or discard the draft

---

## 📁 Project Structure

```
├── main.py                          # Backend entry point (FastAPI + Uvicorn)
├── app.py                           # Frontend entry point (Streamlit dashboard)
├── customer_support_agent/
│   ├── api/
│   │   ├── app_factory.py           # FastAPI app setup & lifespan
│   │   ├── dependencies.py          # Dependency injection
│   │   └── routers/                 # API endpoints
│   │       ├── tickets.py           # Ticket CRUD + draft generation
│   │       ├── drafts.py            # Draft review + accept/discard
│   │       ├── knowledge.py         # Knowledge base ingestion
│   │       ├── memory.py            # Customer memory search
│   │       └── health.py            # Health check
│   ├── core/
│   │   └── settings.py              # Configuration (env vars, paths, models)
│   ├── integrations/
│   │   ├── memory/mem0_store.py      # Mem0 customer memory store
│   │   ├── rag/chroma_kb.py          # ChromaDB knowledge base (RAG)
│   │   └── tools/support_tools.py    # AI agent tools (plan lookup, ticket load)
│   ├── repositories/sqlite/          # Database layer (customers, tickets, drafts)
│   ├── schemas/api.py                # Pydantic request/response models
│   └── services/
│       ├── copilot_service.py        # Core AI logic (draft generation, memory)
│       ├── draft_service.py          # Draft lifecycle management
│       └── knowledge_service.py      # Knowledge base ingestion
├── knowledge_base/                   # FAQ markdown files for RAG
├── pyproject.toml                    # Dependencies & project config
├── Dockerfile                        # Container build
└── docker-compose.yml                # Multi-container orchestration
```

---

## 🔧 Tech Stack

| Technology | Purpose |
|-----------|---------|
| [FastAPI](https://fastapi.tiangolo.com/) | Backend API framework |
| [Streamlit](https://streamlit.io/) | Frontend dashboard |
| [LangChain](https://python.langchain.com/) | AI agent framework with tool calling |
| [Groq](https://groq.com/) (Llama 3.1 8B) | LLM for draft generation |
| [Mem0](https://mem0.ai/) | Long-term customer memory |
| [ChromaDB](https://www.trychroma.com/) | Vector database for RAG & memory |
| [Gemini Embeddings](https://ai.google.dev/) | Text embeddings for semantic search |
| [SQLite](https://www.sqlite.org/) | Relational storage for tickets & drafts |

---

## 🐳 Docker

```bash
# Run everything with Docker Compose
docker-compose up --build
```

---

## 🔑 Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GROQ_API_KEY` | ✅ | Groq API key for Llama 3.1 |
| `GOOGLE_API_KEY` | ✅ | Google API key for Gemini embeddings |
| `GROQ_MODEL` | ❌ | Model name (default: `llama-3.1-8b-instant`) |
| `LLM_TEMPERATURE` | ❌ | Response randomness (default: `0.2`) |
| `API_HOST` | ❌ | Backend host (default: `0.0.0.0`) |
| `API_PORT` | ❌ | Backend port (default: `8000`) |

---

## 📄 License

MIT
