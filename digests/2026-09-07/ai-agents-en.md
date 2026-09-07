# OpenClaw Ecosystem Digest 2026-09-07

> Issues: 144 | PRs: 500 | Projects covered: 5 | Generated: 2026-09-07 04:41 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-09-07

## 1. Today's Overview

OpenClaw saw very high activity on 2026-09-07: 500 PRs were updated with **225 merged/closed**, while 144 issues were touched (**89 closed**, 55 open/active). No new release shipped today; issue reports indicate current stable is **2026.9.2**, with a **2026.9.3 candidate** referenced in release-validation CI and unblocked by PR [#140794](https://github.com/openclaw/openclaw/pull/140794). A large share of issue closures came from stale-lifecycle automation (labels like `stale`, `clawsweeper:no-new-fix-pr`, `clawsweeper:needs-product-decision`), signaling active backlog hygiene but also a growing pile of product decisions deferred. Development effort concentrated on agent/session lifecycle correctness (subagent dispatch, reply authority, sequential tool enforcement), channel fixes (Discord, WhatsApp/iMessage/Signal approvals), auth/catalog hardening (xAI/Grok OAuth), and client polish (Android, macOS, iOS).

## 2. Releases

None. No new releases published on 2026-09-07. The next expected version is **2026.9.3**, currently in full release validation; PR [#140794](https://github.com/openclaw/openclaw/pull/140794) (fix release scanner accepting reviewed cleanup fixtures) was submitted to unblock that candidate.

## 3. Project Progress

High merge/close throughput today (225 PRs merged/closed). From the top-30 PR list, notable closed items include:

- **Worktree diagnostics fixed**: PR [#140251](https://github.com/openclaw/openclaw/pull/140251) preserves the precise "no commits" error instead of the generic "not a git checkout" message (closes issue [#140204](https://github.com/openclaw/openclaw/issues/140204)).
- **CLI**: PR [#140789](https://github.com/openclaw/openclaw/pull/140789) clarifies `openclaw telemetry show` as a local preview.
- **CI**: PR [#140751](https://github.com/openclaw/openclaw/pull/140751) avoids duplicate Watch builds in iOS screenshot CI.
- **macOS app**: PR [#140756](https://github.com/openclaw/openclaw/pull/140756) rebuilds the hand-styled Connection window as a standard Settings window.

Major fixes/features still open and awaiting merge/review:

- **Subagent lifecycle**: PR [#140137](https://github.com/openclaw/openclaw/pull/140137) dispatches nested requester yield-batch wakes on settlement (closes #139963); PR [#140158](https://github.com/openclaw/openclaw/pull/140158) fixes reply-authority fallback when a captured reply operation is retired (closes #139847).
- **Code Mode**: PR [#140767](https://github.com/openclaw/openclaw/pull/140767) honors sequential-only tools when Code Mode runs parallel calls.
- **Channels**: PR [#140645](https://github.com/openclaw/openclaw/pull/140645) shares approval-reaction settlement logic across WhatsApp/iMessage/Signal (closes #131004); PR [#140798](https://github.com/openclaw/openclaw/pull/140798) adds explicitly trusted Discord administrators; PR [#140653](https://github.com/openclaw/openclaw/pull/140653) keeps Grok token discovery on the subscription route.
- **Sessions**: PR [#140748](https://github.com/openclaw/openclaw/pull/140748) restricts pins to root sessions; PR [#140793](https://github.com/openclaw/openclaw/pull/140793) splits deleted vs. reset archive retention; PR [#140733](https://github.com/openclaw/openclaw/pull/140733) avoids loading the full agent runtime for session storage.
- **Clients**: PR [#140796](https://github.com/openclaw/openclaw/pull/140796) and [#140759](https://github.com/openclaw/openclaw/pull/140759) fix Android model-settings staleness and reading-position loss during streaming.
- **New capability**: PR [#139850](https://github.com/openclaw/openclaw/pull/139850) adds a Team Reports plugin for GitHub/Discord activity reports; PR [#119773](https://github.com/openclaw/openclaw/pull/119773) adds shared structured image extraction for vision-capable providers.
- **Infra/maintenance**: PR [#140278](https://github.com/openclaw/openclaw/pull/140278) bumps managed llama.cpp to b10809; PR [#140781](https://github.com/openclaw/openclaw/pull/140781) applies Anthropic cache markers on the OpenAI-completions transport; PR [#138405](https://github.com/openclaw/openclaw/pull/138405) makes the memory-core dream-diary timeout configurable.

## 4. Community Hot Topics

- **[#79077 — Telegram bot-to-bot & guest-bot support](https://github.com/openclaw/openclaw/issues/79077)** — 14 comments, **8 👍 (highest-reacted item)**. Users want parity with Telegram's May-2026 platform release (guest bots, bot-to-bot communication). Closed as `stale` pending product decision; this is the clearest roadmap-demand signal in the data.
- **[#97616 — Zombie child-process leak](https://github.com/openclaw/openclaw/issues/97616)** — 14 comments, P1 regression. Hook/tool child processes (bash, codex, hooks) are never reaped, accumulating zombies under the main gateway and degrading runtime. Awaiting more info (`clawsweeper:needs-info`); no fix PR yet.
- **[#140010 — Windows sleep/resume reconnect failures](https://github.com/openclaw/openclaw/issues/140010)** — 9 comments, P1. After wake, UI/WebSocket reconnects stall 30–60s+ behind a busy gateway and event-loop stalls. No fix PR yet.
- **[#140535 — Discord `/new` returns "No reply was generated"](https://github.com/openclaw/openclaw/issues/140535)** — 7 comments, P1. Stock `/new` is acknowledged but doesn't reset the channel session. Closed.
- **[#78963 — WhatsApp listen-only/hooks-only mode](https://github.com/openclaw/openclaw/issues/78963)** — 7 comments, 1 👍. Repeated ask for inbound-only message ingestion (archival/ETL) without agent runs or LLM calls; closed stale pending product decision.
- **[#140497 — Discord setup accepts application ID as bot token](https://github.com/openclaw/openclaw/issues/140497)** — 5 comments on a **P0, open** issue; setup reports "configured/stopped" with `lastError: null`, appearing hung rather than failed.
- **[#140129 — Anthropic cache stuck at ~46k prefix on long sessions](https://github.com/openclaw/openclaw/issues/140129)** — 5 comments; every turn rewrites the full history (`cache_create` 377k–386k), implying significant cost/latency waste.

Underlying needs: reliable long-running process/session semantics, platform feature parity, and fail-explicitly onboarding UX.

## 5. Bugs & Stability

Ranked by severity (P0 first). Fix-PR status noted where one exists.

**P0**
- [Open] [#140497](https://github.com/openclaw/openclaw/issues/140497): Discord setup persists the application ID as the bot token; channel shows enabled/configured/stopped with no error. Onboarding traps users in a silent hang. Needs live repro.
- [Closed] [#140482](https://github.com/openclaw/openclaw/issues/140482): xAI OAuth login overwrites the working Grok OAuth catalog with the xAI API catalog (`baseUrl` drift). Closed as fix-shape-clear/queueable-fix.
- [Closed] [#140393](https://github.com/openclaw/openclaw/issues/140393): 2026.9.2 onboarding installs Codex but first dashboard chat fails with a missing prepared runtime.
- [Closed] [#106920](https://github.com/openclaw/openclaw/issues/106920): `openclaw update` left the gateway unable to restart in 2026.7.1 (regression; 5 👍).
- [Closed] [#96203](https://github.com/openclaw/openclaw/issues/96203): Gateway crash-loops ~every 10–12s under default Node heap (~4GB) on large workspaces.

**P1**
- [Open] [#97616](https://github.com/openclaw/openclaw/issues/97616): unreaped hook/tool child processes → zombie accumulation and runtime degradation (regression). No fix PR.
- [Open] [#140010](https://github.com/openclaw/openclaw/issues/140010): Windows sleep/resume leaves gateway unreachable from UI/WebChat for 30–60s+. No fix PR.
- [Open] [#118018](https://github.com/openclaw/openclaw/issues/118018): stale subagent completion can be delivered into a replaced requester lifecycle and settled without error. Related fix PR [#140137](https://github.com/openclaw/openclaw/pull/140137) is open.
- [Open] [#121232](https://github.com/openclaw/openclaw/issues/121232): memory-core dreaming reports "Ranked N, Promoted 0 forever" — ranker and applier structurally disagree, with nothing surfacing the conflict.
- [Closed w/ fix] [#140466](https://github.com/openclaw/openclaw/issues/140466): xAI `auto` alias normalizes to canonical model, then fails runtime auth rematerialization.
- [Closed w/ fix] [#137690](https://github.com/openclaw/openclaw/issues/137690): `sessions_spawn` fails with "unknown parent session" for Telegram-originated sessions on 2026.8.2.
- [Closed] [#140535](https://github.com/openclaw/openclaw/issues/140535): Discord `/new` doesn't reset session and returns a misleading fallback error.

**P2 notable**
- [Closed w/ fix] [#140214](https://github.com/openclaw/openclaw/issues/140214): `memory.search.extraPaths` silently omits a configured symlink root (Obsidian vault missing after QMD migration).
- [Closed w/ fix] [#140416](https://github.com/openclaw/openclaw/issues/140416): bare `--import tsx` breaks worker spawns outside a package-root cwd.
- [Open] [#140129](https://github.com/openclaw/openclaw/issues/140129): Anthropic prompt cache stuck at ~46k tools+system prefix; whole conversation rewritten every turn.
- [Open] [#102078](https://github.com/openclaw/openclaw/issues/102078): compaction fails on local MLX with "Thread group size (1024) exceeds maximum (896)", blocking sessions.

Overall, today's bug inflow skews toward **session-state lifecycle correctness**, **auth/catalog state drift**, and **silent misconfiguration**, with several fix-shape-clear items already closed — suggesting a fast triage pipeline for reproducible bugs.

## 6. Feature Requests & Roadmap Signals

Highest-demand user requests:

- **Telegram guest bots + bot-to-bot** ([#79077](https://github.com/openclaw/openclaw/issues/79077)): 8 👍, closed stale — needs a product decision. Given platform-parity history, this is a plausible near-term roadmap item despite the stale closure.
- **WhatsApp listen-only/hooks-only mode** ([#78963](https://github.com/openclaw/openclaw/issues/78963)): repeated ETL/archival use case; closed stale awaiting product decision.
- **Local vs. cloud provenance in the Control UI model picker** ([#122403](https://github.com/openclaw/openclaw/issues/122403)): small UX change derived from data OpenClaw already has — likely candidate for an upcoming release.
- **Native approval buttons for Feishu/Microsoft Teams/Mattermost** ([#104521](https://github.com/openclaw/openclaw/issues/104521)): restores clickable `/approve` UX after typed-approval refactor; note related shared settlement refactor PR [#140645](https://github.com/openclaw/openclaw/pull/140645) is already open for other channels.
- **Manual context clearing for tool results** ([#45503](https://github.com/openclaw/openclaw/issues/45503)): P3, stale since March; efficiency ask for large transient tool outputs.
- **Chinese input typo/grammar detection** ([#82011](https://github.com/openclaw/openclaw/issues/82011)): P3, open, no maintainer action.
- **MCP `notifications/tools/list_changed` + HTTP reload endpoint** ([#91556](https://github.com/openclaw/openclaw/issues/91556)): driven by a scale-operations customer (Composio MCP at scale).

Signals from PRs suggest the nearer-term direction favors: **approval UX normalization across channels** ([#140645](https://github.com/openclaw/openclaw/pull/140645)), **admin/trust controls for Discord** ([#140798](https://github.com/openclaw/openclaw/pull/140798)), **session data-retention controls** ([#140793](https://github.com/openclaw/openclaw/pull/140793)), and **provider-behavior fixes** (xAI/Grok subscription routes, [#140653](https://github.com/openclaw/openclaw/pull/140653)).

## 7. User Feedback Summary

- **Cost sensitivity on long sessions**: The Anthropic cache issue ([#140129](https://github.com/openclaw/openclaw/issues/140129)) describes a fixed ~46k cache prefix and 377k–386k `cache_create` rewrites every turn on long sessions — users are effectively paying to re-upload full history repeatedly. This is a strong cost/latency pain point.
- **Silent failure modes erode trust**: Discord setup hanging instead of failing ([#140497](https://github.com/openclaw/openclaw/issues/140497)), memory-core dreaming "0 promoted forever" with no surfaced disagreement ([#121232](https://github.com/openclaw/openclaw/issues/121232)), and zombie-process accumulation ([#97616](https://github.com/openclaw/openclaw/issues/97616)) all share a theme: failures that don't fail loudly.
- **Release-cadence frustration**: [#102311](https://github.com/openclaw/openclaw/issues/102311) documents a fix merged to `main` (Telegram outbound filename UUID suffix, #96538) that never shipped in the stable cut, with two reopen requests unanswered. Users are noticing the gap between `main` and release builds.
- **Platform-parity and admin UX demand**: Telegram features drew 8 👍 ([#79077](https://github.com/openclaw/openclaw/issues/79077)); operators want Discord as a full control surface ([#140798](https://github.com/openclaw/openclaw/pull/140798) is the PR response); self-hosting users want local-vs-cloud clarity in the model picker ([#122403](https://github.com/openclaw/openclaw/issues/122403)).
- **Observability gaps**: vLLM usage/cost pages show no data ([#87110](https://github.com/openclaw/openclaw/issues/87110)); `/context detail` can't account for ~62k untracked tokens on fresh sessions ([#86819](https://github.com/openclaw/openclaw/issues/86819)). Users want predictable, explainable token/cost accounting.

## 8. Backlog Watch

Items that are old, important, and still awaiting maintainer action or a product decision:

- **[#45503](https://github.com/openclaw/openclaw/issues/45503)** (2026-03-13, P3): manual context clearing for tool results — unanswered for ~6 months; needs product decision.
- **[#68264](https://github.com/openclaw/openclaw/issues/68264)** (2026-04-17, P2 regression): Canvas/Browser UI visualization fails to render in chat — open ~5 months.
- **[#77378](https://github.com/openclaw/openclaw/issues/77378)** (2026-05-04, P2): session rotation creates duplicate sessions with broken delivery context (Mattermost) — open; needs info.
- **[#97616](https://github.com/openclaw/openclaw/issues/97616)** (2026-06-29, P1): zombie child-process leak — no fix PR; awaiting info.
- **[#118018](https://github.com/openclaw/openclaw/issues/118018)** (2026-08-02, P1): stale subagent completion delivered into a replaced requester lifecycle — linked fix PR open ([#140137](https://github.com/openclaw/openclaw/pull/140137)).
- **[#121232](https://github.com/openclaw/openclaw/issues/121232)** (2026-08-09, P1): memory-core dreaming ranker/applier disagreement — no new fix PR.
- **[#118482](https://github.com/openclaw/openclaw/issues/118482)** (2026-08-03, P2): codex-supervisor WebSocket handshake fails over unix socket (permessage-deflate negotiation).
- **[#122403](https://github.com/openclaw/openclaw/issues/122403)** (2026-08-12, P2): local-vs-cloud model provenance in UI picker — needs product decision.
- **[#104521](https://github.com/openclaw/openclaw/issues/104521)** (stale, P2): native approval buttons for Feishu/Teams/Mattermost — closed-stale risk; still relevant to channel parity.

Long-running PRs that would benefit from maintainer attention: [#117504](https://github.com/openclaw/openclaw/pull/117504) (Bedrock custom embedding endpoints, since Aug 1), [#117605](https://github.com/openclaw/openclaw/pull/117605) (fail-closed task cancellation, since Aug 1), [#119773](https://github.com/openclaw/openclaw/pull/119773) (media-understanding extraction, since Aug 5), [#127240](https://github.com/openclaw/openclaw/pull/127240) (CI broker for noncanonical runs; security-scan related, since Aug 21), and [#130877](https://github.com/openclaw/openclaw/pull/130877) (SQLite trajectory export OOM bound, since Aug 27).

Finally, several high-reaction feature issues were closed today purely by stale automation ([#79077](https://github.com/openclaw/openclaw/issues/79077), [#78963](https://github.com/openclaw/openclaw/issues/78963), [#86986](https://github.com/openclaw/openclaw/issues/86986), [#86946](https://github.com/openclaw/openclaw/issues/86946), [#96203](https://github.com/openclaw/openclaw/issues/96203)). If those represent real roadmap demand, maintainers should convert them into explicit product decisions rather than letting them lapse silently.

---

## Cross-Ecosystem Comparison

# Cross-Project Ecosystem Comparison — 2026-09-07

---

## 1. Ecosystem Overview

The open-source personal AI assistant/agent landscape is converging on a shared architecture: a long-running gateway/daemon owns session state, memory, and tool execution; channel adapters (Discord, Telegram, WhatsApp, email, etc.) serve as control surfaces; and desktop/mobile/Web clients are increasingly optional front-ends over a headless runtime. Across the five projects digested today — OpenClaw, Hermes Agent, IronClaw, QwenPaw, and ZeroClaw — roughly 639 PRs were touched and ~244 merged/closed, while ~192 issues saw activity with ~96 closures; **no project shipped a release**, signaling a stabilization period rather than a feature-launch cycle. The dominant engineering themes are no longer new bells and whistles but *session-lifecycle correctness, crash-safe turn persistence, daemon reliability, provider/cost control, and channel UX parity* — the operational concerns of agents being run as always-on services. A secondary but clear signal is that Anthropic prompt-cache tuning, context-window fallbacks, and token/cost accounting have become first-class platform problems, not provider-side details.

---

## 2. Activity Comparison

Figures as reported in each project digest; some counts include automated lifecycle/CI activity (e.g., OpenClaw's `clawsweeper` bot).

| Project | Issues Updated (closed) | PRs Updated (merged/closed) | Release Status | Health Score* |
|---|---|---|---|---|
| **OpenClaw** | 144 (89 closed / 55 open) | 500 (225 merged/closed) | None today; stable **2026.9.2**, **2026.9.3** in release validation | **4.0 / 5** |
| **Hermes Agent** | 9 (0 closed) | 50 (5 merged/closed) | None | **4.0 / 5** |
| **IronClaw** | 0 (0 closed) | 13 (3 merged/closed, all dep/CI) | None | **3.5 / 5** |
| **QwenPaw** | 25 (≈7 closed) | 26 (6 merged/closed) | None; v2.2.0-beta.7 release-duty item closed | **3.0 / 5** |
| **ZeroClaw** | 14 (0 closed) | 50 (5 merged/closed) | None | **2.5 / 5** |

*\*Health score is an analyst composite (1–5) of 24-hour throughput, issue-closure ratio, fix-PR availability for top-severity bugs, and open-severity load.*

- **OpenClaw** — exceptional triage throughput (225 PRs and 89 issues closed in 24h), but carries an open P0 (Discord setup accepts application ID as bot token) and multiple open P1s without fix PRs; large stale-automation closures are deferring product decisions.
- **Hermes Agent** — small issue volume and rapid issue→fix-PR pairing on the same day; penalized by an unresolved 170-comment skills-index infrastructure issue and overlapping open PRs (#76774/#77359) needing consolidation.
- **IronClaw** — clean bill of health: zero issue inflow, no crash-level reports, consistent dependency hygiene; only housekeeping merges landed, so momentum is low.
- **QwenPaw** — active beta stabilization with healthy first-time-contributor participation (contributors filed bugs *and* submitted fix PRs), but several high-severity issues (#7579, #7589, #7363, #7576) lack fixes and threaten v2.2.0 GA.
- **ZeroClaw** — healthiest contributor-maintainer collaboration loop, but zero issue closures in 24h, an open **S0 data-loss bug** (#10121), and a PR queue growing faster than merge throughput.

---

## 3. OpenClaw's Position

**Advantages over peers**

- **Scale advantage by an order of magnitude.** OpenClaw touched 500 PRs and merged/closed 225 in a single day — roughly 10× the PR-touch volume and 37–75× the merge volume of its nearest peers. It closed more issues in 24h (89) than the other four projects *touched* combined (48).
- **Most mature release process.** It is the only project operating a named stable release train (2026.9.2) with a candidate (2026.9.3) moving through release-validation CI — peers are pre-release or release-quiet.
- **Widest platform surface.** OpenClaw is the only project spanning Discord, Telegram, WhatsApp, iMessage/Signal, Feishu/Teams/Mattermost, a Canvas/Browser UI, *and* dedicated Android/iOS/macOS clients. Its managed-runtime approach (llama.cpp bump #140278, Codex onboarding, local MLX support) makes it the most "platform-like" project in the cohort.
- **Process maturity.** Automated lifecycle tooling (`clawsweeper`, stale labels) and a fast triage pipeline for reproducible bugs exist at a scale none of the peers have reached.

**Technical approach differences**

- OpenClaw models agent activity as a **session tree with root-session pinning** and explicit subagent dispatch semantics — yield-batch wake-ups (#140137), reply-authority fallback (#140158), and sequential-only tool enforcement in Code Mode (#140767). Peers are still converging on simpler "one session = one conversation" models.
- It operates a distinct **memory subsystem** (memory-core with dream cycles, ranker/applier behavior) and a **plugin layer** (Team Reports, #139850) — components the other digests do not show at comparable depth.
- Its channel approval logic is being normalized *across* channels (#140645 WhatsApp/iMessage/Signal), indicating an abstraction layer peers have not yet built.

**Community size comparison**

- OpenClaw's contributor/process traffic is clearly the largest. However, per-thread end-user engagement is not proportionally higher — Hermes' group-chat issue (#97681) drew 25 comments, versus 14 for OpenClaw's most-discussed item. OpenClaw's dominance likely reflects a much larger *contributor base plus heavier automation*, not necessarily a proportionally larger user community.
- Its highest-reaction issue reached only 8 👍 (#79077, Telegram guest bots), suggesting room for deeper end-user engagement relative to PR volume.

**Watch-outs**

- Stale automation closed several high-demand feature requests (Telegram bot-to-bot #79077, WhatsApp listen-only #78963) with no product decision — a community-trust risk at its scale.
- OpenClaw shares the cohort's core stability gaps (zombie child processes #97616, Windows sleep/resume #140010, silent Discord misconfiguration P0 #140497) despite its throughput advantage.

---

## 4. Shared Technical Focus Areas

Multiple projects are independently converging on the same problem set — strong evidence of where the ecosystem's real requirements are.

### 4.1 Durable, ordered, race-free session execution (all five; most acute in ZeroClaw, QwenPaw, OpenClaw)
- **ZeroClaw:** Partial Code/ACP turns vanish on process exit (**S0, #10121**); failed turns disappear after session switch (#9333); budget-exceeded turns discard streamed progress (#10659); a second message can start a duplicate parallel run (#10408). Fixes in flight: checkpoint/recovery PR #10197, transcript persistence #9378.
- **QwenPaw:** Model replies disappear from context after persistence (#7579); duplicate-message pileup and ~2h unresponsiveness in heartbeat cron (#7589).
- **OpenClaw:** Stale subagent completion can be settled into a replaced requester lifecycle (#118018); reply-authority fallback on retired operations (#139847).
- **Hermes Agent:** ACP clients mint permanent empty probe sessions (#104724); resident session leases must be reclaimed from dead Desktop lanes (#104737).

**Shared need:** crash-safe, WAL-like turn transcripts; exactly-once dispatch; one agent run per session enforced server-side.

### 4.2 Always-on, daemon-first reliability (OpenClaw, ZeroClaw, QwenPaw, Hermes)
- **OpenClaw:** zombie child-process accumulation (#97616); Windows sleep/resume reconnect stalls (#140010).
- **ZeroClaw:** daemon startup/reload stack overflow (#10230); RPC connections not closed on reload (#10262).
- **QwenPaw:** synchronous calls freeze the Windows event loop for 118–135s (#7363); LAN LLM servers repeatedly disconnect (#7505).
- **Hermes:** users explicitly want group chats to keep running after Desktop closes (#97681) and propose gateway-side round drivers (#95163).

**Shared need:** separate client UI from authoritative backend sessions; treat reconnect/resume as core SLA, not an edge case.

### 4.3 Provider cost, cache, and configuration integrity (OpenClaw, ZeroClaw, QwenPaw)
- **Anthropic cache economics:** OpenClaw reports a stuck ~46k prefix with 377k–386k `cache_create` rewrites every turn (#140129); ZeroClaw is adding a third cache breakpoint (#10660/#10666), fixing a sub-minimum OAuth marker (#10662), and requesting configurable cache TTL (#10663).
- **Context-window correctness:** QwenPaw shipped a hardcoded 32768-token fallback affecting *all* v2.1–v2.2 releases (#7576); DeepSeek compression breaks via `role=user` (#6541).
- **Provider-state drift:** OpenClaw xAI OAuth overwrites the Grok catalog (#140482, closed); ZeroClaw wants live provider identity on usage events (PR #8966, still open).

**Shared need:** provider abstractions must handle catalog state, cache markers, TTL, and context sizes explicitly — never silently.

### 4.4 Chat channels as agent control surfaces (OpenClaw, QwenPaw, ZeroClaw, Hermes, IronClaw)
- **Approvals & admin trust:** OpenClaw is normalizing approval-reaction settlement across WhatsApp/iMessage/Signal (#140645) and adding trusted Discord administrators (#140798); users still want native approval buttons for Feishu/Teams/Mattermost (#104521). ZeroClaw has a blocked supervised-shell approval-routing PR (#10241).
- **Output quality in IM:** QwenPaw is fixing raw Markdown tables on Telegram (#7590), adding intermediate-message cleanup (#7592), and collapsing long Feishu "thinking" cards (#7570).
- **Channel-instance addressing:** ZeroClaw cron delivery (#9940) and heartbeat targets (#10670) mishandle `<type>.<alias>` composite keys; IronClaw is distinguishing "paired but disconnected" from "unpaired" assistant channels (#8076).

**Shared need:** a channel capability matrix (approvals, admin roles, instance addressing, message editing/cleanup) maintained consistently across adapters.

### 4.5 Tool-output/context hygiene (IronClaw, OpenClaw, QwenPaw, ZeroClaw, Hermes)
- IronClaw is making ephemeral command-result cards first-class UI: dismissal (#8069), stable sizing (#8071), menu navigation (#8068).
- OpenClaw users still want manual clearing of large tool results (#45503, stale since March). ZeroClaw reports tool-result truncation is invisible outside model context (#10115).
- QwenPaw contributors are adding tool-call visibility toggles (#7357) and streaming scroll-lock (#7356).
- Hermes has a composer z-index bug hiding todo items (#104723).

**Shared need:** lifecycle management for tool outputs — dismiss, shrink, truncate, summarize, and make truncation visible to users.

---

## 5. Differentiation Analysis

| Project | Feature Focus | Target Users | Technical Architecture |
|---|---|---|---|
| **OpenClaw** | Full-stack personal-AI gateway; broadest IM/client coverage; memory subsystem; plugins; multi-provider routing incl. local runtimes | Self-hosters and organizations running assistants across chat, desktop, and mobile | Multi-runtime orchestration (managed llama.cpp, Codex, MLX) behind a gateway; session-tree model; subagent lifecycle with reply authority; per-channel approval abstraction |
| **Hermes Agent** | Desktop-centric agent workspace: bot pinboard, group chats, email sessioning, cron/kanban workflow, ACP/IDE integration | Nous/Hermes-model users running agentic workflows from desktop with IDE/Code relay extensions | Desktop client + gateway split, migrating group-room state gateway-side (proposed); strong ACP/OpenCode relay; session visibility seams still being unified |
| **IronClaw** | Lean, high-performance agent gateway; WebUI command/slash-command polish; MCP diagnostics; channel-state clarity | Technical self-hosters wanting a native, dependency-light agent runtime | Rust-native (tokio, wasmtime/WASM component surface, WebUI); conservative, maintainer-driven change cadence |
| **QwenPaw** | Multi-provider agent hub in the AgentScope ecosystem; console/plugin/skill workflows; v2.2 beta stabilization | Developers building on Qwen/AgentScope plus mixed-provider setups (DeepSeek, LM Studio LAN, OpenAI-compatible) | Python/AgentScope runtime (RuntimeWebUI integration); modes (e.g., Advisor Mode paired advisor/worker loop); plugin and skill stores; very open to first-time contributors |
| **ZeroClaw** | Reliability-first coding/ACP agent: crash-safe turn persistence, daemon/RPC stability, budget/cron enforcement, supervised shell approval | Heavy IDE/coding-agent users running long-lived, multi-session assistants with ZeroCode quickstart | Daemon/RPC architecture with reload semantics; ACP/Code turn checkpoints; severity taxonomy (S0–S3); strong trusted-contributor maintainership |

The practical split: **OpenClaw = the broad platform**, Hermes = the polished desktop/workflow client, IronClaw = the lean native core, QwenPaw = the model-ecosystem/dev-tooling play, ZeroClaw = the reliability-and-durability specialist.

---

## 6. Community Momentum & Maturity

**Tier 1 — Industrial-scale iteration: OpenClaw**
Merges 225 PRs and closes 89 issues daily, operates release-validation CI for a named stable/candidate pair, and runs automated backlog hygiene. It is the only project whose bottleneck is *product decision throughput*, not engineering capacity.

**Tier 2 — Active stabilization with parallel feature work: QwenPaw and ZeroClaw**
- **QwenPaw** is the classic pre-GA profile: high issue inflow, first-time contributors filing+fixing bugs, meaningful refactors still landing (memory lifecycle #7561, theme tokens #7487), and a beta release-duty item closed today. v2.2.0 is clearly imminent but not yet shippable.
- **ZeroClaw** is contribution-rich (50 PRs touched) but triage-constrained: 0 issue closures and only 5 PR merges in 24h, with an open S0. Maintainers are visibly investing in trusted contributors (shepherding #9739, approving #10623), but the queue is growing faster than the merge rate.

**Tier 3 — Responsive but lower-volume: Hermes Agent**
Moderate activity (50 PRs, 5 merges, 9 issues) with the best issue→fix response time in the cohort (same-day fix PRs for ACP pollution and vision-relay 422s). Still resolving architectural direction (Desktop-local vs. gateway-side group sessions) and PR overlap (#76774/#77359).

**Tier 4 — Steady maintenance: IronClaw**
Zero issue inflow, three dependency merges, and a queue of careful WebUI/MCP polish PRs awaiting review. The healthiest *codebase* signal in the cohort, but the least community momentum. Its wasm dependency PR (#7834) has sat since Aug 23 and needs a maintainer decision.

---

## 7. Trend Signals

**1. Crash-safe agent turns are becoming table stakes.**
Evidence: ZeroClaw S0 #10121 (mid-turn process exit loses work), QwenPaw #7579 (replies vanish after persistence), OpenClaw #118018 (stale subagent completion), Hermes #104724 (empty ACP sessions). *Value for developers:* persist every assistant/tool segment before execution continues; design sessions as append-only logs with idempotent resumption, not in-memory chat buffers.

**2. The desktop client is becoming a view, not the runtime.**
Evidence: Hermes #97681/#95163 (bots must outlive Desktop), OpenClaw #140010 (sleep/resume must not kill reachability), QwenPaw #7363 (UI event-loop blocking stalls the whole agent), ZeroClaw #10230 (daemon reload crashes). *Value:* build headless-first — authoritative state server-side, clients as reconnectable surfaces, and test reload/wake/resume paths explicitly.

**3. Provider cost engineering is now a core platform feature.**
Evidence: Anthropic cache breakpoint/TTL work in OpenClaw (#140129) and ZeroClaw (#10660–#10663); cache markers on OpenAI-completions transports (#140781); hardcoded context-window regression in QwenPaw (#7576); missing usage/cost pages in OpenClaw (#87110). *Value:* expose cache-hit ratios and per-session cost; make `cache_control`, TTL, and context-window sizes configurable and dynamically derived — never hardcoded.

**4. Chat platforms are becoming the ops console for agents.**
Evidence: approvals/admin controls across OpenClaw (#140645, #140798, #104521), Telegram guest-bot/bot-to-bot demand (#79077), QwenPaw Telegram/Feishu output cleanup (#7590/#7592/#7570), ZeroClaw heartbeat/cron channel-instance bugs (#9940/#10670). *Value:* model channels as a capability matrix (approve, admin, edit, delete, instance-address); design for bot-to-bot communication, not just human-to-bot.

**5. Silent failure is the most corrosive failure mode.**
Evidence: OpenClaw P0 Discord setup reports "configured/stopped" while hung (#140497); ZeroClaw unauthenticated `/health` leaks component errors (#10606); QwenPaw memory "dreaming" reports "Promoted 0 forever" with no surfaced disagreement (OpenClaw #121232 is the same pattern); QwenPaw FTS corruption silently breaks retention (#7596). *Value:* validate configuration by calling the real API at setup time; fail loudly with human-readable errors; sanitize but never hide component state in health endpoints.

**6. Multi-model orchestration is separating planner, worker, and coder roles.**
Evidence: QwenPaw Advisor Mode (#7569) pairs advisor/worker models; Hermes adds a dedicated model route for `/plan` (#104735); OpenClaw enforces sequential-only tooling in Code Mode (#140767) and dispatches nested subagents (#140137); ZeroClaw routes supervised shell work (#10241). *Value:* architect model routing by *task role*, with per-role providers, budgets, and tool restrictions — a single "best model" setting will not survive production use.

**7. Tool output needs an explicit lifecycle.**
Evidence: IronClaw's dismissible/stably-sized command-result cards (#8069/#8071), OpenClaw's stale manual-context-clearing request (#45503), ZeroClaw's invisible truncation (#10115), QwenPaw's tool-call visibility toggles (#7357). *Value:* give users dismiss, collapse, truncate, and "clear context" controls over tool results, with truncation always surfaced in the UI — otherwise long-running agents drown in their own output.

---

**Bottom line for decision-makers:** the ecosystem's center of gravity has shifted from model capability to *operational reliability* — durable sessions, headless operation, cost visibility, and channel-grade UX. OpenClaw leads in breadth and throughput but shares the cohort's open reliability debt; Hermes and QwenPaw offer the most responsive communities per issue; IronClaw is the low-risk, low-momentum option; ZeroClaw is the one to watch for durability patterns that will likely become industry defaults.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-09-07

## Today's Overview
Hermes Agent saw heavy activity in the last 24 hours: 9 issues were updated (all still open), and 50 PRs were updated, of which 45 remain open and 5 were merged or closed. No new release was published, so there is no user-facing version change to propagate. The busiest areas were session lifecycle and state persistence, ACP/email/gateway handling, Desktop UI polish, provider-specific model routing, and cron/kanban fixes. A notable pattern this window is the rapid pairing of bug reports with fix PRs on the same day, especially for ACP session pollution and Console Go vision relay failures.

## Releases
No new releases were published in the last 24 hours.

## Project Progress
Five PRs moved to merged/closed status in this window. The visible closed items were:

- [#104715 — `feat(cron): support atomic disabled job creation`](https://github.com/NousResearch/hermes-agent/pull/104715)  
  Added `hermes cron create ... --disabled`, persisting one inert paused record instead of creating an enabled job and pausing it later.
- [#104730 — `feat(desktop): add bot pinboard above sessions`](https://github.com/NousResearch/hermes-agent/pull/104730)  
  Added a compact bot pinboard above the Desktop sessions area, reusing the existing bots roster and routing without changing the full Bots management pane.

Other notable PRs advancing features or fixes, though still open:

- [#104739 — `fix(acp): never persist empty probe sessions; prune can sweep legacy ACP shells`](https://github.com/NousResearch/hermes-agent/pull/104739)  
  Targets the ACP empty-session pollution issue.
- [#104735 — `feat: add a dedicated model route for /plan`](https://github.com/NousResearch/hermes-agent/pull/104735)  
  Adds optional `planning` provider/model configuration for a single `/plan` turn.
- [#104148 — `fix(email): isolate sessions by normalized subject when opted in`](https://github.com/NousResearch/hermes-agent/pull/104148)  
  Implements subject-based email session isolation.
- [#104734](https://github.com/NousResearch/hermes-agent/pull/104734) and [#104736](https://github.com/NousResearch/hermes-agent/pull/104736)  
  Two fix candidates for the Console Go/OpenCode relay 422 vision error.

## Community Hot Topics
The most-discussed issues by comment volume were:

- [#66616 — `[skills-index-watchdog] Skills index is stale or degraded`](https://github.com/NousResearch/hermes-agent/issues/66616) — **170 comments**  
  This automated freshness failure has generated sustained discussion. The underlying problem is operational: the Skills Hub index is rebuilt on a cron schedule, and the index was 29.8h old against a 26h limit. The high comment count suggests ongoing noisy/automated watchdog activity more than a controversial design question.
- [#97681 — `Bot Group Chats should keep working after Desktop closes`](https://github.com/NousResearch/hermes-agent/issues/97681) — **25 comments**  
  Users want group chats to be backend-resident so bots can continue running on laptops, home servers, or VPSes and conversations can be resumed from another device.
- [#95163 — `Opt-in backend-hosted group rooms — gateway-side round driver + authoritative room log`](https://github.com/NousResearch/hermes-agent/issues/95163) — **14 comments, 1 👍**  
  A technical proposal to move group-room orchestration out of the Desktop renderer and into the gateway. Combined with #97681, this is a clear roadmap signal around persistent, multi-device bot collaboration.
- [#26277 — `Feature request: optional email session isolation by normalized subject`](https://github.com/NousResearch/hermes-agent/issues/26277) — **10 comments, 2 👍**  
  Users want separate Hermes sessions for separate email topics rather than one continuous session per sender. An implementation PR now exists.

No PR comment counts were populated in this snapshot, but the most active PR threads were likely those tied to session visibility, gateway delivery, and ACP state fixes.

## Bugs & Stability
There were no closed issues in the last 24 hours. Reported and continuing bugs, ranked by expected user impact:

1. [#104731 — `HTTP 422 from Console Go relay: vision_analyze embeds image parts in tool messages upstream rejects`](https://github.com/NousResearch/hermes-agent/issues/104731)  
   A successful vision call through the OpenCode Go relay kills the turn with a non-retryable 422. Two same-day fix PRs exist: [#104734](https://github.com/NousResearch/hermes-agent/pull/104734) and [#104736](https://github.com/NousResearch/hermes-agent/pull/104736).

2. [#104724 — `ACP mints a permanent empty session row for every session/new that never receives a prompt`](https://github.com/NousResearch/hermes-agent/issues/104724)  
   Session discovery probes flood `state.db` with `message_count=0` rows that look like real conversations. Fix PR: [#104739](https://github.com/NousResearch/hermes-agent/pull/104739).

3. [#66616 — `Skills index is stale or degraded`](https://github.com/NousResearch/hermes-agent/issues/66616)  
   An ongoing documentation-infrastructure flake: the Skills Hub index is being served too old. No direct fix PR in this window.

Also in the PR queue are stability fixes for issues filed outside the visible issue set:

- [#104738](https://github.com/NousResearch/hermes-agent/pull/104738) — clears billing `failure_reason` on OAuth recovery and prevents an unexpiring billing latch.
- [#104737](https://github.com/NousResearch/hermes-agent/pull/104737) — reclaims resident session leases from dead Desktop lanes.
- [#104723](https://github.com/NousResearch/hermes-agent/pull/104723) — fixes a Desktop Composer z-index stack overflow hiding overflowing todo items.
- [#104721](https://github.com/NousResearch/hermes-agent/pull/104721) — fixes Kanban stop nudges to follow the originating worker run.

## Feature Requests & Roadmap Signals
The strongest roadmap signal is **persistent bot group chats**. The combination of [#97681](https://github.com/NousResearch/hermes-agent/issues/97681) and [#95163](https://github.com/NousResearch/hermes-agent/issues/95163) points toward moving group-room state and round-robin orchestration from Desktop-local storage into gateway-side authoritative sessions. This is likely a near-term feature target given the existing session-state workstreams.

Other feature signals:

- [#26277 — Email session isolation by normalized subject](https://github.com/NousResearch/hermes-agent/issues/26277) has an open implementation PR and could land soon.
- [#104735 — Dedicated model route for `/plan`](https://github.com/NousResearch/hermes-agent/pull/104735) is small, optional, and config-driven, making it a plausible upcoming addition.
- [#104729 — Desktop slash autocomplete full-description hover](https://github.com/NousResearch/hermes-agent/issues/104729) is a low-risk UX enhancement.
- [#104720 — Dashboard light/cream theme](https://github.com/NousResearch/hermes-agent/issues/104720) responds to UI accessibility feedback.
- [#100655 — Opt-in pre-lifecycle boundary for external applications](https://github.com/NousResearch/hermes-agent/issues/100655) is explicitly seeking maintainer direction rather than implementation approval.

## User Feedback Summary
User pain points in this window were mostly around **state hygiene**, **cross-device continuity**, and **UI polish**:

- The Dashboard default theme drew unusually strong dissatisfaction: one user called the defaults “absolutely terrible,” “horribly dark and monochromatic,” and “painful on the eyes” ([#104720](https://github.com/NousResearch/hermes-agent/issues/104720)).
- Desktop users find slash-command descriptions truncated with no way to read the full text ([#104729](https://github.com/NousResearch/hermes-agent/issues/104729)).
- ACP clients and editor model-discovery probes are creating confusing empty sessions that look like real chats ([#104724](https://github.com/NousResearch/hermes-agent/issues/104724)).
- Users want group-chat bots to keep working independently of a running Desktop client, with their own models, tools, and credentials ([#97681](https://github.com/NousResearch/hermes-agent/issues/97681)).
- Email users want multi-topic conversations with the same sender to stop merging into one session ([#26277](https://github.com/NousResearch/hermes-agent/issues/26277)).

## Backlog Watch
Several meaningful items appear to need maintainer review or decision:

- [#66616 — Skills index stale/degraded](https://github.com/NousResearch/hermes-agent/issues/66616) has been open since July and carries 170 comments, but remains unresolved.
- [#26277 — Email session isolation by subject](https://github.com/NousResearch/hermes-agent/issues/26277) has been open since May; an implementation PR now exists ([#104148](https://github.com/NousResearch/hermes-agent/pull/104148)) and needs evaluation.
- [#53544 — Recover undelivered tool-call content](https://github.com/NousResearch/hermes-agent/pull/53544) is a P2 core-agent delivery bug-fix PR open since late June.
- [#76774 — Unify session-source visibility across all resume surfaces](https://github.com/NousResearch/hermes-agent/pull/76774) and [#77359 — Unify cross-source session visibility for `/sessions` and `/resume`](https://github.com/NousResearch/hermes-agent/pull/77359) are overlapping open PRs that should probably be consolidated.
- [#85744 — Show persisted usage without live agent](https://github.com/NousResearch/hermes-agent/pull/85744) has been open since August and needs a decision.
- [#100655 — Pre-lifecycle boundary for external applications](https://github.com/NousResearch/hermes-agent/issues/100655) is explicitly waiting on maintainer input, not implementation.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# Ironclaw Project Digest — 2026-09-07

## Today's Overview

Ironclaw showed steady maintainer-driven activity in the 24 hours ending 2026-09-07: 0 Issues were updated, while 13 PRs were updated (10 open, 3 closed/merged). No new releases were published. Active work focused on WebUI command-result/slash-command polish ([#8071](https://github.com/nearai/ironclaw/pull/8071), [#8070](https://github.com/nearai/ironclaw/pull/8070), [#8069](https://github.com/nearai/ironclaw/pull/8069), [#8068](https://github.com/nearai/ironclaw/pull/8068)), assistant channel-state messaging ([#8076](https://github.com/nearai/ironclaw/pull/8076)), MCP diagnostics ([#8077](https://github.com/nearai/ironclaw/pull/8077)), and automated dependency bumps. The three closed/merged PRs were dependency/CI housekeeping updates, so no user-facing feature landed in this window. Overall project health appears stable, with low incoming issue volume and continued core-contributor refinement work.

## Releases

No new releases were published during this period.

## Project Progress

Three PRs were closed/merged during the window, all dependency/CI updates:

- [PR #8049](https://github.com/nearai/ironclaw/pull/8049) — `chore(deps)`: bump the “everything-else” Rust dependency group with 19 updates.
- [PR #7835](https://github.com/nearai/ironclaw/pull/7835) — `chore(deps)`: bump GitHub Actions group with 5 updates.
- [PR #7020](https://github.com/nearai/ironclaw/pull/7020) — `chore(deps)`: bump `tokio-tungstenite` from 0.29.0 to 0.30.0 in the tokio-ecosystem group.

These were maintenance-focused merges rather than feature or bugfix landings. The more feature-oriented work remains in-flight: four WebUI slash-command/command-result fixes from a core contributor ([#8071](https://github.com/nearai/ironclaw/pull/8071), [#8070](https://github.com/nearai/ironclaw/pull/8070), [#8069](https://github.com/nearai/ironclaw/pull/8069), [#8068](https://github.com/nearai/ironclaw/pull/8068)), plus assistant shared-channel handling ([#8076](https://github.com/nearai/ironclaw/pull/8076)) and MCP response-leak diagnostics ([#8077](https://github.com/nearai/ironclaw/pull/8077)).

## Community Hot Topics

No Issues were updated in the window, and no PRs in the dataset show meaningful comment/reaction activity, so there are no true community discussion threads to highlight. The most active workstreams are:

- **WebUI slash-command and command-result polish** — four related PRs from `italic-jinxin`: preserve command-result card height ([#8071](https://github.com/nearai/ironclaw/pull/8071)), align slash-command metadata ([#8070](https://github.com/nearai/ironclaw/pull/8070)), add dismiss actions to command-result cards ([#8069](https://github.com/nearai/ironclaw/pull/8069)), and keep active slash command visible ([#8068](https://github.com/nearai/ironclaw/pull/8068)).
- **Assistant channel-state clarity** — [PR #8076](https://github.com/nearai/ironclaw/pull/8076) distinguishes a paired user's disconnected shared channel from an unpaired account.
- **Dependency automation pressure** — large Dependabot batch updates remain open ([#8080](https://github.com/nearai/ironclaw/pull/8080), [#8078](https://github.com/nearai/ironclaw/pull/8078), [#8079](https://github.com/nearai/ironclaw/pull/8079), [#7834](https://github.com/nearai/ironclaw/pull/7834)).

The underlying signal is a project in refinement mode: improving WebUI usability around ephemeral command results, making assistant error states more understandable, and keeping dependencies current.

## Bugs & Stability

No crash-level bugs or new Issue reports were recorded in the window. Several open fix PRs target existing defects or rough edges, ordered by likely severity:

1. **MCP response-leak diagnostic misclassification** — [PR #8077](https://github.com/nearai/ironclaw/pull/8077), which closes issue #8009, fixes MCP egress diagnostics so leak-blocking remains safe while preserving a distinct MCP-visible reason. This is the most safety-adjacent fix in the batch.
2. **Disconnected shared-channel confusion** — [PR #8076](https://github.com/nearai/ironclaw/pull/8076) fixes incorrect UX when a paired user’s shared channel is disconnected versus never paired.
3. **Command-result card height collapse** — [PR #8071](https://github.com/nearai/ironclaw/pull/8071) prevents structured command-result cards from shrinking/collapsing inside the transcript viewport.
4. **Missing dismiss actions on command-result cards** — [PR #8069](https://github.com/nearai/ironclaw/pull/8069) adds accessible dismissal for ephemeral command results while preserving durable messages.
5. **Active slash-command option can be hidden** — [PR #8068](https://github.com/nearai/ironclaw/pull/8068) fixes keyboard and pointer navigation visibility in the command menu.
6. **Slash-command metadata misalignment** — [PR #8070](https://github.com/nearai/ironclaw/pull/8070) fixes variable-width layout issues; primarily cosmetic.

## Feature Requests & Roadmap Signals

No new user-submitted feature requests appeared in the Issue tracker during this window. The open PRs nevertheless provide roadmap signals:

- **Ephemeral command-result lifecycle** is becoming first-class UI behavior: dismissal ([#8069](https://github.com/nearai/ironclaw/pull/8069)), stable sizing ([#8071](https://github.com/nearai/ironclaw/pull/8071)), and better menu navigation ([#8068](https://github.com/nearai/ironclaw/pull/8068)).
- **Assistant channel diagnostics** are being improved so users get channel-specific guidance ([#8076](https://github.com/nearai/ironclaw/pull/8076)).
- **MCP diagnostics** are being standardized across product, adapter, and OpenAI-compatible surfaces ([#8077](https://github.com/nearai/ironclaw/pull/8077)).

If these PRs merge cleanly, the next release will likely include meaningful WebUI command-menu/result improvements and more accurate assistant/MCP error messaging.

## User Feedback Summary

There is not enough Issue or comment data this window to measure satisfaction directly. Indirectly, the open fixes reflect likely user pain points: long or accumulated command results becoming hard to view, inability to dismiss ephemeral command cards, confusing behavior when slash-command options are off-screen, and unclear shared-channel or MCP rejection messages. No explicit negative feedback was reported through GitHub Issues in this 24-hour period.

## Backlog Watch

- **[PR #7834](https://github.com/nearai/ironclaw/pull/7834)** remains the most notable long-open PR: a Dependabot bump of the `wasm` group (wasmtime, wasmtime-wasi, wit-component, wit-parser), created 2026-08-23, risk marked medium, still open after being updated on 2026-09-06. It needs a maintainer decision: merge, update, or close.
- No long-unanswered Issues were present in the dataset. The older dependency PR backlog is being cleared, as seen by the closure of [#7020](https://github.com/nearai/ironclaw/pull/7020), [#7835](https://github.com/nearai/ironclaw/pull/7835), and [#8049](https://github.com/nearai/ironclaw/pull/8049).

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

## QwenPaw Project Digest — 2026-09-07

### 1. Today's Overview

QwenPaw entered 2026-09-07 with high but release-quiet activity: 25 issues were updated in the last 24 hours (18 still open) and 26 PRs were updated (20 still open). No new release was published during this window; the most recent public signal remains the v2.2.0-beta.7 release-duty item. The update flow is dominated by v2.2.x bug reports and first-time-contributor fixes, with recurring themes around task queue behavior, context persistence, streaming/channel rendering, and tool-call error visibility. Overall, the project appears to be in an active stabilization phase, with meaningful feature work still progressing in parallel.

---

### 2. Releases

**None during this period.**

No new release artifacts were published in the last 24 hours. The latest tracked item is the earlier v2.2.0-beta.7 installation-verification duty issue, which was updated/closed on 2026-09-07:

- [QwenPaw v2.2.0-beta.7 Release Duty #7503](https://github.com/agentscope-ai/QwenPaw/issues/7503)

---

### 3. Project Progress

Twenty-six PRs were updated in the last 24 hours; six are listed as merged/closed. Visible closed PRs in the sample:

- [#7595 fix(console): unify language selector options](https://github.com/agentscope-ai/QwenPaw/pull/7595) — Closed; aligns language choices between the top dropdown and sidebar settings.
- [#7086 fix(console): unify language options between settings gear and dropdown](https://github.com/agentscope-ai/QwenPaw/pull/7086) — Also closed, likely superseded by the above or by an equivalent change.

Open PRs with notable forward progress this cycle:

- [#7561 refactor(memory): unify automatic memory lifecycle and actions](https://github.com/agentscope-ai/QwenPaw/pull/7561) — Significant memory-manager contract change.
- [#7487 feat/theme token unification](https://github.com/agentscope-ai/QwenPaw/pull/7487) — Migrates multiple UI surfaces to centralized semantic theme tokens.
- [#7502 feat(console): redesign sidebar and settings experience](https://github.com/agentscope-ai/QwenPaw/pull/7502)
- [#7486 feat(creator) 1.1.2](https://github.com/agentscope-ai/QwenPaw/pull/7486) — Large plugin update: async delegation, A/B compare, media scheduling, Docker deployment.
- [#7509 feat(skill): Update make-skill to v2](https://github.com/agentscope-ai/QwenPaw/pull/7509) — Approval-driven draft-then-publish workflow.
- [#7569 feat(modes): add Advisor Mode](https://github.com/agentscope-ai/QwenPaw/pull/7569) — Adds a paired advisor/worker model loop mode.
- [#7382 feat(chat): adapt AgentScopeRuntimeWebUI 1.2.0 APIs](https://github.com/agentscope-ai/QwenPaw/pull/7382)

Several first-time-contributor fix PRs are also waiting on review and correspond to urgent bugs from this week:

- [#7577 fix(console): enqueue follow-up messages when chat task is running](https://github.com/agentscope-ai/QwenPaw/pull/7577)
- [#7578 fix(tool_calls): log exceptions in coordinator _drain()](https://github.com/agentscope-ai/QwenPaw/pull/7578)
- [#7593 feat(console): restore session direct path input alongside picker](https://github.com/agentscope-ai/QwenPaw/pull/7593)
- [#7592 feat(telegram): optional cleanup of intermediate messages after final answer](https://github.com/agentscope-ai/QwenPaw/pull/7592)
- [#7590 fix(telegram): render Markdown tables as `<pre>` instead of raw pipes](https://github.com/agentscope-ai/QwenPaw/pull/7590)

---

### 4. Community Hot Topics

The most active issues by comment volume are:

- [#7505 qwenpaw accessing LAN LLM server frequently disconnects and times out](https://github.com/agentscope-ai/QwenPaw/issues/7505) — 12 comments. Closed. Users running local LM Studio servers see repeated `client disconnect` retries and eventual timeout. This suggests a need for better connection reuse/retry handling for OpenAI-compatible LAN endpoints.

- [#7559 Sending a new message while a task is running triggers 409](https://github.com/agentscope-ai/QwenPaw/issues/7559) — 5 comments. Users expect follow-up messages to enter a queue, not be rejected. Linked to open fix PR [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577).

- [#6820 UI does not show model output / tool calls until everything is finished](https://github.com/agentscope-ai/QwenPaw/issues/6820) — 5 comments. Older closed issue; still reflective of user expectations around streaming visibility.

- [#7587 OpenAI-compatible provider gets Cloudflare 403 with WUSRouter](https://github.com/agentscope-ai/QwenPaw/issues/7587) — 4 comments. Integration gap in provider connectivity / model-list fetching.

- [#7513 DeepSeek conversation mixes model responses with QwenPaw tool calls](https://github.com/agentscope-ai/QwenPaw/issues/7513) — 4 comments. User reports other agent tools are unaffected; points to parsing/handling issues at the tool-call boundary.

- [#7363 Synchronous calls freeze the event loop and timeout never fires](https://github.com/agentscope-ai/QwenPaw/issues/7363) — 4 comments. Serious Windows desktop performance issue during startup and message sends.

Underlying needs: users increasingly depend on QwenPaw as a multi-provider agent hub, and they are requesting smoother streaming, safe task queueing, and less brittle HTTP/provider behavior.

---

### 5. Bugs & Stability

**Critical / high severity**

- [#7579 Model replies unexpectedly disappear from context after persistence](https://github.com/agentscope-ai/QwenPaw/issues/7579) — Open. The model “cannot see its own previous response,” leading to empty responses and repeated tool loops. Related severe report [#7584](https://github.com/agentscope-ai/QwenPaw/issues/7584) was closed as a duplicate. No explicit fix PR yet.

- [#7589 Heartbeat cron session feedback loop / duplicate message pile-up](https://github.com/agentscope-ai/QwenPaw/issues/7589) — High severity; agent was unresponsive for ~2 hours. Verified against latest `main`. No fix PR visible.

- [#7363 Synchronous calls freeze event loop and timeout never fires](https://github.com/agentscope-ai/QwenPaw/issues/7363) — Windows desktop freezes for 118–135s during startup and ~126s when sending messages.

- [#7567 Stop button reports task stopped, but the task keeps running](https://github.com/agentscope-ai/QwenPaw/issues/7567) — Causes subsequent 409s and incorrect execution of a mistaken command.

- [#7576 RetryChatModel hardcoded 32768 context_size fallback causes CONTEXT_UNFIT](https://github.com/agentscope-ai/QwenPaw/issues/7576) — Affects all published releases from v2.1.0 through v2.2.0; model context windows are not inferred correctly.

- [#7596 Scroll history.db FTS corruption undetected by integrity check](https://github.com/agentscope-ai/QwenPaw/issues/7596) — Causes retention purge to fail silently on every startup.

**Medium severity / need attention**

- [#7597 Tool-returned image/PDF binary sent as bare base64 triggers 400](https://github.com/agentscope-ai/QwenPaw/issues/7597) — New issue; causes conversation failures when tools return binary content.

- [#7559 409 error when user sends message during active task](https://github.com/agentscope-ai/QwenPaw/issues/7559) — Fix PR [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577) is open.

- [#7572 Tool dispatch layer swallows exception stacks in `_coordinator.py`](https://github.com/agentscope-ai/QwenPaw/issues/7572) — Hurts failure diagnosis; fix PR [#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578) is open.

- [#7587 Cloudflare 403 / “Just a moment” challenge with WUSRouter](https://github.com/agentscope-ai/QwenPaw/issues/7587)

- [#6541 Scroll context compression uses `role=user` on DeepSeek, causing MODEL_EXECUTION_ERROR](https://github.com/agentscope-ai/QwenPaw/issues/6541) — Still open from July.

- [#7585 Telegram Markdown tables appear as raw `|` and `---`](https://github.com/agentscope-ai/QwenPaw/issues/7585) — Fix PR [#7590](https://github.com/agentscope-ai/QwenPaw/pull/7590) is open.

- [#7505 LAN LLM server disconnects/client retries](https://github.com/agentscope-ai/QwenPaw/issues/7505) — Closed, but representative of persistent local-server reliability concerns.

---

### 6. Feature Requests & Roadmap Signals

Significant user-driven feature signals this week:

- **Restore direct path input for working directory**
  [#7588 [Feature] Restore v2.1.0 main working-directory switching](https://github.com/agentscope-ai/QwenPaw/issues/7588) — Users strongly prefer the old type-and-Enter workflow over the current directory picker. Open fix: [#7593](https://github.com/agentscope-ai/QwenPaw/pull/7593).

- **Cleaner channel output**
  - Telegram: auto-clean intermediate reasoning/tool messages after final answer — [#7586](https://github.com/agentscope-ai/QwenPaw/issues/7586), with PR [#7592](https://github.com/agentscope-ai/QwenPaw/pull/7592).
  - Feishu: automatically collapse long “thinking” cards after streaming ends — [#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570). User even provided a local implementation approach.

- **Plugin store usability**
  [#7582 Plugin store needs one-click updates and update notifications](https://github.com/agentscope-ai/QwenPaw/issues/7582) — Users managing multiple QwenPaw installs find the current store flow too click-heavy and easy to lose context in.

- **Chat readability controls**
  Open PRs from community contributors are already anticipating the next version:
  - Tool-call visibility toggle: [#7357](https://github.com/agentscope-ai/QwenPaw/pull/7357)
  - Chat scroll lock while streaming: [#7356](https://github.com/agentscope-ai/QwenPaw/pull/7356)
  - Rich input caret visibility fix: [#7347](https://github.com/agentscope-ai/QwenPaw/pull/7347)

- **Older request**
  [#4077 UI font scaling & clickable file-path links](https://github.com/agentscope-ai/QwenPaw/issues/4077) — Closed, but the need is likely to reappear as desktop usage grows.

Probably next-version candidates: restored path input, Telegram output cleanup, Markdown table handling, plugin-store improvements, and one or more console/chat UX PRs.

---

### 7. User Feedback Summary

Real user pain points this week show a mix of technical and workflow dissatisfaction:

- Task control is not yet trustable: users hit 409s when queueing new messages, stop buttons are not honored, and duplicate task outputs appear at odd intervals.
  [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559), [#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567), [#7594](https://github.com/agentscope-ai/QwenPaw/issues/7594)

- Long-term instruction / memory adherence is a visible pain point, especially for plugin-development use cases where QwenPaw forgets desired working paths after a few days.
  [#7571](https://github.com/agentscope-ai/QwenPaw/issues/7571)

- Power users feel that v2.2.0 regressed useful v2.1.0 desktop/console interactions, particularly direct working-directory path entry.
  [#7588](https://github.com/agentscope-ai/QwenPaw/issues/7588)

- Channel users find output noisy and under-rendered on Telegram/Feishu, especially reasoning text, tool calls, and Markdown tables.
  [#7585](https://github.com/agentscope-ai/QwenPaw/issues/7585), [#7586](https://github.com/agentscope-ai/QwenPaw/issues/7586), [#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570)

- Several reports include high-quality diagnostic detail, and new contributors are voluntarily submitting fix PRs for the issues they filed. That is a strong positive health signal.

---

### 8. Backlog Watch

Issues/PRs that appear important and still need maintainer attention:

- [#6541 Scroll context compression triggers MODEL_EXECUTION_ERROR on DeepSeek](https://github.com/agentscope-ai/QwenPaw/issues/6541) — Open since 2026-07-29; no linked fix PR seen. High-impact for DeepSeek API users.

- [#7363 Synchronous calls freeze the event loop and timeout never fires](https://github.com/agentscope-ai/QwenPaw/issues/7363) — Open since 2026-08-27; Windows desktop performance issue with no visible fix.

- [#7576 RetryChatModel hardcoded context window fallback](https://github.com/agentscope-ai/QwenPaw/issues/7576) — Reported as present in all released v2.1–v2.2 builds; no fix PR yet.

- [#7589 Heartbeat cron duplicate-message feedback loop](https://github.com/agentscope-ai/QwenPaw/issues/7589) — High severity, recent, no observable maintainer response or fix PR yet.

- [#7596 Scroll `history.db` FTS corruption](https://github.com/agentscope-ai/QwenPaw/issues/7596) — Silent data corruption issue reported today; needs a durable repair strategy.

Open PRs that deserve prompt maintainer review because they fix recently reported, high-signal bugs:

- [#7577 Enqueue follow-up messages instead of returning HTTP 409](https://github.com/agentscope-ai/QwenPaw/pull/7577)
- [#7578 Log exceptions in tool-coordinator `_drain()`](https://github.com/agentscope-ai/QwenPaw/pull/7578)
- [#7590 Telegram Markdown table fallback rendering](https://github.com/agentscope-ai/QwenPaw/pull/7590)
- [#7592 Telegram intermediate-message cleanup](https://github.com/agentscope-ai/QwenPaw/pull/7592)
- [#7593 Restore direct path input for working-directory switching](https://github.com/agentscope-ai/QwenPaw/pull/7593)

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-09-07

## 1. Today's Overview
ZeroClaw shows very high activity: 14 issues were updated in the last 24 hours (all remaining open, none closed) and 50 PRs were touched — 45 still open and 5 merged/closed. No new releases were published. The dominant themes are ACP/Code turn durability and data-loss prevention ([#9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333), [#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121), [#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659)), daemon/RPC stability under reload ([#10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230)), and Anthropic prompt-cache tuning ([#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660), [#10662](https://github.com/zeroclaw-labs/zeroclaw/issues/10662), [#10663](https://github.com/zeroclaw-labs/zeroclaw/issues/10663)). The maintainer team is actively shepherding large community PRs, but the all-open issue list and zero closed issues suggest triage throughput is still catching up with the report volume. Overall project health is solid on contribution volume, with the main risk concentrated in turn-lifecycle reliability and a growing queue of P1 bugs.

## 2. Releases
No new ZeroClaw releases in the last 24 hours. No changelog, breaking-change, or migration notes to report.

## 3. Project Progress
- 5 PRs were merged or closed in the window; those items were not included in the visible top-20 data, so specifics are unavailable. No issues were closed.
- Features and fixes that advanced visibly (all still open):
  - [PR #10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197) — fix(acp): checkpoint and recover interrupted Code/ACP turn progress; directly targets the S0/S1 persistence family ([#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121), [#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659)).
  - [PR #9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378) — fix(acp): persist failed and cancelled turn transcripts; sibling of #10197, currently `stale-candidate`/`needs-author-action`.
  - [PR #10654](https://github.com/zeroclaw-labs/zeroclaw/pull/10654) — fix(runtime): bound RPC dispatch stack usage, with structural regression test; mitigates the stack-overflow risk behind [#10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230).
  - [PR #10262](https://github.com/zeroclaw-labs/zeroclaw/pull/10262) — fix(rpc): close RPC connections on daemon reload and unstick ZeroCode quickstart.
  - [PR #10623](https://github.com/zeroclaw-labs/zeroclaw/pull/10623) (approved 09-05) and stacked [PR #10666](https://github.com/zeroclaw-labs/zeroclaw/pull/10666) — Anthropic prompt-cache passthrough plus a third cache breakpoint (#10666 closes [#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660)).
  - [PR #9739](https://github.com/zeroclaw-labs/zeroclaw/pull/9739) — multi-session panes with agent sidebar; maintainer completed reconnect and lifecycle repairs.
- Other large in-flight features: clickable transcript URLs ([#10386](https://github.com/zeroclaw-labs/zeroclaw/pull/10386)), persistent session prompt attachments ([#10407](https://github.com/zeroclaw-labs/zeroclaw/pull/10407)), and live provider identity on usage events ([#8966](https://github.com/zeroclaw-labs/zeroclaw/pull/8966)).

## 4. Community Hot Topics
Most active issues by comment count (all with 0 reactions):
- [Issue #10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230) (6 comments) — Daemon startup/reload stack overflow during agent initialization when a Quickstart config is applied; S1, needs-repro, in progress.
- [Issue #9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333) (4 comments) — Failed ACP turns disappear after switching sessions; S1, in progress with fix candidates [#9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378)/[#10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197).
- [Issue #10408](https://github.com/zeroclaw-labs/zeroclaw/issues/10408) (3 comments) — Second message during an active turn starts a parallel run, producing duplicate work and duplicate replies; S2.
- [Issue #10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121) (3 comments) — Partial Code/ACP turns disappear if the process exits mid-turn; S0 data loss / security risk.
- [Issue #9191](https://github.com/zeroclaw-labs/zeroclaw/issues/9191) (3 comments) — Cron agent jobs lack wall-clock timeout; in-flight locks only cleared at process start; S1.

Underlying need: users are operating ZeroClaw as a long-running, multi-session assistant and are hitting lifecycle race conditions — messages sent during active turns, session switches, budget exhaustion, and process exit all risk lost or duplicated work. The common demand is durable, ordered, crash-safe session execution.

## 5. Bugs & Stability
Ranked by severity; fix-PR status noted where one exists.

- **S0 — data loss / security risk**
  - [#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121) — Partial Code/ACP turns vanish if the process exits before completion. Fix in review: [PR #10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197).
- **S1 — workflow blocked**
  - [#10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230) — Stack overflow in daemon startup/reload during agent initialization; related hardening in [PR #10654](https://github.com/zeroclaw-labs/zeroclaw/pull/10654) and [PR #10262](https://github.com/zeroclaw-labs/zeroclaw/pull/10262).
  - [#9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333) — Failed ACP turns disappear after session switching. Fix candidate (stale): [PR #9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378).
  - [#9191](https://github.com/zeroclaw-labs/zeroclaw/issues/9191) — Cron jobs never time out; stale in-flight locks survive until restart. No fix PR attached yet.
  - [#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659) — Budget-exceeded Code turn discards already-streamed progress after session restore (new 09-06).
  - [#10670](https://github.com/zeroclaw-labs/zeroclaw/issues/10670) — `heartbeat.target` rejects channel instance composite key `<type>.<alias>`, breaking non-default channel instances (new 09-07).
- **S2 — degraded behavior**
  - [#10408](https://github.com/zeroclaw-labs/zeroclaw/issues/10408) — Parallel agent run on second message in the same session.
  - [#9940](https://github.com/zeroclaw-labs/zeroclaw/issues/9940) — Turn-context instructs agents to use an unresolvable cron delivery channel.
  - [#10115](https://github.com/zeroclaw-labs/zeroclaw/issues/10115) — Tool-result truncation is invisible outside the model's context.
- **S3 — minor**
  - [#10104](https://github.com/zeroclaw-labs/zeroclaw/issues/10104) — `zeroclaw-hardware` feature-gated lib tests never execute in CI.
- **Provider/security items**
  - [#10662](https://github.com/zeroclaw-labs/zeroclaw/issues/10662) — Anthropic OAuth system-prefix cache marker is below Anthropic's cache minimum and wastes a breakpoint slot (P2).
  - [#10606](https://github.com/zeroclaw-labs/zeroclaw/issues/10606) — Unauthenticated `/health` response leaks component `last_error`; accepted P1, no fix PR yet.

## 6. Feature Requests & Roadmap Signals
- **Anthropic cache efficiency is the clearest near-term roadmap cluster**: third cache breakpoint ([#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660)) has a stacked fix PR ([#10666](https://github.com/zeroclaw-labs/zeroclaw/pull/10666)) built on the already-approved [PR #10623](https://github.com/zeroclaw-labs/zeroclaw/pull/10623); configurable 1-hour prompt-cache TTL ([#10663](https://github.com/zeroclaw-labs/zeroclaw/issues/10663)) is also requested. This cache work is the most likely to land in the next release.
- **Security hardening**: [Issue #10606](https://github.com/zeroclaw-labs/zeroclaw/issues/10606) (sanitized component errors in unauthenticated health responses) is accepted P1 and a plausible release candidate.
- **Turn persistence roadmap**: PRs #10197/#9378 converging on crash-safe ACP transcripts are likely headline fixes for the next minor release, given the S0 severity of [#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121).
- **Longer-horizon user-facing features**: clickable transcript URLs ([PR #10386](https://github.com/zeroclaw-labs/zeroclaw/pull/10386)), persistent session prompt attachments ([PR #10407](https://github.com/zeroclaw-labs/zeroclaw/pull/10407)), and multi-session panes ([PR #9739](https://github.com/zeroclaw-labs/zeroclaw/pull/9739)).

## 7. User Feedback Summary
- **Data loss is the dominant pain point**: reporters describe visible assistant text, tool calls, and tool results disappearing after failed turns, session switches, budget exhaustion, or process exit ([#9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333), [#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121), [#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659)).
- **Concurrency expectations are explicit**: users expect one agent run per session; [#10408](https://github.com/zeroclaw-labs/zeroclaw/issues/10408) shows that expectation is currently violated, producing duplicate work and replies.
- **Channel/cron configuration is confusing**: cron delivery defaults and heartbeat targets handle channel instance addressing (`<type>.<alias>`) inconsistently ([#9940](https://github.com/zeroclaw-labs/zeroclaw/issues/9940), [#10670](https://github.com/zeroclaw-labs/zeroclaw/issues/10670)).
- **Cost control matters for heavy Anthropic users**: cache breakpoint/TTL issues ([#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660), [#10662](https://github.com/zeroclaw-labs/zeroclaw/issues/10662), [#10663](https://github.com/zeroclaw-labs/zeroclaw/issues/10663)) indicate real spend pressure and interest in reducing cache misses.
- **Satisfaction signal**: the maintainer team is visibly collaborating with distinguished/trusted contributors — completing bounded reconnect work on [PR #9739](https://github.com/zeroclaw-labs/zeroclaw/pull/9739), retaining original authorship, and approving [PR #10623](https://github.com/zeroclaw-labs/zeroclaw/pull/10623) — which indicates a healthy open-source contributor loop despite the bug backlog.

## 8. Backlog Watch
Items open the longest, or stalled, that need maintainer attention:
- [PR #8966](https://github.com/zeroclaw-labs/zeroclaw/pull/8966) (created 07-11) — live provider identity on usage events and correct context-window ceiling; `needs-author-action`, XL, touches many channel/runtime paths.
- [PR #9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109) (created 07-17) — native Hailo-Ollama provider; `do-not-merge`.
- [Issue #9191](https://github.com/zeroclaw-labs/zeroclaw/issues/9191) (created 07-20) — P1 S1 cron jobs without wall-clock timeout; accepted/no-stale but still no fix PR attached after ~7 weeks.
- [PR #9420](https://github.com/zeroclaw-labs/zeroclaw/pull/9420) (created 07-26) — Anthropic stored OAuth profiles; `do-not-merge` + blocked.
- [PR #9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378) (created 07-26) — ACP failed/cancelled turn persistence; `stale-candidate` and `needs-author-action`, with overlapping newer PR #10197 now moving faster — risks duplication or needs consolidation.
- [PR #9447](https://github.com/zeroclaw-labs/zeroclaw/pull/9447) (created 07-27) — classify incomplete Anthropic terminal responses; `needs-author-action`.
- [PR #10241](https://github.com/zeroclaw-labs/zeroclaw/pull/10241) (created 08-22) — restore supervised shell approval routing; status `blocked`.
- [PR #10356](https://github.com/zeroclaw-labs/zeroclaw/pull/10356) (created 08-25) — AnySearch web search provider; `blocked`, `do-not-merge`, `needs-maintainer-review`.
- [Issue #10104](https://github.com/zeroclaw-labs/zeroclaw/issues/10104) (created 08-18) — CI never runs hardware-gated lib tests; accepted P2 with follow-up but no visible movement.

</details>