---
title: Features
layout: default
nav_order: 3
---

# Features

Complete feature list for the Blueprint Insight plugin.

---

## Core experience

- **Single panel** — One Blueprint Insight tab with collapsible sidebar (6 views).
- **Views** — Project Dashboard, Asset Inspector, History, Runtime Debugger, Settings, Help.
- **Onboarding** — First-run banner, scheduled scan, PIE toast to open Runtime Debugger.

---

## Project Dashboard

- **Scan Project** — Full project Blueprint analysis (scanner + circular dependency detection).
- **Save as Baseline** / **Compare to Baseline** — Track regressions over time.
- **Index for AI** — RAG/GraphRAG indexing for project-wide AI context.
- **Health ring** — 0–100% system status.
- **Metric cards:**
  - **Critical Issues** — Count and "View Details" (list with "Open in Inspector").
  - **Circular Dependency Loops** — Count and "Visualize Chain" (cycle view).
  - **Assets Scanned** — Total Blueprints.
  - **Architecture** — Schema picker and **Validate** (layer rules).
- **Drill-down** — Critical issues list, dependency loop visualization, architecture pass/fail.
- **Sync project docs (Live documentation)** — Write scan results to a configurable project docs folder: `BlueprintInsight-Overview.md` (summary, issues, loops) and optionally `BlueprintInsight-ProjectStructure.md` (Blueprints with functions and variables, tied to the LLM index). Option to sync after each scan. Settings → Live Documentation.

---

## Asset Inspector (per-Blueprint)

- **Overview** — Issues list, severity, educational tooltips, quick actions (Open node, Decouple with Interface, Nativize).
- **Chat** — AI conversation with Blueprint/project context; intent detection; refactor suggestions.
- **Audit Report** — Static report, Performance Audit, UMG Accessibility Audit.
- **Dependency Graph** — Interactive graph with loop highlighting.
- **Heatmap** — Execution heat (when available).
- **Export** — UML and Markdown to `Saved/BlueprintInsight/`.
- **Compare to previous revision** (source control) — Diff report and "Blueprint Insight" chat bubble (fixed/introduced counts). **Summarize with AI** uses your configured LLM (e.g. local Ollama) to produce a short human-readable summary of the diff.
- **Generate Semantic Changelog** — Semantic diff vs previous version.
- **Generate Functional Test** — Creates an `AFunctionalTest` asset.
- **Investigation projects** — Multi-asset projects (max 5), move sessions, AI over project.
- **Create Investigation from Runtime** — From Runtime Debugger: save logs, trace, and involved assets for AI analysis.

---

## Content Browser

- **Analyze with Blueprint Insight** — Opens the panel and Inspector for the selected Blueprint(s).
- **Add to Investigation Project** — Adds the selected Blueprints to an investigation project.

---

## History

- **Export History** — List of exported files (UML, Audit Reports, analysis snapshots); Open Folder, Refresh.

---

## Runtime Debugger

- **Message traffic log** — Sender, payload type, timestamp (with repeat count).
- **Log search** — Full-text search over sender and message; filter the log instantly when hunting bugs.
- **Show only issues** — One-click filter to entries containing Error, Failed, Null, Exception, Warning, Collision, Invalid (with highlighting in the list).
- **Time range filter** — Min/Max time boxes and “Use range” to focus the log on a window; all filters (search, issues, type, range) combine.
- **Bookmarks** — Add bookmark at current playback time; list with Jump and Remove; create investigation from bookmarked range (earliest to latest bookmark).
- **Copy log to clipboard** / **Save log to file** — Export the visible (filtered) log for sharing or reports; files saved to `Saved/BlueprintInsight/`.
- **Investigation from range** — Create an investigation using only logs and frames in the current time range (or bookmarked range).
- **Frame trace** — Blueprint node execution, actor location/velocity/movement/collision, watched vars, visual snapshot.
- **Timeline** — Scrub, play/pause, step back/forward.
- **Frame viewer** — Visual snapshot at time.
- **Flame graph** — Performance/call visualization.
- **Create Investigation Project from Logs** — Persist full logs, trace, and assets for AI analysis.
- **In-game** — Gameplay Debugger category (e.g. Numpad 5) when using the Gameplay Debugger.

---

## Architecture and CI

- **Architect schema** — Define layer rules; use the Dashboard schema picker and **Validate**.
- **Audit commandlet** — Headless project scan; non-zero exit code when critical issues exist (for CI).
- **Architecture commandlet** — Validate against a schema from the command line (e.g. `-Schema=/Path/To/Schema.Schema`) for CI.

---

## Settings

- **AI** — Provider (Ollama / Gemini / OpenAI / Custom), endpoint, model, API key, system prompts (static + runtime).
- **Local LLM** — Hardware tier, graph partitioning, GraphRAG.
- **Runtime** — Passive tracking, phantom debugger, dashcam offset.
- **Project scan** — Scheduled scan, interval, scan on save, notify on critical issues.
- Banned nodes, static analysis limits.

---

## Other

- **Performance Benchmark** Blueprint node.
- Hot reload sentinel, dependency graph cache, validators.
