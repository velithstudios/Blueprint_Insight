# Design ideas: Live documentation & LLM-powered diff summary

This document captures product and technical directions for Blueprint Insight. Not all items are implemented; they serve as a roadmap and single source for design decisions.

---

## 1. Live documentation (user-facing: keep project docs current)

**Intent:** A **user-facing feature** so that the **project’s own documentation** is always current. The plugin **reads and writes** project documentation to files (e.g. in the project’s `Docs/` or a configured folder) so that whenever the user runs a scan, architecture check, or similar, the written docs reflect the current state of the project—no separate manual doc updates.

**Problem:** Project docs (architecture overview, Blueprint inventory, dependency maps, etc.) go stale as soon as Blueprints or structure change. Users want one place that stays in sync with the actual project.

**Proposed behavior:**

- **Output location:** User-configurable (e.g. `ProjectRoot/Docs/`, `Content/Documentation/`, or `Saved/BlueprintInsight/Docs/`). Default could be a single folder so the project can commit it to source control.
- **Read/write:**
  - **Write:** When the user runs “Scan Project”, “Validate Architecture”, or an explicit “Sync project docs” action, the plugin **writes** (or updates) documentation files with current data, e.g.:
    - Blueprint list and high-level health/issue counts
    - Dependency overview or circular-loop summary
    - Architecture validation result (pass/fail, violations)
    - Optional: per-Blueprint summaries, UML or dependency snippets (if not too large)
  - **Read:** Optionally **read** existing doc files (e.g. a hand-written `Docs/Overview.md`) and merge or cross-link with generated sections, so the live docs are a combination of generated + human-maintained content.
- **Format:** Markdown (or a single structured file like JSON/MDX) so the docs are human-readable and work with version control and static site generators.
- **Trigger:** Explicit “Sync project docs” (and optionally “sync after each scan” in Settings) so the user controls when files are written.

**Technical fit:**

- Reuse existing scan and analysis: **Project Scanner**, **Dependency Scan**, **Architecture validator** already produce the data. A “Live docs” service would format that data and write to the chosen directory.
- **History** and **Export** already write to `Saved/BlueprintInsight/`; live docs would extend that idea to a user-chosen, project-level docs folder and keep it updated from current state.

**Summary:** Live documentation = **user’s project docs** kept current by the plugin via **read/write to project doc files** (e.g. `Docs/`), driven by scan/architecture and an explicit “Sync project docs” (and optionally auto-sync). **Implementation (done):** Settings → Live Documentation (project docs folder, sync after scan); Dashboard “Sync project docs” button; FBlueprintInsightProjectDocsWriter writes BlueprintInsight-Overview.md (summary, issues, loops, high-risk assets) and, when "Include project structure in docs" is on, BlueprintInsight-ProjectStructure.md (each Blueprint's functions and variables—same data as the LLM index).

---

## 2. Source control diff + local LLM for readable reports

**Problem:** Diffs and semantic changelogs are accurate but noisy—long lists of “variable X renamed”, “pin Y default changed”, which obscure real behavioral changes.

**Current behavior:**  
“Compare to previous revision” uses source control to load the previous Blueprint, runs Unreal’s diff and the **semantic diff** (node add/remove, connections, default values, comments). The result is shown as a **Logic Historian** changelog (bullet list) plus an **Blueprint Insight** issue summary (fixed/introduced). No AI is involved yet.

**Proposed addition: “Summarize with AI”**

- After showing the Logic Historian changelog and issue summary, offer a button: **“Summarize with AI”**.
- When clicked, send the **semantic changelog text** (and optionally the **delta context JSON**) to the **existing Chat/LLM pipeline** (local Ollama or cloud), with a fixed prompt asking for:
  - A short (2–4 sentence) **human-readable summary** of what changed.
  - Focus on **behavior and intent** (e.g. “Health is now clamped after damage”, “New branch for low ammo”).
  - **De-emphasize** trivial renames, comment-only edits, and layout.
- The LLM response is shown as a new chat bubble (e.g. “Summary”) so users get a concise report instead of a wall of bullets.

**Technical fit:**

- **ChatService** already has `SendRequest(RawPrompt, Callback)`. Add a thin wrapper `RequestDiffSummary(ChangelogText, Callback)` that builds the prompt and calls `SendRequest`. No new endpoints; works with local (Ollama) and cloud providers.
- **Inspector** already has the changelog in scope when it adds the “Logic Historian” and “Blueprint Insight” bubbles. Add a third slot with a “Summarize with AI” button that calls `RequestDiffSummary(Changelog, …)` and appends the result as a bubble.
- **Privacy:** If the user uses **local Ollama**, the diff never leaves the machine. If they use cloud, the same policy as Chat applies (user-configured; data sent only for that request).

**Implementation (done):**

- **ChatService::RequestDiffSummary(ChangelogText, OptionalIssueSummary, Callback)** builds a prompt and calls the existing LLM pipeline (local Ollama or cloud). The prompt asks for 2–4 sentences focused on behavior/logic and de-emphasizes renames and layout.
- After “Compare to previous revision”, the Inspector shows the **Logic Historian** changelog and **Blueprint Insight** issue summary, then a **“Summarize with AI”** button. Clicking it sends the changelog (and issue summary) to the configured AI and appends a **Summary** chat bubble with the response. Works with local LLM so diff text never leaves the machine when using Ollama.

**Optional extensions:**

- **Delta JSON:** For larger changes, include a truncated `GenerateDeltaContextAsJson` in the prompt; keep prompt size bounded.
- **Default to local:** In Settings, “Use local LLM for diff summaries when available” could auto-run the summarizer when Ollama is configured.

---

## 3. Runtime Debugger: OmniDebug-inspired roadmap

**Intent:** Use the **Project OmniDebug** architectural proposal (next-generation runtime debugging for UE5) as a strategic reference for the Runtime Debugger. **Roadmap:** [OMNIDEBUG_RUNTIME_ROADMAP.md](OMNIDEBUG_RUNTIME_ROADMAP.md) maps OmniDebug's 20 features to Blueprint Insight and proposes phased adoption (tag/event stream, variable watcher, tick budget, step-by-tick-group, optional overlays). Full rewind, Iris/network tools, and 3D holographic graph are out of scope.

---

## 4. Summary

| Idea | Status | Notes |
|------|--------|--------|
| **Live documentation** (user project docs) | Done | Project docs folder in Settings; “Sync project docs” button and optional sync after scan; writes Overview + ProjectStructure (functions, variables) tied to LLM index. |
| Diff: Semantic changelog + issue summary | Done | Compare to previous revision. |
| Diff: “Summarize with AI” (LLM) | Done | RequestDiffSummary + “Summarize with AI” button; uses local or cloud LLM. |
