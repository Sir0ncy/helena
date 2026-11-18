# ARCHITECTURE.md — Helena MVP System Specification

## 1. System Overview
Helena combines two decoupled pipelines inside a lightweight two-container Docker Compose stack:
1. **Background Threat Ingestion (`helena-cron`):** Incrementally syncs newly published or modified CVEs from **NVD API 2.0** (`lastModStartDate`) and active exploits from **CISA KEV** into a persistent SQLite database—requiring **zero LLM tokens** to serve the landing page and telemetry dashboard.
2. **On-Demand Contextual Triage (`helena-web`):** A Flask server hosting the 3-page web interface (`/`, `/dashboard`, `/chatbot`) and a provider-agnostic single-LLM agent (`openai` SDK targeting `https://api.hcnsec.cn/v1`, Google Gemini's OpenAI endpoint, or Groq/Ollama) equipped with deterministic tools (`query_osv_packages` and `search_recent_cves`).

```text
+-------------------------------------------------------------------+
|                   Docker Compose (AWS EC2 Host)                   |
|                                                                   |
|  +---------------------+         +-----------------------------+  |
|  |   helena-cron       |         |        helena-web           |  |
|  |  (sync_feeds.py)    |         |   (Gunicorn + Flask App)    |  |
|  |                     |         |                             |  |
|  |  Runs on boot +     |         |  - GET  / (landing.html)    |  |
|  |  every 1 hour       |         |  - GET  /dashboard          |  |
|  |  (delta watermark)  |         |  - GET  /chatbot            |  |
|  +----------+----------+         |  - GET  /health             |  |
|             |                    |  - GET  /api/cves           |  |
|             v                    |  - POST /api/analyze        |  |
|  +----------------------------------+   +------+---------------+  |
|  | Shared Volume: /app/data/        |   |      |               |  |
|  | -> helena.db (SQLite)            |<--+      |               |  |
|  +----------------------------------+          |               |  |
+------------------------------------------------|---------------+--+
                                                 |
                         +-----------------------+
                         | External APIs (HTTPS)
                         v
         +-----------------------------------------------+
         | - OpenAI-Compatible LLM (api.hcnsec.cn/v1     |
         |   or generativelanguage.googleapis.com/...)   |
         | - OSV.dev /v1/querybatch                      |
         | - NVD API 2.0 & CISA KEV JSON                 |
         +-----------------------------------------------+
```

---

## 2. Repository Structure
```text
helena/
├── AGENTS.md           # Auto-loaded rules & constraints for Hermes Agent
├── ARCHITECTURE.md     # System design, DB schema & API contracts (this file)
├── PLAN.md             # Step-by-step MVP execution & verification plan
├── DESIGN.md           # Revised Ferrari-inspired telemetry UI/UX spec
├── USERS-FLOW.md       # 3-route user journey & telemetry chart blueprint
├── PRD.md              # Product Requirements Document (Bahasa Indonesia)
├── Dockerfile          # Python 3.11-slim container image
├── docker-compose.yml  # web + cron-sync services + helena_data volume
├── requirements.txt    # flask, gunicorn, httpx, openai
├── .env.example        # LLM_BASE_URL, LLM_API_KEY, LLM_MODEL, NVD_API_KEY
├── sync_feeds.py       # Incremental NVD + CISA KEV ingestion worker
├── app.py              # Flask routes + OpenAI-compatible tool-calling agent
├── static/
│   └── icon.svg        # Custom Helena app icon & favicon (or icon.png)
└── templates/
    ├── base.html       # Shared layout: favicon, fonts, CDN scripts, 64px nav
    ├── landing.html    # Minimalist marketing & live SQLite proof page (/)
    ├── dashboard.html  # Telemetry KPIs, Chart.js graphs & CVE feed (/dashboard)
    └── chatbot.html    # AI config & CVE analyst workstation (/chatbot)
```

---

## 3. Environment Configuration (`.env.example`)
```dotenv
# Option A: HCNSEC Free Security Gateway (https://api.hcnsec.cn/)
LLM_BASE_URL=https://api.hcnsec.cn/v1
LLM_API_KEY=sk-your-hcnsec-key
LLM_MODEL=DeepSeek-V4-Flash

# Option B: Google Gemini (OpenAI-Compatible Endpoint)
# LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
# LLM_API_KEY=AIza-your-gemini-key
# LLM_MODEL=gemini-3.8-flash

# Optional: Free NVD API key for higher rate limits
NVD_API_KEY=

# Database location (defaults to /app/data/helena.db in Docker)
DB_PATH=/app/data/helena.db
```

---

## 4. Database Schema (SQLite)
Stored at `DB_PATH` (`/app/data/helena.db` in Docker). Created idempotently via `CREATE TABLE IF NOT EXISTS`.

```sql
CREATE TABLE IF NOT EXISTS sync_state (
    key TEXT PRIMARY KEY,
    value TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS cisa_kev (
    cve_id TEXT PRIMARY KEY,
    vendor TEXT,
    product TEXT,
    vuln_name TEXT,
    date_added TEXT,
    due_date TEXT
);

CREATE TABLE IF NOT EXISTS cves (
    cve_id TEXT PRIMARY KEY,
    description TEXT,
    cvss_score REAL,
    severity TEXT,
    published_at TEXT,
    last_modified_at TEXT,
    is_kev INTEGER DEFAULT 0
);

CREATE INDEX IF NOT EXISTS idx_cves_published ON cves(published_at DESC);
CREATE INDEX IF NOT EXISTS idx_cves_severity ON cves(severity);
CREATE INDEX IF NOT EXISTS idx_cves_kev ON cves(is_kev);
```

---

## 5. Web & REST API Contracts

### `GET /`
- **Description:** Renders `templates/landing.html` with live aggregate counts (`total_cves`, `kev_total`, `last_synced_at`) read directly from SQLite (zero LLM calls).

### `GET /dashboard`
- **Description:** Renders `templates/dashboard.html` (KPI strip, Chart.js severity donut & 7-day velocity bar chart, and filterable CVE feed).

### `GET /chatbot`
- **Description:** Renders `templates/chatbot.html` (accepts optional `?cve=CVE-YYYY-NNNN` query parameter to pre-populate a target vulnerability from the dashboard).

### `GET /health`
- **Description:** Container health check and telemetry status endpoint.
- **Response (`200 OK`):**
  ```json
  {
    "status": "ok",
    "active_model": "DeepSeek-V4-Flash",
    "last_synced_at": "2026-11-18T02:00:00.000",
    "cve_count": 142,
    "kev_count": 1245
  }
  ```

### `GET /api/cves`
- **Description:** Returns KPI summary metrics, Chart.js datasets, and filtered CVE feed rows from SQLite.
- **Query Parameters:**
  - `severity` (optional): `CRITICAL` | `HIGH` | `MEDIUM` | `LOW`
  - `kev_only` (optional): `true` | `false`
  - `q` (optional): Substring filter matching `cve_id` or `description`
- **Response (`200 OK`):**
  ```json
  {
    "stats": {
      "reported_24h": 18,
      "critical_count": 19,
      "kev_active_count": 4,
      "high_count": 47,
      "total_cves": 142
    },
    "severity_breakdown": {
      "CRITICAL": 19,
      "HIGH": 47,
      "MEDIUM": 62,
      "LOW": 14
    },
    "daily_velocity": [
      {"date": "2026-11-12", "total": 14, "kev": 0},
      {"date": "2026-11-18", "total": 26, "kev": 2}
    ],
    "cves": [
      {
        "cve_id": "CVE-2026-1234",
        "description": "Buffer overflow in memcpy()...",
        "cvss_score": 9.8,
        "severity": "CRITICAL",
        "published_at": "2026-11-18T01:15:00.000",
        "is_kev": 1
      }
    ]
  }
  ```

### `POST /api/analyze`
- **Description:** Runs the single-LLM agent with OpenAI-compatible tool calling against the user's pasted/uploaded config or CVE question.
- **Request Body (`application/json`):**
  ```json
  {
    "user_input": "flask==3.0.0\nrequests==2.31.0",
    "deployment_context": "Internal worker behind API gateway, no public ingress",
    "history": []
  }
  ```
- **Response (`200 OK`):**
  ```json
  {
    "model_used": "DeepSeek-V4-Flash",
    "report": "## Priority Triage Summary\n..."
  }
  ```
- **Error Response (`400` / `500`):**
  ```json
  {
    "error": "Human-readable error message"
  }
  ```

---

## 6. Single-LLM Tool Specifications (`app.py`)
Use `openai.OpenAI(api_key=os.environ["LLM_API_KEY"], base_url=os.environ["LLM_BASE_URL"])` in a standard tool-calling loop (`max_iterations=4`) with two JSON-schema tools:

1. **`query_osv_packages(packages: list[dict]) -> list[dict]`**
   - Accepts `[{"ecosystem": "PyPI", "name": "requests", "version": "2.31.0"}, ...]`.
   - Calls `POST https://api.osv.dev/v1/querybatch` in a single request.
   - Fetches vulnerability details (`GET /v1/vulns/{id}`) for up to 5 vulns per package (protecting free-tier TPM limits), resolves `CVE-*` aliases, and cross-checks `is_actively_exploited_kev` against the local `cisa_kev` SQLite table.
2. **`search_recent_cves(keyword: str) -> list[dict]`**
   - Queries the local `cves` SQLite table (`WHERE cve_id LIKE ? OR description LIKE ? ORDER BY is_kev DESC, cvss_score DESC LIMIT 10`) so the LLM can inspect newly ingested zero-days clicked from the dashboard.

---

## 7. Container Configuration
- **`Dockerfile`:** Based on `python:3.11-slim`, installs `requirements.txt`, copies `app.py`, `sync_feeds.py`, `static/`, and `templates/`, creates `/app/data`, exposes port `5000`, runs `gunicorn --workers 2 --timeout 120 --bind 0.0.0.0:5000 app:app`.
- **`docker-compose.yml`:**
  - `web`: Maps host port `80:5000` (or `5000:5000` locally), passes `LLM_BASE_URL`, `LLM_API_KEY`, `LLM_MODEL`, and `DB_PATH`, mounts `helena_data:/app/data`, sets `restart: unless-stopped`.
  - `cron-sync`: Shares `helena_data:/app/data`, passes `NVD_API_KEY` and `DB_PATH`, runs `python sync_feeds.py` immediately on startup and loops every `3600` seconds (1 hour), sets `restart: unless-stopped`.