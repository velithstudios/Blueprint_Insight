---
title: Documentation
layout: default
nav_order: 4
---

# Documentation

Installation, features, and usage for Blueprint Insight.

---

## Installation

1. **Download** the plugin from [Fab](https://www.fab.com/) or your distribution source.
2. **Copy** the `Blueprint_Insight` folder into your project’s `Plugins` directory:
   ```
   YourProject/Plugins/Blueprint_Insight
   ```
3. **Restart** the editor (or build the project) so the plugin compiles and loads.
4. **Open** **Window → Blueprint Insight**, or right-click a Blueprint in the Content Browser and choose **Analyze with Blueprint Insight**.

---

## Quick start

1. **Open the panel** — Window → Blueprint Insight.
2. **Run a project scan** — On the **Project Dashboard**, click **Scan Project** to analyze all Blueprints and detect critical issues and circular dependencies.
3. **Inspect an asset** — In the Content Browser, right-click a Blueprint → **Analyze with Blueprint Insight**, or select a Blueprint and switch to the **Asset Inspector** tab.
4. **Use AI (optional)** — In **Settings**, choose an AI provider (e.g. Local Ollama, Google Gemini, or OpenAI) and optionally add an API key. Then use the **Chat** tab in the Inspector for analysis and refactor suggestions.
5. **Runtime trace** — Start PIE; use the in-editor toast or the **Runtime Debugger** tab to view message traffic and frame trace, and optionally **Create Investigation Project from Logs** for AI analysis.

---

## Project Dashboard

- **Scan Project** — Full project Blueprint analysis (scanner + circular dependency detection).
- **Save as Baseline** / **Compare to Baseline** — Track regressions over time.
- **Index for AI** — RAG/GraphRAG indexing for project-wide AI context.
- **Health ring** — 0–100% system status.
- **Metric cards:**
  - **Critical Issues** — Count and “View Details” (list with “Open in Inspector”).
  - **Circular Dependency Loops** — Count and “Visualize Chain” (cycle view).
  - **Assets Scanned** — Total Blueprints.
  - **Architecture** — Schema picker and **Validate** (layer rules).
- **Drill-down** — Critical issues list, dependency loop visualization, architecture pass/fail.
- **Sync project docs** — Write current scan results to a project documentation folder (e.g. `Docs/`) so your project docs stay current. Configure path and “Sync after scan” in **Settings → Live Documentation**.

---

## Asset Inspector

- **Overview** — Issues list, severity, educational tooltips, quick actions (Open node, Decouple with Interface, Nativize).
- **Chat** — AI conversation with Blueprint/project context; intent detection; refactor suggestions.
- **Audit Report** — Static report, Performance Audit, UMG Accessibility Audit.
- **Dependency Graph** — Interactive graph with loop highlighting.
- **Heatmap** — Execution heat (when available).
- **Export** — UML and Markdown to `Saved/BlueprintInsight/`.
- **Compare to previous revision** (source control) — Diff report and “Blueprint Insight” chat bubble (fixed/introduced counts). **Summarize with AI** sends the changelog to your configured LLM (e.g. local Ollama) for a short human-readable summary instead of a long bullet list.
- **Generate Semantic Changelog** — Semantic diff vs previous version.
- **Generate Functional Test** — Creates an `AFunctionalTest` asset.
- **Investigation projects** — Multi-asset projects (max 5), move sessions, AI over project.
- **Create Investigation from Runtime** — From Runtime Debugger: save logs, trace, and involved assets for AI analysis.

---

## Content Browser

- **Analyze with Blueprint Insight** — Opens the panel and Inspector for the selected Blueprint(s).
- **Add to Investigation Project** — Adds the selected Blueprints to an investigation project.

---

## Runtime Debugger

- **Message traffic log** — Sender, payload type, timestamp.
- **Frame trace** — Blueprint node execution, actor location/velocity/movement/collision, watched vars, visual snapshot.
- **Timeline** — Scrub, play/pause, step back/forward.
- **Frame viewer** — Visual snapshot at time.
- **Flame graph** — Performance/call visualization.
- **Create Investigation Project from Logs** — Persist logs, trace, and assets for AI analysis.
- **In-game** — Gameplay Debugger category (e.g. Numpad 5) when using the Gameplay Debugger.

---

## History

- **Export History** — List of exported files (UML, Audit Reports, analysis snapshots); Open Folder, Refresh.

---

## Architecture and CI

- **Architect schema** — Define layer rules; use the Dashboard schema picker and **Validate**.
- **Audit commandlet** — Headless project scan; non-zero exit code when critical issues exist (for CI).
- **Architecture commandlet** — Validate against a schema from the command line (e.g. `-Schema=/Path/To/Schema.Schema`) for CI.

---

## AI and privacy

- **Local (Ollama)** — When using a local endpoint, no Blueprint or project data leaves your machine.
- **Cloud (OpenAI/Gemini)** — If you use cloud providers, data is sent over HTTPS for generating responses only; configure API keys in **Settings**.

---

## In-editor help

Use the **Help** tab inside the Blueprint Insight panel for prompts, placeholders, and FAQs.
