---
version: 1.1
name: helena-scuderia-telemetry
description: A precision-engineered AppSec & CVE telemetry interface adapted from Ferrari's Scuderia editorial design system. Built on a near-black canvas (`#181818`) with elevated graphite panels (`#303030`), crisp 1px hairlines, pure white tabular telemetry numerals, and a single high-voltage brand accent — **Rosso Corsa** (`#da291c`) — reserved strictly for primary triage CTAs, CISA KEV active-exploit alerts, and CVSS 9.0+ critical counters. Sharp `0px` corners govern all structural containers, brand icon frames, and action buttons, conveying race-wall engineering precision.

colors:
  primary: "#da291c"
  primary-active: "#b01e0a"
  primary-hover: "#9d2211"
  ink: "#ffffff"
  body: "#969696"
  body-strong: "#ffffff"
  muted: "#666666"
  muted-soft: "#8f8f8f"
  hairline: "#303030"
  hairline-strong: "#454545"
  canvas: "#181818"
  canvas-elevated: "#303030"
  canvas-inset: "#121212"
  surface-card: "#303030"
  on-primary: "#ffffff"
  on-dark: "#ffffff"
  focus-ring: "#f6e500"
  semantic-critical: "#da291c"
  semantic-high: "#f13a2c"
  semantic-medium: "#4c98b9"
  semantic-low: "#03904a"

typography:
  font-sans: "'Inter', -apple-system, BlinkMacSystemFont, system-ui, sans-serif"
  font-mono: "'JetBrains Mono', 'Fira Code', ui-monospace, SFMono-Regular, monospace"
  display-mega:
    fontFamily: "'Inter', sans-serif"
    fontSize: 56px
    fontWeight: 500
    lineHeight: 1.08
    letterSpacing: -1.12px
  display-lg:
    fontFamily: "'Inter', sans-serif"
    fontSize: 32px
    fontWeight: 500
    lineHeight: 1.15
    letterSpacing: -0.64px
  display-md:
    fontFamily: "'Inter', sans-serif"
    fontSize: 22px
    fontWeight: 500
    lineHeight: 1.3
    letterSpacing: -0.22px
  title-md:
    fontFamily: "'Inter', sans-serif"
    fontSize: 16px
    fontWeight: 700
    lineHeight: 1.25
    letterSpacing: 0
  title-sm:
    fontFamily: "'Inter', sans-serif"
    fontSize: 14px
    fontWeight: 600
    lineHeight: 1.4
    letterSpacing: 0.08px
  body-md:
    fontFamily: "'Inter', sans-serif"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: 0
  body-sm:
    fontFamily: "'Inter', sans-serif"
    fontSize: 13px
    fontWeight: 400
    lineHeight: 1.45
    letterSpacing: 0
  code-mono:
    fontFamily: "'JetBrains Mono', monospace"
    fontSize: 13px
    fontWeight: 500
    lineHeight: 1.45
    letterSpacing: 0
  caption:
    fontFamily: "'Inter', sans-serif"
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.4
    letterSpacing: 0
  caption-uppercase:
    fontFamily: "'Inter', sans-serif"
    fontSize: 11px
    fontWeight: 600
    lineHeight: 1.3
    letterSpacing: 1.1px
    textTransform: uppercase
  button:
    fontFamily: "'Inter', sans-serif"
    fontSize: 13px
    fontWeight: 700
    lineHeight: 1.0
    letterSpacing: 1.4px
    textTransform: uppercase
  nav-link:
    fontFamily: "'Inter', sans-serif"
    fontSize: 12px
    fontWeight: 600
    lineHeight: 1.4
    letterSpacing: 0.8px
    textTransform: uppercase
  number-display:
    fontFamily: "'Inter', sans-serif"
    fontSize: 44px
    fontWeight: 700
    lineHeight: 1.0
    letterSpacing: -1.1px

rounded:
  none: 0px
  xs: 2px
  sm: 4px
  full: 9999px

spacing:
  xxxs: 4px
  xxs: 8px
  xs: 16px
  sm: 24px
  md: 32px
  lg: 48px
  xl: 64px

components:
  brand-icon-lockup:
    iconSize: "28px x 28px"
    gap: 12px
    textColor: "{colors.ink}"
    wordmarkSize: 18px
    wordmarkWeight: 700
    wordmarkTracking: 2.0px
    rounded: "{rounded.none}"
  brand-icon-hero:
    iconSize: "48px x 48px"
    border: "1px solid {colors.hairline-strong}"
    backgroundColor: "{colors.canvas-elevated}"
    rounded: "{rounded.none}"
    padding: 8px
  brand-icon-avatar:
    iconSize: "24px x 24px"
    border: "1px solid {colors.hairline-strong}"
    backgroundColor: "{colors.canvas}"
    rounded: "{rounded.none}"
    padding: 3px
  top-nav-telemetry:
    backgroundColor: "{colors.canvas}"
    borderBottom: "1px solid {colors.hairline}"
    textColor: "{colors.ink}"
    typography: "{typography.nav-link}"
    height: 64px
  kpi-stat-cell:
    backgroundColor: "{colors.canvas-elevated}"
    border: "1px solid {colors.hairline-strong}"
    textColor: "{colors.ink}"
    numberTypography: "{typography.number-display}"
    labelTypography: "{typography.caption-uppercase}"
    rounded: "{rounded.none}"
    padding: 20px 24px
  kpi-stat-cell-alert:
    backgroundColor: "{colors.canvas-elevated}"
    border: "1px solid {colors.hairline-strong}"
    numberColor: "{colors.primary}"
    labelColor: "{colors.body}"
    rounded: "{rounded.none}"
    padding: 20px 24px
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.button}"
    rounded: "{rounded.none}"
    padding: 14px 28px
    height: 48px
  button-primary-active:
    backgroundColor: "{colors.primary-active}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.none}"
  button-outline-dark:
    backgroundColor: transparent
    border: "1px solid {colors.ink}"
    textColor: "{colors.ink}"
    typography: "{typography.button}"
    rounded: "{rounded.none}"
    padding: 10px 16px
    height: 36px
  button-row-action:
    backgroundColor: transparent
    border: "1px solid {colors.hairline-strong}"
    textColor: "{colors.ink}"
    typography: "{typography.caption-uppercase}"
    rounded: "{rounded.none}"
    padding: 6px 12px
  workstation-panel:
    backgroundColor: "{colors.canvas-elevated}"
    border: "1px solid {colors.hairline-strong}"
    textColor: "{colors.ink}"
    rounded: "{rounded.none}"
    padding: 24px
  cve-feed-row:
    backgroundColor: "{colors.canvas}"
    borderBottom: "1px solid {colors.hairline}"
    textColor: "{colors.ink}"
    padding: 16px 20px
  text-input-dark:
    backgroundColor: "{colors.canvas}"
    border: "1px solid {colors.hairline-strong}"
    textColor: "{colors.ink}"
    typography: "{typography.body-md}"
    rounded: "{rounded.sm}"
    padding: 12px 16px
    height: 44px
  textarea-manifest:
    backgroundColor: "{colors.canvas-inset}"
    border: "1px solid {colors.hairline-strong}"
    textColor: "{colors.ink}"
    typography: "{typography.code-mono}"
    rounded: "{rounded.sm}"
    padding: 14px 16px
  badge-kev-pill:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.caption-uppercase}"
    rounded: "{rounded.full}"
    padding: 3px 10px
  badge-severity-pill:
    backgroundColor: "{colors.canvas-elevated}"
    textColor: "{colors.ink}"
    typography: "{typography.caption-uppercase}"
    rounded: "{rounded.full}"
    padding: 3px 10px
  report-output-well:
    backgroundColor: "{colors.canvas-inset}"
    border: "1px solid {colors.hairline}"
    textColor: "{colors.ink}"
    typography: "{typography.body-sm}"
    rounded: "{rounded.none}"
    padding: 20px
---

## Overview

Helena adapts Ferrari's Scuderia editorial and race-telemetry visual language into a focused **AppSec & CVE Triage Workstation**. Instead of automotive marketing photography and white editorial bands, the entire application operates on a cohesive **near-black telemetry floor (`#181818`)** paired with **elevated graphite panels (`#303030`)**, crisp **1px hairline dividers**, and **sharp `0px` architectural edges**.

The single brand voltage is **Rosso Corsa (`#da291c`)**, applied with extreme discipline to signal immediate operational priority: the Helena brand emblem, primary triage trigger buttons, active CISA KEV exploit badges, and CVSS 9.0+ critical telemetry metrics.

**Key Characteristics:**
- **Single Brand Voltage:** `{colors.primary}` (Rosso Corsa `#da291c`) reserved exclusively for the brand mark, primary CTAs, CISA KEV badges, and Critical (`9.0+`) KPI counters.
- **Near-Black Telemetry Floor:** `{colors.canvas}` (`#181818`) base with `{colors.canvas-elevated}` (`#303030`) panels and `{colors.canvas-inset}` (`#121212`) code/manifest wells—never pure `#000000` page backgrounds and no white marketing bands.
- **Typography Pairing:** **Inter** (open-source substitute for FerrariSans, weight `500` for display headings, `700` for tabular KPI numbers and uppercase CTAs) paired with **JetBrains Mono** for CVE IDs, dependency manifests, and remediation commands.
- **Sharp Precision Geometry:** `{rounded.none}` (`0px`) on all CTA buttons, KPI cards, icon frames, feed tables, and workstation panels; `{rounded.sm}` (`4px`) on form inputs; `{rounded.full}` (`9999px`) reserved strictly for compact status pills (`KEV`, `CRITICAL`, `HIGH`).
- **Hairline Elevation:** Depth comes from surface brightness steps (`#121212` -> `#181818` -> `#303030`) and `1px` hairline borders—zero drop-shadow clutter.

## Brand Mark & App Icon

Helena uses a dedicated custom app icon stored in `static/icon.svg` (or `static/icon.png` if provided as raster artwork) to establish an authoritative, Cavallino-inspired Scuderia badge across every surface:

| Surface | Component Token | Dimensions & Framing | Placement & Behavior |
| :--- | :--- | :--- | :--- |
| **Browser Tab** | `<link rel="icon">` | `32px × 32px` SVG/PNG | Loaded in `templates/base.html` via `url_for('static', filename='icon.svg')`. |
| **Top Navigation** | `{components.brand-icon-lockup}` | `28px × 28px` icon + `12px` gap + `HELENA` wordmark | Left slot of the `64px` top bar; links back to `/` (Landing) or `/dashboard`. |
| **Landing Hero** | `{components.brand-icon-hero}` | `48px × 48px` inside a `0px`-sharp `#303030` frame (`1px solid #454545`) | Positioned directly above the hero eyebrow badge on `/`. |
| **Chatbot Analyst** | `{components.brand-icon-avatar}` | `24px × 24px` inside a `0px`-sharp `#181818` frame (`1px solid #454545`) | Paired with `HELENA ANALYST` header on every assistant triage response card. |

### Icon Styling Rules
- **Sharp Framing:** Never crop the app icon inside a circle (`border-radius: 50%`). Always render inside `{rounded.none}` (`0px`) containers with `object-contain` so no edges of the emblem are clipped.
- **Contrast:** Ensure the icon reads clearly against both `#181818` (`canvas`) and `#303030` (`canvas-elevated`).
- **Graceful Fallback:** Include an `onerror` fallback or default inline geometric SVG emblem (a sharp Rosso Corsa `#da291c` & white `#ffffff` shield/crosshair mark) so the UI never displays a broken image icon before the user drops their custom file into `static/icon.svg`.

## Colors

### Brand & Primary Voltage
- **Rosso Corsa** (`{colors.primary}` — #da291c): Brand mark accent, primary CTA fill (`ANALYZE EXPLOITABILITY`), `KEV` active-exploit pills, and `CRITICAL` KPI numerals.
- **Rosso Corsa Active** (`{colors.primary-active}` — #b01e0a): Button press state.
- **Rosso Corsa Hover** (`{colors.primary-hover}` — #9d2211): Button hover fill.
- **Focus Ring Yellow** (`{colors.focus-ring}` — #f6e500): High-contrast keyboard accessibility outline (`2px solid #f6e500`).

### Surfaces & Hairlines
- **Canvas** (`{colors.canvas}` — #181818): Main application background and CVE feed rows.
- **Canvas Elevated** (`{colors.canvas-elevated}` — #303030): KPI cards, triage workstation panel, and default pill fills.
- **Canvas Inset** (`{colors.canvas-inset}` — #121212): Monospace manifest textarea and AI triage Markdown output well.
- **Hairline** (`{colors.hairline}` — #303030): Row dividers and section separators.
- **Hairline Strong** (`{colors.hairline-strong}` — #454545): 1px structural borders around elevated panels, icon frames, and inputs.

### Text Hierarchy
- **Ink** (`{colors.ink}` — #ffffff): Primary headings, wordmark, CVE IDs, KPI numerals, and active labels.
- **Body** (`{colors.body}` — #969696): CVE descriptions, secondary metadata, and AI narrative text.
- **Muted** (`{colors.muted}` — #666666): Timestamps, placeholders, and disabled controls.

### Semantic Vulnerability & Triage Mapping
- **Critical / CISA KEV** (`{colors.semantic-critical}` — #da291c): CVSS `9.0–10.0`, `KEV` actively exploited badge, and `CRITICAL` effective risk verdict.
- **High Severity** (`{colors.semantic-high}` — #f13a2c): CVSS `7.0–8.9` and `HIGH` effective risk verdict.
- **Medium Severity** (`{colors.semantic-medium}` — #4c98b9): CVSS `4.0–6.9` and informational tags.
- **Mitigated / Low** (`{colors.semantic-low}` — #03904a): CVSS `0.1–3.9` and `MITIGATED / LOW` contextual triage verdicts.

## Typography

Load **Inter** (`400, 500, 600, 700`) and **JetBrains Mono** (`400, 500, 700`) via Google Fonts CDN in `templates/base.html`.

| Token | Font | Size | Weight | Line Height | Tracking | Usage |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `{typography.display-mega}` | Inter | 56px | 500 | 1.08 | -1.12px | Landing page hero headline |
| `{typography.display-lg}` | Inter | 32px | 500 | 1.15 | -0.64px | Page headers / main section titles |
| `{typography.display-md}` | Inter | 22px | 500 | 1.3 | -0.22px | Workstation panel heading |
| `{typography.number-display}` | Inter | 44px | 700 | 1.0 | -1.1px | Top KPI telemetry numbers (`tabular-nums`) |
| `{typography.title-md}` | Inter | 16px | 700 | 1.25 | 0 | Report section headings |
| `{typography.body-md}` | Inter | 14px | 400 | 1.5 | 0 | Standard form inputs & body copy |
| `{typography.body-sm}` | Inter | 13px | 400 | 1.45 | 0 | CVE descriptions & report prose |
| `{typography.code-mono}` | JetBrains Mono | 13px | 500 | 1.45 | 0 | CVE IDs, manifests, `pip`/`npm` fix commands |
| `{typography.caption-uppercase}` | Inter | 11px | 600 | 1.3 | 1.1px (uppercase) | KPI labels, table headers, severity pills |
| `{typography.button}` | Inter | 13px | 700 | 1.0 | 1.4px (uppercase) | Primary Rosso Corsa CTA & outline buttons |
| `{typography.nav-link}` | Inter | 12px | 600 | 1.4 | 0.8px (uppercase) | Top navigation telemetry status items |

## Layout & Grid Architecture

- **Max Container Width:** `1360px` centered with `24px` (`{spacing.sm}`) horizontal padding.
- **8px Spacing Ladder:** `xxxs` 4px · `xxs` 8px · `xs` 16px · `sm` 24px · `md` 32px · `lg` 48px · `xl` 64px.
- **Shared Top Telemetry Bar (`64px` height):** Full-width `#181818` header with `1px solid #303030` bottom border. Left: `{components.brand-icon-lockup}` (`28px × 28px` Helena icon + `HELENA` uppercase wordmark). Center: route links (`DASHBOARD`, `CONFIG ANALYST`). Right: live telemetry readouts (`LAST SYNC` and `ENGINE`).

## Do's and Don'ts

### Do
- Keep `{rounded.none}` (`0px`) on every button, KPI card, app icon frame, feed container, and workstation panel for crisp Scuderia engineering precision.
- Reserve `{colors.primary}` (`#da291c`) strictly for the brand emblem, primary CTAs, `KEV` pills, and `CRITICAL` telemetry metrics so urgent threats command instant visual focus.
- Use `tabular-nums` on all CVSS scores, timestamps, and KPI counters so numbers align vertically.
- Style Markdown code blocks inside the triage report with `#181818` background, `1px solid #454545` border, `0px` corners, and `JetBrains Mono` so upgrade commands are easy to spot and copy.

### Don't
- Do **not** clip the Helena app icon inside a circular avatar (`rounded-full`)—keep icon frames sharp at `0px`.
- Do **not** include white editorial bands, car photography heroes, or marketing footers from the original brand guide.
- Do **not** use rounded pill buttons (`border-radius: 9999px` is strictly for small status badges like `KEV` and `CRITICAL`).
- Do **not** use drop shadows (`box-shadow`) for card elevation—rely on `#121212` / `#181818` / `#303030` surface contrast and `1px` hairlines.
- Do **not** use pure `#000000` for the main page floor; keep `#181818` as the base canvas.