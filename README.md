🤖 PM Agent — AI-Powered Project Management

An autonomous project management agent that connects Jira with AI — extract requirements from documents, auto-generate tickets, decompose large stories, and manage your sprint through a chat interface or Claude Desktop via MCP.


📌 About
🤖 Autonomous AI-powered Project Management Agent that connects Jira with AI to automate your entire project management workflow.
📄 DOCUMENT PROCESSING
✅ Upload any PDF or requirements document
✅ AI extracts all actionable requirements automatically
✅ Converts raw text into structured, professional specifications
🎫 TICKET MANAGEMENT
✅ Auto-generates Jira tickets (Epic, Story, Task, Bug)
✅ AI assigns priority — Critical / High / Medium / Low
✅ Auto-calculates story points (Fibonacci scale)
✅ Generates acceptance criteria for every ticket
✅ Suggests assignee role — Backend, Frontend, QA, DevOps
✅ Auto-labels and tags tickets
✅ Create, update, and delete tickets from the dashboard
🧩 SPRINT PLANNING
✅ AI complexity scoring (0.0 – 1.0) for every ticket
✅ Auto-decomposes large stories (>8 pts) into 3–7 sub-tasks
✅ Each sub-task independently completable in 1–3 days
✅ Automatically assigns tickets to Sprint 1 / 2 / 3
✅ Flags oversized tickets before they enter the sprint
🔄 JIRA SYNC
✅ Bi-directional sync with your Atlassian workspace
✅ Configurable sync interval (default every 30 seconds)
✅ Full Jira REST API integration
🤖 CLAUDE DESKTOP MCP INTEGRATION
✅ Exposes a full MCP server — manage Jira via natural language
✅ Ask Claude: "Show open tickets", "Create a story", "Decompose PM-42"
✅ Works with Claude Desktop and any MCP-compatible client
✅ Installable as a PyPI package — pm-agent-jira
📊 DASHBOARD & OBSERVABILITY
✅ React dashboard for visual ticket management
✅ View, create, edit, and manage tickets from the UI
✅ All AI prompts tracked and editable live via Langfuse
✅ No redeployment needed to update AI behavior
🐳 DEPLOYMENT
✅ Fully Dockerized — one command: docker compose up --build
✅ Backend: FastAPI on Python 3.11
✅ Frontend: React + Vite served via nginx
✅ Supports local dev and production deployments
🛠 TECH STACK
LayerTechnologiesBackendFastAPI · LangGraph · LangChain · Python 3.11FrontendReact · Vite · nginxAIAzure OpenAI (GPT) · LangGraph agentJiraAtlassian REST API · bi-directional syncMCPModel Context Protocol · Claude DesktopObservabilityLangfuse · prompt versioningAuthPyJWT · python-josePDFpdfplumber · pdfminerDeployDocker · Docker Compose · PyPI

✨ Features

Document → Tickets — Upload a PDF or requirements doc and let the agent extract, classify, and create Jira issues automatically
Smart Decomposition — Stories over 8 points are automatically split into 3–7 independently completable sub-tasks
Sprint Planning — AI-powered complexity scoring and sprint assignment (Sprint 1/2/3)
Jira Sync — Bi-directional sync with your Atlassian workspace on a configurable interval
Claude Desktop Integration — Use as an MCP server so you can manage your Jira board directly from Claude Desktop
Prompt Observability — All AI prompts tracked via Langfuse for monitoring and iteration
React Dashboard — Clean frontend for viewing, creating, and managing tickets


🏗️ Architecture
┌─────────────────────┐     ┌──────────────────────────────────────┐
│   React Frontend    │────▶│         FastAPI Backend               │
│   (Vite + nginx)    │     │         (Python 3.11)                 │
└─────────────────────┘     │                                       │
                             │  ┌────────────────────────────────┐  │
┌─────────────────────┐     │  │     MCP Server  (pm-agent mcp) │  │
│   Claude Desktop    │────▶│  │  Tools: list_tickets,          │  │
│   (or Claude.ai)    │     │  │  create_ticket, decompose, ... │  │
└─────────────────────┘     │  └────────────────────────────────┘  │
                             └──────────────┬───────────────────────┘
                                            │
              ┌─────────────────────────────┼────────────────────────┐
              ▼                             ▼                        ▼
     ┌─────────────────┐    ┌─────────────────────┐   ┌─────────────────┐
     │  Jira REST API  │    │  Azure OpenAI / GPT │   │    Langfuse     │
     │  (Atlassian)    │    │  (LangGraph agent)  │   │  (Observability)│
     └─────────────────┘    └─────────────────────┘   └─────────────────┘
PM Agent has two interfaces:

Web Dashboard — React frontend for visual ticket management
MCP Server — Exposes Jira + AI tools directly to Claude Desktop or any MCP-compatible client, so you can manage your entire project board through natural language conversation


🚀 Quick Start (Docker)
Prerequisites

Docker and Docker Compose
A Jira account with an API token
An Azure OpenAI resource (or compatible OpenAI endpoint)
A Langfuse account (free tier works)

1. Clone and configure
bashgit clone https://github.com/your-org/pm-agent.git
cd pm-agent

cp .env.example .env
# Edit .env with your credentials (see Environment Variables below)
2. Upload prompts to Langfuse
bashpip install langfuse python-dotenv
python upload_prompts.py
3. Start with Docker Compose
bashdocker compose up --build
ServiceURLFrontendhttp://localhost:5173Backend APIhttp://localhost:8001API Docshttp://localhost:8001/docs

⚙️ Environment Variables
Copy .env.example to .env and fill in the values:
env# Azure OpenAI
AZURE_OPENAI_API_KEY=your-key
AZURE_OPENAI_ENDPOINT=https://your-resource.cognitiveservices.azure.com/
AZURE_OPENAI_DEPLOYMENT=gpt-5-nano
AZURE_OPENAI_API_VERSION=2024-08-01-preview

# Langfuse (prompt tracking)
LANGFUSE_SECRET_KEY=sk-lf-...
LANGFUSE_PUBLIC_KEY=pk-lf-...
LANGFUSE_BASE_URL=https://cloud.langfuse.com

# Jira
JIRA_BASE_URL=https://your-org.atlassian.net
JIRA_EMAIL=you@yourcompany.com
JIRA_API_TOKEN=your-jira-api-token

# App
SECRET_KEY=change-me-to-a-long-random-string
FRONTEND_URL=http://localhost:5173
JIRA_SYNC_INTERVAL=30   # seconds between Jira syncs

🖥️ Claude Desktop MCP Integration
This project ships as a pip package (pm-agent-jira) so you can wire it directly into Claude Desktop as an MCP server.
Install the package
bashpip install pm-agent-jira
Configure Claude Desktop
Add the following to your claude_desktop_config.json (usually at ~/Library/Application Support/Claude/claude_desktop_config.json on macOS):
json{
  "mcpServers": {
    "pm-agent": {
      "command": "pm-agent",
      "args": ["mcp"],
      "env": {
        "JIRA_BASE_URL": "https://your-org.atlassian.net",
        "JIRA_EMAIL": "you@yourcompany.com",
        "JIRA_API_TOKEN": "your-jira-api-token",
        "AZURE_OPENAI_API_KEY": "your-key",
        "AZURE_OPENAI_ENDPOINT": "https://your-resource.cognitiveservices.azure.com/",
        "AZURE_OPENAI_DEPLOYMENT": "gpt-5-nano",
        "AZURE_OPENAI_API_VERSION": "2024-08-01-preview"
      }
    }
  }
}
Restart Claude Desktop — you can now ask Claude things like:

"Show me all open tickets in the PM-AGENT project"
"Create a story for user authentication"
"Decompose ticket PM-42 into sub-tasks"


🛠️ Local Development (without Docker)
Backend
bashcd backend
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp ../.env.example ../.env       # fill in your values
uvicorn backend.main:app --reload --port 8001
Frontend
bashcd frontend
npm install
npm run dev                      # runs on http://localhost:5173

📦 Building the PyPI Package
bash# Builds frontend, copies static files, then creates wheel + sdist
python build_package.py

# Publish
twine upload dist/*

🧩 AI Prompts
Four prompts power the agent pipeline. They live in Langfuse and can be edited without redeploying:
Prompt NamePurposepm-agent-extract-requirementsParses a document and returns structured JSON ticketspm-agent-classify-complexityFlags tickets too large for a single sprintpm-agent-decompose-ticketBreaks a large ticket into 3–7 sub-taskspm-agent-enhance-requirementTransforms raw text into a professional specification
Run python upload_prompts.py to push all prompts to your Langfuse instance.

🗂️ Project Structure
pm-agent/
├── backend/            # FastAPI app + LangGraph agent
│   └── Dockerfile
├── frontend/           # React + Vite dashboard
│   └── Dockerfile
├── pm_agent/           # Installable Python package (CLI + MCP server)
│   └── static/         # Built frontend (auto-populated by build_package.py)
├── docker-compose.yml
├── pyproject.toml
├── requirements.txt
├── build_package.py    # One-shot build script
├── upload_prompts.py   # Langfuse prompt uploader
└── .env.example
