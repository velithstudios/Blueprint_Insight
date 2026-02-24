# Runtime Debugger Roadmap: OmniDebug-Inspired Directions

This document maps the **Project OmniDebug** architectural proposal (comprehensive next-generation runtime debugging for Unreal Engine 5) onto **Blueprint Insight**'s existing Runtime Debugger and proposes a practical, phased adoption of selected concepts. OmniDebug is used as a **strategic reference**, not as a literal implementation spec—Blueprint Insight remains an analysis- and Blueprint-focused plugin at a different scope and price point.

---

## 1. Source and Scope

- **OmniDebug (proposal):** 20 features across Reactive Data Bus, SlateIM overlays, temporal rewind, spatial/visual layers, logic/state inspection, Iris/network observability, and UX (command palette, multi-monitor). Assumes a dedicated "OmniDebug" plugin with heavy engine integration.
- **Blueprint Insight (current):** Runtime Debugger tab with message traffic log, frame trace (node + actor forensics), timeline scrub, frame viewer, flame graph, search/filter/bookmarks/time range, copy/save log, investigation from range/bookmarks, and Gameplay Debugger category in PIE.
- **This roadmap:** Identifies which OmniDebug ideas **align with Blueprint Insight's product** (Blueprint-centric, analysis-first, useful for solo/small teams) and which are **feasible** without becoming a second full-time project.

---

## 2. Mapping: OmniDebug Domains → Blueprint Insight

| OmniDebug domain | Blueprint Insight today | Gap / opportunity |
|------------------|-------------------------|--------------------|
| **Reactive Data Bus / persisted history** | Message traffic logger (ring-style), circular trace buffer for frames | We already have "event stream + ring buffer"; could formalize as a single **debug event bus** and add more event types (see below). |
| **Temporal: rewind, branch, step-by-subsystem** | Timeline scrub, play/pause, step fwd/back, bookmarks, time range | No full state rewind or deterministic replay; **Chronos-style rewind** is out of scope. **Delta-step (tick group)** inspection could be a future "step by tick group" in the timeline. |
| **Spatial: invisible renderer, audio, physics islands** | Frame viewer (dashcam), collision callouts in trace | No unified "debug layers" or Chaos/Iris-specific views. **Physics sleep/active** or **simple collision overlay** could be added as optional viewport overlays if we add a minimal debug draw pass. |
| **Logic: 3D Blueprint graph, variable watcher** | Frame trace includes NodeName, NodeGuid, WatchedVariables; Inspector has static graph | No **in-viewport** 3D graph or **world-space variable labels**. **Reactive Variable Watcher** (world labels + pulse on change) fits our "watched vars" and could extend the existing trace. |
| **AI: Blackboard, EQS, Behavior Tree HUD** | — | Not in scope today; would require AI module dependency and significant UI. **Event Bus Logger / Tag Stream** (GAS/tag feed) is closer: we could add a **Gameplay Tag stream** to the message log (if GAS/tags are present). |
| **Network: Iris, bandwidth, ghost trails** | — | Full Iris/bandwidth tooling is **out of scope**. **Phase 4 (future):** lightweight **replication observability** in the Runtime Debugger—replication events in the event stream, who/what is replicated for tracked actor(s), optional RPC logging—without building a full replication graph. |
| **Performance: tick budget** | Flame graph (execution timing) | **Tick budget per actor** (cost thermometer, budget alerts) aligns with our flame graph and could be a **Phase 2** addition: show tick cost per Blueprint/actor in the runtime tab. |
| **UX: command palette, multi-monitor** | Single panel, tabbed views | **Command palette** (Ctrl+Shift+P) for "focus Runtime Debugger", "clear log", "add bookmark" could be a small UX win. **Multi-monitor** (undock panels) is an editor feature; we could support it if Slate allows. |

---

## 3. Phased Roadmap (OmniDebug-Inspired)

### Phase 1 (Current release + near-term)

- **Already done:** Log search, "Show only issues," time range, bookmarks, copy/save log, investigation from range/bookmarks. These give a **seamless UX for catching bugs** without implementing OmniDebug's full temporal or spatial stack.
- **Optional next:**
  - **Tag / event stream:** If the project uses Gameplay Tags or a simple event name string, add a **filter or secondary log** for "tag-like" events (e.g. `+Status.Stunned`) so the message log can double as a lightweight **Event Bus Logger** (OmniDebug Feature 12).
  - **Reactive variable watcher (simplified):** Expose **WatchedVariables** in the frame viewer or a small "current frame state" panel with **pulse on change** (e.g. highlight when a value changes from the previous frame). This is OmniDebug Feature 14 in minimal form.

### Phase 2 (Medium-term)

- **Tick budget / cost per actor:** Use existing trace or a lightweight sampler to show **tick cost per Blueprint/actor** in the Runtime Debugger (list or bar chart). Optional "budget" threshold and highlight when exceeded (OmniDebug Feature 18).
- **Step by tick group:** If we can hook **tick group boundaries**, add "Step to next tick group" (PrePhysics → DuringPhysics → PostPhysics) to the timeline controls to help with **order-of-operations** bugs (OmniDebug Feature 3, reduced scope).
- **Debug event bus (formalize):** Treat message traffic + frame trace as a single **event stream** with a small set of event types (Message, Frame, Collision, Custom). Enables future filters and "show only physics events" etc., without building a full RDB.

### Phase 3 (Longer-term / exploratory)

- **Viewport overlays:** Optional **collision** or **physics state** (sleep/active) overlay using existing debug draw or a minimal SlateIM-style pass, if we can keep performance and complexity under control (OmniDebug Features 5, 7 in very reduced form).
- **Command palette:** Editor shortcut to "Open Blueprint Insight → Runtime Debugger" and perhaps "Clear log" / "Add bookmark" (OmniDebug Feature 19, subset).

### Phase 4 (Future): Network replication observability

*Replication visibility in the Runtime Debugger*, defined as **replication observability** (not full Iris/bandwidth tooling):

- **Replication events in the event stream:** Treat replication as another event type (e.g. "Var replicated", "RPC sent/received"). Show these in the message log/timeline with a filter (e.g. "Event type: Replication") so you can correlate with Blueprint execution and state.
- **Who / what is replicated:** For the **currently tracked actor** (and optionally a small set of actors): show which properties/components are replicated, and a simple "last replicated at" or "replicated this frame" indicator in the existing frame/watched-vars UI. No full replication graph—just "this actor, these vars, replication yes/no + timing."
- **RPC / reliability:** If we can hook at a high level: log "RPC X called (server→client or client→server)" with timestamp into the same log/timeline as one more event type in the debug event bus, not a full RPC profiler.

*Explicitly out of scope for this phase:* Full Iris Replication Graph Explorer, bandwidth heatmaps, and deep engine replication internals—better as a dedicated or Epic-provided tool.

### Explicitly out of scope (for this plugin)

- **Chronos State Rewind / deterministic replay** (OmniDebug 1, 2): Requires keyframe + input replay and possibly Pocket Worlds; too large for current scope.
- **Butterfly Effect diff / ghost simulation** (OmniDebug 2): Same.
- **Full Iris Replication Graph Explorer / bandwidth heatmap** (OmniDebug 15, 16): Deep networking; better as a dedicated or Epic-provided tool.
- **3D holographic Blueprint graph** (OmniDebug 10): High effort; our strength is the existing Inspector + static graph + runtime **data** (trace, log), not in-viewport graph rendering.
- **Audio propagation, EQS, full GAS attribute graph** (OmniDebug 6, 11, 13): Niche and module-heavy; document as "future or separate" if we get user demand.

---

## 4. Comparative Summary

| Feature domain | Traditional UE5 | OmniDebug proposal | Blueprint Insight (current + roadmap) |
|----------------|-----------------|--------------------|--------------------------------------|
| **Logic / state** | PrintString, breakpoints | 3D graph, variable watcher | Frame trace (node + vars), timeline; **roadmap:** variable watcher pulse, optional tag stream |
| **Time control** | Pause, slomo | State rewind, branch diff | Timeline scrub, bookmarks, time range; **roadmap:** step by tick group (no rewind) |
| **Log / events** | Output Log, console | Event bus, tag stream | Message log + search/issues/range/bookmarks, copy/save; **roadmap:** tag-like stream, formal event bus |
| **Performance** | stat game, profiler | Tick budget profiler | Flame graph; **roadmap:** tick cost per actor, budget alert |
| **Network** | NetTrace (post-mortem) | Iris explorer, bandwidth heatmap | — (out of scope); **roadmap Phase 4:** replication observability in Runtime Debugger |
| **Physics** | Console (e.g. collision) | Island Isigator, jitter | Collision in trace; **roadmap:** optional overlay (Phase 3) |
| **UX** | Console, separate windows | Command palette, multi-monitor | Single panel; **roadmap:** optional palette shortcut |

---

## 5. Conclusion

The OmniDebug research provides a **clear target for "ideal" runtime observability** in UE5. Blueprint Insight does not aim to implement OmniDebug in full; it aims to:

- **Strengthen the Runtime Debugger** with OmniDebug-**inspired** features that fit our scope: better logs (done), variable/tag visibility, tick cost, and optional viewport/step controls.
- **Keep the plugin focused** on Blueprint analysis, dependencies, and AI-assisted workflow, with the runtime tab as the **place to catch bugs and create investigations**, not as a full engine observability platform.

This roadmap should be updated as we ship Phase 1 options and reassess Phase 2/3 based on user feedback and engine APIs (e.g. SlateIM, tick group hooks, Iris exposure).

---

*Reference: "Project OmniDebug: A Comprehensive Architectural Proposal for Next-Generation Runtime Debugging in Unreal Engine 5" (internal research).*
