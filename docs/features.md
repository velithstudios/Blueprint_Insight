# Features — Blueprint Insight

Summary of what Blueprint Insight provides. For installation and quick start, see [Installation](installation) and the [README](https://github.com/velithstudios/Blueprint_Insight#quick-start).

---

## Core experience

- **Single panel** — One Blueprint Insight tab with a collapsible sidebar.
- **Six views** — Project Dashboard, Asset Inspector, History, Runtime Debugger, Settings, Help.
- **Onboarding** — First-run banner, optional scheduled scan, and PIE toast to open the Runtime Debugger.

---

## Project Dashboard

- **Scan Project** — Analyze all Blueprints and detect circular dependencies.
- **Save as Baseline** / **Compare to Baseline** — Track regressions over time.
- **Index for AI** — RAG/GraphRAG indexing for project-wide AI context.
- **Health ring** — 0–100% system status.
- **Metric cards** — Critical Issues, Circular Dependency Loops, Assets Scanned, Architecture (schema + Validate).
- **Drill-down** — Issue list, dependency loop visualization, architecture pass/fail.

---

## Asset Inspector (per-Blueprint)

- **Overview** — Issues, severity, tooltips, quick actions (Open node, Decouple with Interface, Nativize).
- **Chat** — AI conversation with Blueprint/project context; refactor suggestions.
- **Audit Report** — Static report, Performance Audit, UMG Accessibility Audit.
- **Dependency Graph** — Interactive graph with loop highlighting.
- **Heatmap** — Execution heat when available.
- **Export** — UML + Markdown to `Saved/BlueprintInsight/`.
- **Compare to previous revision** — Diff report and chat bubble (e.g. X fixed, Y introduced).
- **Generate Semantic Changelog** — Semantic diff vs previous version.
- **Generate Functional Test** — Creates an `AFunctionalTest` asset.
- **Investigation projects** — Multi-asset projects (max 5), AI over project.
- **Create Investigation from Runtime** — From Runtime Debugger: save logs + trace + assets for AI.

---

## Content Browser

- **Analyze with Blueprint Insight** — Opens the panel and Inspector for selected Blueprint(s).
- **Add to Investigation Project** — Add selected Blueprints to an investigation project.

---

## History

- **Export History** — List of exported files (UML, Audit Reports, snapshots); Open Folder, Refresh.

---

## Runtime Debugger

- **Message traffic log** — Sender, payload type, timestamp.
- **Frame trace** — Blueprint node, actor data, watched vars, visual snapshot.
- **Timeline** — Scrub, play/pause, step back/forward.
- **Frame viewer** — Visual snapshot at a time.
- **Flame graph** — Performance/call visualization.
- **Create Investigation Project from Logs** — Persist logs + trace + assets for AI.
- **In-game** — Gameplay Debugger category (e.g. Numpad 5).

---

## Architecture & CI

- **Architect schema** asset and **Architecture validator** (layer rules) in the Dashboard.
- **Audit commandlet** — Headless project scan; exit code on critical issues (CI).
- **Architecture commandlet** — `-Schema=` path; validate in CI.

---

## Settings

- **AI** — Provider (Ollama / Gemini / OpenAI / Custom), endpoint, model, API key, system prompts.
- **Local LLM** — Hardware tier, graph partitioning, GraphRAG.
- **Runtime** — Passive tracking, phantom debugger, dashcam offset.
- **Project scan** — Scheduled scan, interval, scan on save, notify on critical issues.
- Banned nodes, static analysis limits.

---

