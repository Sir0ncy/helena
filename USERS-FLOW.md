# USERS-FLOW.md — Helena User Journey & Page Blueprint

## 1. Multi-Page Architecture & Navigation Model

Helena separates passive threat monitoring from active LLM reasoning across **three Flask Jinja2 routes**. This keeps public browsing fast and completely free of LLM token usage while dedicating AI inference exclusively to contextual configuration analysis.

| Route | Template | Purpose | Backend Data Source | LLM Cost |
| :--- | :--- | :--- | :--- | :--- |
| **`/`** | `templates/landing.html` | Minimalist marketing & live proof-of-life entry page | SQLite aggregate counts (`cves`, `cisa_kev`, `sync_state`) | **$0 (Zero calls)** |
| **`/dashboard`** | `templates/dashboard.html` | Real-time CVE telemetry, severity charts & filterable threat feed | `GET /api/cves` (SQLite only) | **$0 (Zero calls)** |
| **`/chatbot`** | `templates/chatbot.html` | Interactive AI analyst for config files, manifests & specific CVEs | `POST /api/analyze` (LLM + OSV.dev + SQLite tools) | **On-demand only** |

### Shared Top Navigation (`64px` Telemetry Bar)
Rendered consistently across `templates/base.html`:
- **Left (Brand Lockup):** Custom **Helena App Icon** (`28px × 28px`, `static/icon.svg`) paired with the uppercase `HELENA` wordmark (`18px / 700 / 2px tracking`), linking to `/`.
- **Center Links:** `DASHBOARD` (`/dashboard`) · `CONFIG ANALYST` (`/chatbot`) — active route highlighted with a `2px` Rosso Corsa (`#da291c`) bottom underline.
- **Right Telemetry:** `LAST SYNC: <UTC timestamp>` and `ENGINE: <LLM_MODEL>` badge.

---

## 2. End-to-End User Flow Diagram

```text
+-------------------------------------------------------------------------+
|  1. LANDING PAGE ( / )                                                  |
|  - Brand Mark: [Helena Icon 48x48] + Scuderia Telemetry Hero            |
|  - Hero: AI-era 30-year-old memcpy() zero-day problem statement         |
|  - Live SQLite Strip: [Tracked CVEs] | [Active KEVs] | [Last Sync]      |
|  - Primary CTA: [OPEN THREAT DASHBOARD]   Secondary: [ANALYZE CONFIG]   |
+------------------------------------+------------------------------------+
                                     |
                 +-------------------+-------------------+
                 v                                       v
+-----------------------------------+   +---------------------------------+
|  2. TELEMETRY DASHBOARD           |   |  3. AI CONFIG ANALYST           |
|     ( /dashboard )                |   |     ( /chatbot )                |
|                                   |   |                                 |
|  - 4 KPI Cards (24h, Crit, KEV)   |   |  - Pre-loaded CVE banner (if    |
|  - Donut Chart (Severity Split)   |   |    navigated via ?cve=...)      |
|  - Bar Chart (7-Day Velocity)     |   |  - Upload/Paste Config (.yaml,  |
|  - Filterable CVE Table           |   |    package.json, requirements,  |
|    [ALL | CRITICAL | HIGH | KEV]  |   |    Dockerfile)                  |
|                                   |   |  - 1-Click Context Chips:       |
|  Click row CTA:                   |   |    [Public] [Behind WAF]        |
|  [ANALYZE IN CHATBOT ->] ---------+-->|    [Internal Worker Only]       |
+-----------------------------------+   |                                 |
                                        |  - LLM Tool Loop:               |
                                        |    OSV.dev Batch + CISA KEV     |
                                        |  - Output: [Helena Icon 24x24]  |
                                        |    Effective Risk + Copy-Paste  |
                                        |    Fix Commands                 |
                                        +---------------------------------+
```

---

## 3. Detailed Page Flows

### Page 1: Landing Page (`/`)
**Goal:** Communicate Helena's value in under 10 seconds using Ferrari's Scuderia editorial aesthetic (`#181818` near-black canvas, sharp `0px` edges, high-contrast typography, and disciplined Rosso Corsa `#da291c` accents) without relying on static mockups.

1. **Hero Section:**
   - **Brand Emblem + Eyebrow:** `48px × 48px` Helena app icon inside a sharp `0px` `#303030` frame (`1px solid #454545`), paired with `AUTONOMOUS ERA APPSEC TRIAGE` (`caption-uppercase`).
   - **Headline (`display-mega` / `56px`):** *"30-Year-Old Bugs Are Breaking at Machine Speed. Patch Only What Actually Matters."*
   - **Subheadline (`body-md`):** Explains how Helena pairs live NVD & CISA KEV threat telemetry with an AI config analyst that filters out unreachable false positives.
   - **Action Bar (`0px` sharp corners):**
     - Primary CTA (`button-primary`, `#da291c`): **`OPEN THREAT DASHBOARD`** $\rightarrow$ navigates to `/dashboard`.
     - Secondary CTA (`button-outline-dark`, `1px solid #ffffff`): **`ANALYZE CONFIG FILE`** $\rightarrow$ navigates to `/chatbot`.
2. **Live Proof-of-Life Telemetry Bar:**
   - Displays 3 live counters injected by Flask from SQLite (`TRACKED CVES`, `CISA KEV ACTIVE EXPLOITS`, `LAST NVD DELTA SYNC`) to demonstrate real-time readiness at zero LLM cost.
3. **3-Column Value Grid (`#303030` elevated panels, `0px` corners):**
   - **01 / Real-Time Threat Radar:** Incremental NVD + CISA KEV delta ingestion every hour.
   - **02 / Deterministic Package Matching:** Batch-checks `package.json`, `requirements.txt`, `go.mod`, and `Dockerfile` against OSV.dev.
   - **03 / Context-Aware Verdict:** Evaluates attack vectors against your server exposure (WAF, internal worker, disabled modules) to eliminate alert fatigue.

### Page 2: CVE Telemetry Dashboard (`/dashboard`)
**Goal:** Give engineers an immediate, scannable picture of today's threat landscape and a 1-click path to test any dangerous CVE against their stack.

1. **Row 1 — 4-Column KPI Telemetry Strip (`tabular-nums`):**
   - **Card 1 — REPORTED TODAY (24H):** Count of CVEs published or modified in the last 24 hours (`#ffffff` numeral).
   - **Card 2 — CRITICAL (`CVSS >= 9.0`):** High-severity alert counter (**Rosso Corsa `#da291c`** numeral).
   - **Card 3 — ACTIVELY EXPLOITED (KEV):** Count of tracked CVEs confirmed in CISA's Known Exploited Vulnerabilities catalog (**Rosso Corsa `#da291c`** numeral).
   - **Card 4 — HIGH SEVERITY (`CVSS 7.0–8.9`):** Secondary remediation queue counter (`#ffffff` numeral).
2. **Row 2 — Visual Telemetry Charts (Rendered via Chart.js CDN):**
   - **Left Chart — Severity Distribution (Donut Chart):**
     - Segments: `Critical` (`#da291c`), `High` (`#f13a2c`), `Medium` (`#f6e500`), `Low / Unscored` (`#4c98b9`), with total tracked CVEs centered inside the ring.
   - **Right Chart — 7-Day Threat Velocity (Stacked Bar Chart):**
     - Plots daily newly published/updated CVEs over the past 7 days, highlighting CISA KEV exploits in **Rosso Corsa (`#da291c`)** against standard CVEs in `#454545` so engineers can spot sudden zero-day spikes.
3. **Row 3 — Interactive Threat Feed Table:**
   - **Filter Toolbar:** Keyword search input (`CVE ID`, `memcpy`, `nginx`, `openssl`), quick filter tabs (`ALL`, `CRITICAL`, `HIGH`), and a prominent `KEV ONLY` toggle switch.
   - **Table Columns:**
     1. `CVE ID` (Monospace `#ffffff`)
     2. `CVSS & SEVERITY` (Pill badge + numeric score)
     3. `EXPLOIT STATUS` (`KEV` solid `#da291c` pill if actively exploited)
     4. `SUMMARY` (2-line truncated advisory description; clicking expands full description)
     5. `PUBLISHED` (UTC date)
     6. `ACTION` (`ANALYZE IN CHATBOT →` sharp `0px` button)
4. **Cross-Page Handoff Action:**
   - Clicking **`ANALYZE IN CHATBOT →`** on `CVE-2026-1234` redirects the user to `/chatbot?cve=CVE-2026-1234`.

### Page 3: AI Config & CVE Analyst (`/chatbot`)
**Goal:** Provide a fast, low-friction analyst workspace where developers upload or paste infrastructure configs and get a contextual exploitability verdict.

1. **Context-Aware Entry State:**
   - **If opened with `?cve=CVE-2026-XXXX`:** Displays a `#303030` banner at the top of the chat thread (`TARGETING CVE-2026-XXXX — Paste or upload your config below to test exploitability`) and automatically passes that CVE ID into the first LLM turn.
   - **If opened directly (`/chatbot`):** Displays a clean welcome card with the `24px × 24px` Helena App Icon badge and 3 starter examples (`Audit requirements.txt`, `Check docker-compose.yml exposure`, `Evaluate package.json CVEs`).
2. **Input & Configuration Workstation (Bottom Docked Composer):**
   - **File Attachment Button (`+ UPLOAD CONFIG`):** Accepts `.yaml`, `.yml`, `.json`, `.txt`, `.toml`, `.mod`, and `Dockerfile` (up to `50 KB`). Reading happens client-side via browser `FileReader` and appends the file's content cleanly into the prompt payload.
   - **1-Click Deployment Context Chips:** Toggleable chips that attach architectural constraints automatically so the LLM can downgrade unreachable CVEs:
     - `Public Internet-Facing`
     - `Behind Reverse Proxy / WAF`
     - `Internal Worker / No Public Ingress`
     - `Containerized / Non-Root`
   - **Prompt / Paste Textarea (`JetBrains Mono`):** Allows pasting raw config snippets or asking follow-up questions (e.g., *"What if we disable HTTP/3 in Nginx?"*).
   - **Client-Side Secret Redaction:** Automatically masks `API_KEY=`, `SECRET=`, `TOKEN=`, and `PASSWORD=` values in browser JS before sending `POST /api/analyze`.
3. **Structured Triage Response (Sanitized Markdown):**
   - Rendered via `marked.js` + `DOMPurify.sanitize()` inside `#121212` inset message cards with `1px solid #303030` borders, prefixed by the `24px × 24px` **Helena App Icon** + `HELENA TRIAGE VERDICT` header:
     - **Executive Risk Verdict:** Clearly separates `CRITICAL / ACTION REQUIRED` (Rosso Corsa accent) from `MITIGATED / FALSE POSITIVE` (Emerald `#03904a` accent) based on the user's deployment context.
     - **Vulnerability Breakdown Table:** `Package / Component` | `CVE ID` | `KEV Status` | `Effective Risk` | `Why It Applies (or Doesn't)`.
     - **Copy-Paste Remediation Commands:** Monospace code blocks (`pip install --upgrade ...`, `npm update ...`, or YAML config patches) with a 1-click `COPY` button.
   - **Multi-Turn Follow-Up:** Keeps the current session's chat history in browser memory so users can ask follow-up hardening questions without re-uploading their config.

---

## 4. Implementation Notes for Hermes Agent

- **Brand Icon & Favicon Integration (`static/`):**
  - Store the custom app icon at `static/icon.svg` (or `static/icon.png` if raster).
  - Add `<link rel="icon" href="{{ url_for('static', filename='icon.svg') }}">` inside `templates/base.html` `<head>`.
  - Include a clean inline SVG fallback (`onerror`) so the brand mark renders crisply even before a custom icon file is dropped into `static/`.
- **Template Inheritance:** Use a shared `templates/base.html` containing Tailwind CSS CDN, Google Fonts (`Inter` + `JetBrains Mono`), `Chart.js` CDN, `marked.js`, `DOMPurify`, and the `64px` top telemetry bar, extended by `landing.html`, `dashboard.html`, and `chatbot.html`.
- **Chart Data Endpoint:** Extend `GET /api/cves` in `app.py` to return:
  - `stats` (`reported_24h`, `critical_count`, `kev_active_count`, `high_count`, `total_cves`)
  - `severity_breakdown` (`{"CRITICAL": int, "HIGH": int, "MEDIUM": int, "LOW": int}`)
  - `daily_velocity` (7-day array of `{"date": "YYYY-MM-DD", "total": int, "kev": int}`)
  - `cves` (filtered list of CVE records)
- **Zero-LLM Guarantee on `/` and `/dashboard`:** Never import or invoke the `openai` client inside `/`, `/dashboard`, or `/api/cves`.