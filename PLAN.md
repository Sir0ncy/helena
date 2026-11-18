# AGENTS.md — Helena Project Guardrails

You are building **Helena**, a containerized MVP web application for real-time CVE threat intelligence and contextual LLM vulnerability triage.

Hermes loads this file automatically at session start. Follow every rule below strictly across all tasks.

## 1. Core MVP Principles (Do Not Over-Engineer)

* **Keep the stack minimal:** Python 3.12.3, **Flask**, **Gunicorn**, raw **`sqlite3`**, **`httpx`**, and the official **`openai`** Python SDK.

* **Forbidden additions:** Do **NOT** introduce React, Vue, Next.js, Node/npm build pipelines, SQLAlchemy, Alembic, Celery, Redis, or LangChain.

* **Multi-page Jinja2 frontend:** Render UI via Flask Jinja2 templates (`base.html`, `landing.html`, `dashboard.html`, `chatbot.html`) using Tailwind CSS (CDN), `Chart.js` (CDN), vanilla JavaScript (`fetch`), `marked.js`, and `DOMPurify`.

## 2. Provider-Agnostic Single-LLM Architecture

* **Universal OpenAI-Compatible Client:** Use `openai.OpenAI(api_key=LLM_API_KEY, base_url=LLM_BASE_URL)` with standard Chat Completions function/tool calling so the app works identically across providers without code changes:

  * **HCNSEC Security Gateway:** `LLM_BASE_URL="https://api.hcnsec.cn/v1"`, `LLM_MODEL="DeepSeek-V4-Flash"` (or `glm-5.2`, `Kimi-K2.6`)

  * **Google Gemini (OpenAI endpoint):** `LLM_BASE_URL="https://generativelanguage.googleapis.com/v1beta/openai/"`, `LLM_MODEL="gemini-3.8-flash"`

  * **Local Ollama / Groq:** Any `/v1` OpenAI-compatible endpoint.

* **Never hardcode provider URLs, keys, or model names:** Always read `LLM_BASE_URL`, `LLM_API_KEY`, and `LLM_MODEL` from environment variables.

* **Single agent loop:** Equip this single LLM client with Python tool functions (`query_osv_packages`, `search_recent_cves`)—do not build multi-agent chains.

## 3. Container-First, Incremental Sync & AWS EC2 Rules

* **Environment-driven database path:** Read `DB_PATH = os.environ.get("DB_PATH", "/app/data/helena.db")` (fallback to `./helena.db` if `/app/data` does not exist locally).

* **SQLite thread safety:** Open and close `sqlite3.connect(DB_PATH)` inside each Flask route or tool function. Never share a global SQLite connection across Gunicorn workers.

* **Include `static/` in Docker builds:** Always `COPY static/ ./static/` alongside `templates/` in `Dockerfile` so the custom Helena app icon and favicon resolve cleanly in production.

* **Incremental NVD delta sync:** In `sync_feeds.py`, query NVD API 2.0 using `lastModStartDate` / `lastModEndDate` backed by a `sync_state` watermark table in SQLite so stopped EC2 lab sessions never miss new or updated CVEs.

* **Resilience for AWS Academy EC2:** Containers in `docker-compose.yml` must use `restart: unless-stopped` so the app boots automatically whenever an EC2 lab session restarts.

## 4. UI/UX, Brand Icon & Security Hygiene

* **Visual & flow source of truth:** Follow `@DESIGN.md` and `@USERS-FLOW.md` strictly for all color hex codes (`#181818` canvas, `#303030` elevated surfaces/hairlines, `#da291c` Rosso Corsa primary accent), typography (`Inter` + `JetBrains Mono`), sharp `0px` component corners, and severity badge styles.

* **App icon & favicon:** Render the custom Helena app icon from `static/icon.svg` (via `url_for('static', filename='icon.svg')`) in the `<link rel="icon">` tag, top navbar lockup (`28px × 28px`), landing hero (`48px × 48px`), and chatbot assistant header (`24px × 24px`) inside sharp `0px` frames, with a clean inline SVG fallback so no broken image ever appears.

* **XSS protection:** Always sanitize LLM-generated Markdown with `DOMPurify.sanitize(marked.parse(report))` before injecting into the DOM.

* **Client-side secret redaction:** Before sending user input to `POST /api/analyze`, strip common secret patterns (`API_KEY=`, `SECRET=`, `TOKEN=`, `PASSWORD=`) in browser JavaScript.

## 5. Hermes Execution Workflow

* Work strictly **one milestone at a time** from `@PLAN.md`.

* After implementing a milestone, **verify it yourself** using terminal commands (`docker compose`, `curl`, `sqlite3`) before reporting completion.

* Keep code modular, concise, and readable—prefer clean functions over unnecessary classes.