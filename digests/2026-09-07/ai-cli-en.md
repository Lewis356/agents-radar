# AI CLI Tools Community Digest 2026-09-07

> Generated: 2026-09-07 04:41 UTC | Tools covered: 7

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/earendil-works/pi)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## Cross-Tool Comparison

# Cross-Tool AI CLI Comparison Report — 2026-09-07

## 1. Ecosystem Overview

The seven major AI CLI tools surveyed are simultaneously expanding surface area and paying down reliability debt. Community attention on 2026-09-07 is dominated by destructive-file-operation incidents, permission/guardrail regressions, unkillable or falsely-successful background work, and Windows-specific breakage — not by new feature demand. Meanwhile, PR queues show continued platform investment in managed worktrees, MCP transport verification, persistence optimization, process-lifecycle semantics, and WebShell control-plane tooling. Release cadence is bifurcating: Gemini CLI ships daily nightlies and Qwen Code published both a preview and a nightly in the window, while Claude Code, GitHub Copilot CLI, OpenCode, and Pi published no release. The overall picture is a market entering a “trust and hardening” phase where safety accounting — permission protocols, process/session cleanup, and cost/context transparency — has become the primary competitive axis.

## 2. Activity Comparison

Counts below reflect noteworthy items called out in each project’s daily digest for 2026-09-07, not raw GitHub event totals; all seven projects use GitHub Issues/PRs as active community channels, so no channel is marked N/A.

| Project | Hot Issues | Key PRs | Discussion Threads | Release in last 24 h |
|---|---|---|---|---|
| Claude Code (`anthropics/claude-code`) | 10 | 10 | Not reported in digest | None (explicit) |
| OpenAI Codex (`openai/codex`) | 10 | 10 | 8 (Ideas/Q&A/Show & tell) | Not reported in digest |
| Gemini CLI (`google-gemini/gemini-cli`) | 10 | 10 (11 named — #29131/#29132 pair) | Not reported in digest | Nightly `v0.60.0-nightly.20260907.g85aca163f` |
| GitHub Copilot CLI (`github/copilot-cli`) | 10 | 1 | None reported | None (explicit) |
| OpenCode (`anomalyco/opencode`) | 10 | 10 | None reported | None (explicit) |
| Pi (`earendil-works/pi`) | 10 | 10 | 1 | None (explicit) |
| Qwen Code (`QwenLM/qwen-code`) | 6 | 10 | Not reported in digest | `v0.23.1-preview.1` + `v0.23.0-nightly.20260906` |

**Notes:** OpenAI Codex leads engagement volume this cycle with the most discussion-headline activity; Gemini CLI and Qwen Code are the only projects shipping artifacts in the window; GitHub Copilot CLI is the least active at the PR/release level despite heavy issue load.

## 3. Shared Feature Directions

- **Memory/context lifecycle controls.** Multiple communities are asking for explicit control over what enters context and when. Claude Code users want configurable auto-memory compaction thresholds ([#91188](https://github.com/anthropics/claude-code/issues/91188)) and consistent `MEMORY.md` loading across git worktrees ([#81833](https://github.com/anthropics/claude-code/issues/81833)). Gemini CLI’s Auto Memory cluster requests deterministic redaction and an end to indefinite retry of low-signal sessions ([#26522](https://github.com/google-gemini/gemini-cli/issues/26522), [#26525](https://github.com/google-gemini/gemini-cli/issues/26525)). OpenCode shows the failure mode when provider usage is mis-reported and auto-compaction fires every turn ([#47296](https://github.com/anomalyco/opencode/issues/47296)).

- **Deterministic safety under autonomous execution.** Communities across Copilot CLI, Claude Code, Gemini CLI, Qwen Code, and OpenCode are requesting the same guarantees: no silent tool auto-approval (Copilot [#4537](https://github.com/github/copilot-cli/issues/4537)), process-tree cancellation that actually terminates destructive commands (Claude Code [#92593](https://github.com/anthropics/claude-code/issues/92593)), validation of destructive git arguments (Gemini [#29184](https://github.com/google-gemini/gemini-cli/pull/29184)), rollback logic that cannot discard hook-created commits (Qwen [#11253](https://github.com/QwenLM/qwen-code/issues/11253)), and circuit breakers for agent loops (OpenCode [#31942](https://github.com/anomalyco/opencode/issues/31942)).

- **Usage, cost, and rate-limit transparency.** OpenAI Codex users report entitlement inconsistencies between app and CLI ([#40939](https://github.com/openai/codex/issues/40939)); Copilot BYOK users report silently disabled prompt caching at ~5× cost ([#4720](https://github.com/github/copilot-cli/issues/4720)); Together AI reports all-zero usage in OpenCode ([#47716](https://github.com/anomalyco/opencode/issues/47716)); Pi is adding provider-reported cost and better cache breakpoints ([#6881](https://github.com/earendil-works/pi/pull/6881), [#9246](https://github.com/earendil-works/pi/issues/9246)); Qwen Code now displays live Goal token budgets ([#11254](https://github.com/QwenLM/qwen-code/pull/11254)). The shared need: one explainable accounting source that drives compaction, budgets, and billing consistently.

- **Background/subagent observability.** Gemini users want subagent trajectories included in `/chat share` and `/bug` reports ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763)); Copilot ACP users need an idle signal that covers background shells ([#4743](https://github.com/github/copilot-cli/issues/4743)); OpenCode serve users want per-request instances disposed ([#47727](https://github.com/anomalyco/opencode/issues/47727)); Qwen WebShell is adding shell/monitor task output visibility ([#10906](https://github.com/QwenLM/qwen-code/pull/10906)); OpenAI Codex added a managed-worktree browser to answer “what is running where” ([#43286](https://github.com/openai/codex/pull/43286)).

- **Windows and desktop-environment parity.** This is the widest cross-tool pain cluster. Examples include Claude Code’s MSYS `rm -rf` drive-root deletion, torn hook env files, case-sensitive worktree rejection, and read-only `$TMPDIR` ([#92593](https://github.com/anthropics/claude-code/issues/92593), [#78146](https://github.com/anthropics/claude-code/issues/78146), [#91618](https://github.com/anthropics/claude-code/issues/91618), [#92590](https://github.com/anthropics/claude-code/issues/92590)); OpenAI Codex missing the Windows Remote Control UI and failing PowerShell spawn ([#28919](https://github.com/openai/codex/issues/28919), [#17325](https://github.com/openai/codex/issues/17325)); Copilot’s ~31 GB WSL2 RSS ([#4694](https://github.com/github/copilot-cli/issues/4694)); Pi ignoring `shell_path` in favor of WSL bash ([#9229](https://github.com/earendil-works/pi/issues/9229)); and OpenCode desktop not forwarding the user shell `PATH` to plugin subprocesses ([#47710](https://github.com/anomalyco/opencode/issues/47710)).

- **Worktree/multi-session isolation as a correctness primitive.** OpenAI Codex is investing in managed worktree browsing and deferred transitions ([#43286](https://github.com/openai/codex/pull/43286), [#43298](https://github.com/openai/codex/pull/43298)); Claude Code struggles with memory loading differing per worktree ([#81833](https://github.com/anthropics/claude-code/issues/81833)); Copilot Desktop blocks a second Local session per project ([#4742](https://github.com/github/copilot-cli/issues/4742)); Qwen Code enables concurrent daemons only with session fencing ([#11207](https://github.com/QwenLM/qwen-code/pull/11207)). Developers clearly expect deterministic project/session identity as the substrate for rollback, memory, and parallelism.

## 4. Differentiation Analysis

- **Claude Code** is the broadest “assistant platform”: plugin/marketplace infrastructure, enterprise cyber-verification, auto-memory, Cowork, and desktop/editor integration. Its top thread at 197 comments shows large installed-base pressure — most friction is organizational (CVP blocking, model routing) rather than basic functionality. The day’s PR batch is plugin-infrastructure hardening (Windows paths, shell injection, symlink escapes), indicating a mature but security-constrained system.

- **OpenAI Codex** differentiates on runtime engineering: linearizable thread “rollout” ordinals, managed worktree pools, remote permission-profile fidelity, and MCP user-verification flows. It treats session history like an event-sourced ledger, which is why duplicate-ordinal projection bugs read as data loss even when JSONL is intact. Its community’s loudest single ask — turn-level `/rewind` ([#9618](https://github.com/openai/codex/discussions/9618), 119 👍) — reveals the cost of that design: linear history without user undo feels irreversible.

- **Gemini CLI** is the nightly-first, multi-agent research/ops tool: generalist, codebase-investigator, and browser subagents, plus sandboxing and OS-level bash safety discussions. Its standout weakness is self-report accuracy — `MAX_TURNS` interruptions reported as `GOAL` success ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)) — and under-utilization of custom skills/subagents by the model ([#21968](https://github.com/google-gemini/gemini-cli/issues/21968)). The trajectory is toward agent teams that need stronger observability and honest termination semantics.

- **GitHub Copilot CLI** is protocol- and compliance-centric: ACP session lifecycle, enterprise model defaults, GHEC data-residency endpoints, and permission re-request behavior. With only one documentation PR and no release in the window, it is the slowest-moving project at the surface level but carries the heaviest enterprise policy burden. Its top issue — an auto-approval regression in ACP mode ([#4537](https://github.com/github/copilot-cli/issues/4537)) — is the kind of trust-breaking bug that matters most for org rollout.

- **OpenCode** is the modular plugin-centric challenger: Promise/Effect plugin APIs, template commands, permission assertions, a Firecrawl developer-search provider, and a renderer persistence overhaul modeled on VS Code storage. It is also the least battle-tested at the operational layer — unbounded DB growth ([#47729](https://github.com/anomalyco/opencode/issues/47729)) and `serve` instance/MCP leaks ([#47727](https://github.com/anomalyco/opencode/issues/47727)) suggest the v2.0 beta is still catching up to long-running daemon workloads.

- **Pi** is the “universal connective tissue”: a single agent runtime that routes multiple backends (OpenAI Codex, Copilot GPT models, Anthropic, OpenRouter, OpenCode Go) and absorbs per-gateway quirks — endpoint routing, strict-schema flags, MagicDNS resolution, SSE parsing edge cases. Its differentiation is provider-agnostic transport plus an extension API for mid-turn steering, per-call confirmation, and fallback providers. The community is smaller but power-user-dense and unusually Windows-literate.

- **Qwen Code** is the control-plane/WebShell product: daemon session fencing, Goal token budgets in the prompt and UI, IPC review-gate refinement, and WebShell task output. It is also the most operationally automated open-source project in this set — a CI bot filed its own failure, marked it `autofix/in-progress`, and maintenance dashboards track fleet state. The focus is clearly long-running autonomous Goals rather than interactive IDE polish.

## 5. Community Momentum & Maturity

- **OpenAI Codex** has the strongest combined momentum this cycle: ten substantive merged PRs (MCP verification, permission profiles, worktree browser, Windows shutdown) plus the highest single-signal feature demand in any digest (119 👍 for rewind). This is a rapidly iterating project with a vocal, workflow-hungry community.

- **Claude Code** has the largest community by engagement depth — 197-comment and 151-👍 threads are the highest raw numbers across all digests — but little release motion in this 24-hour window. The pattern is a mature platform with enterprise-scale trust debt accumulating in org verification, hang severity, and Windows safety.

- **Gemini CLI** is the fastest shipper in the set: a nightly release plus multiple P1 fixes closed in one cycle (EOL Node bump, symlink glob regression, CRLF misclassification, SSE final-event loss). The community is active but comparably smaller; the maintainers’ nightly cadence compensates.

- **Qwen Code** is high-velocity and unusually self-automating: preview + nightly releases, ten substantive PRs (including a git data-loss fix in progress), plus CI auto-triage. The issue tracker had the fewest updates (6), suggesting a tighter user base or more activity flowing through other channels, but the engineering cadence is among the fastest.

- **OpenCode** shows concentrated refactor energy: a three-PR persistence overhaul closed in one window alongside plugin/desktop fixes. Community size still appears modest (most issues have 1–3 comments), and the v2.0 beta is visibly pre-hardening — but the architectural investments (content-addressed storage, permission assertions, template plugins) are significant.

- **Pi** demonstrates steady, maintainer-driven progress with 10 PRs despite no release. Its signature threads — a 76-comment openai-codex hang and a 57-comment Windows experience thread — reflect a small but engaged community using Pi as a daily-driver meta-agent across many backends.

- **GitHub Copilot CLI** is the least active in this window (1 doc PR, no release) and the most constrained by enterprise/determinism requirements. Its issue queue is full of high-severity regressions relative to surface velocity, which may indicate a conservative release process rather than low investment.

## 6. Trend Signals

- **Process lifecycle and rollback safety are not yet solved.** Destructive commands surviving timeout/backgrounding, rollback deleting hook-created commits, and `end_turn` arriving before background work finishes all point to the same conclusion: agents need transactional process trees and abort semantics as core primitives, not afterthoughts. For developers: design kill/idempotency/ref-compare into tool execution from day one.

- **Guardrails fail in both directions, and both erode trust.** Copilot CLI auto-approves tool calls without permission, while Claude Code’s safeguard system blocks CVP-approved orgs. The fix is not “more” or “less” gating — it is deterministic, auditable, org-policy-aware permission protocols with regression tests, so behavior is consistent across sessions and surfaces.

- **Autonomy without observability is unacceptable to users.** Budgets, rate-limit entitlements, subagent traces, provider-reported cost, actual model identity, and background-task state all surfaced as community demands this cycle. The emerging expectation is that an agent should expose an instrument panel — cost spent, tokens remaining, which subagents ran, what they changed — not just a final diff.

- **Cost and context accounting are now trust features.** Silently disabled BYOK prompt caching (~5× cost), double-counted cached input triggering spurious compaction, and all-zero usage reporting from providers are not cosmetic bugs; they directly undermine confidence in autonomous long-running sessions. Provider accounting needs to be a single source of truth consumed consistently by budgeting, compaction, and billing UI.

- **Enterprise policy must propagate across surfaces, not just the IDE.** Org-managed default models ignored by the CLI, data-residency endpoints chosen incorrectly, unexplained model downgrades, and CVP re-review loops all show that enterprise verification without consistent cross-surface policy enforcement creates incidents. Tool teams that treat model routing, entitlements, and safeguards as one policy plane will win org rollouts.

- **Windows is now a strategic reliability battleground.** The density of Windows-specific data-loss, PATH, shell-launch, and memory-growth reports across six of seven tools indicates that cross-platform parity is the clearest differentiator available in AI CLI tooling. Developers evaluating tools should weight Windows process-tree termination, path translation, and desktop-environment inheritance heavily in their selection criteria.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — Community Highlights Report

**Data snapshot:** 2026-09-07 · github.com/anthropics/skills  
**Data notes:** PR rows were exported sorted by comment volume (50 total; top 20 shown), though per-PR comment counts appear as `undefined`. Issue comment counts are present. Unless otherwise noted, all PRs are **OPEN**.

---

## 1. Top Skills Ranking

*Ranking reflects the dataset's "sorted by comments" order.*

**#1 — [#1298](https://github.com/anthropics/skills/pull/1298): skill-creator evaluation harness fix (rank #1 by comments)**  
Fix for the `skill-creator` meta-skill: `run_eval.py` reported `recall=0%` for every skill description, making `run_loop.py` and `improve_description.py` "optimize against noise." The PR installs the eval artifact as a real skill and fixes Windows stream reading, trigger detection, and parallel workers. This is the community's most urgent pain point — it directly addresses Issue [#556](https://github.com/anthropics/skills/issues/556) (12 comments, 7 👍), with 10+ independent reproductions. Companion Windows fix PRs [#1099](https://github.com/anthropics/skills/pull/1099) and [#1050](https://github.com/anthropics/skills/pull/1050) also sit in the top 20. **Status:** OPEN, updated 2026-06-23.

**#2 — [#514](https://github.com/anthropics/skills/pull/514): document-typography skill**  
New skill for typographic quality control of generated documents: orphan word wrap, widow paragraphs / stranded section headers at page bottoms, and numbering misalignment. Discussion centers on a class of quality issues users rarely request but that affect nearly every AI-generated document. **Status:** OPEN, updated 2026-03-13.

**#3 — [#1615](https://github.com/anthropics/skills/pull/1615): scnet-hpc skill**  
New skill for operating SCNet HPC clusters through profile-based SSH and Slurm workflows: connection setup, partition/memory/module guidance, job generation, and cluster discovery. Signals demand for vertical/infrastructure-specific operational skills rather than general-purpose writing/coding skills. **Status:** OPEN, updated 2026-08-24 (active).

**#4 — [#538](https://github.com/anthropics/skills/pull/538): pdf skill case-sensitivity fix**  
Small but highly discussed fix: 8 case-mismatched file references (`REFERENCE.md` → `reference.md`, `FORMS.md` → `forms.md`) break the PDF skill on case-sensitive filesystems. The discussion reflects community pain around cross-platform correctness of bundled skills — and a concern that even trivial, correct fixes linger in review. **Status:** OPEN since 2026-03-06.

**#5 — [#486](https://github.com/anthropics/skills/pull/486): ODT skill**  
New skill for OpenDocument formats: create/fill/read/convert `.odt`/`.ods`, template filling, and ODT→HTML parsing. Trigger coverage includes "OpenDocument," "LibreOffice," and ISO-standard format requests — a clear enterprise document-interop demand. **Status:** OPEN, updated 2026-04-14.

**#6 — [#210](https://github.com/anthropics/skills/pull/210): frontend-design skill revision**  
Substantive rework of the existing `frontend-design` skill for "clarity, actionability, and internal coherence" — the goal being instructions Claude can follow within a single conversation. Discussion is a proxy for the broader question: how directive and granular should a SKILL.md be? **Status:** OPEN, updated 2026-03-07.

**#7 — [#83](https://github.com/anthropics/skills/pull/83): skill-quality-analyzer + skill-security-analyzer (meta-skills)**  
Adds two meta-skills to the example marketplace: a quality analyzer scoring structure/documentation (20%), examples, and resources across five dimensions, plus a security analyzer. This PR anticipated the community's largest issue (#492, see below) — demand for tooling that verifies skills before users trust them. **Status:** OPEN, updated 2026-01-07.

**#8 — [#541](https://github.com/anthropics/skills/pull/541): docx skill — tracked-change `w:id` collision fix**  
Fixes document corruption when DOCX tracked changes collide with existing bookmarks. Root cause is well explained: OOXML shares one `w:id` ID space, and the skill's hardcoded low IDs (1, 2, 3) collide with real bookmarks. Highly visible because corrupted generated DOCX files are a severe, user-facing failure. **Status:** OPEN, updated 2026-04-16.

---

## 2. Community Demand Trends

*The 15 exported issues show these concentrated demand signals:*

**Security / supply-chain trust (strongest demand in the dataset).**  
Issue [#492](https://github.com/anthropics/skills/issues/492) — *"Security: Community skills distributed under anthropic/ namespace enable trust boundary abuse"* — leads every item with **43 comments** and is the highest-signal thread in the entire export. Related: [#1175](https://github.com/anthropics/skills/issues/1175) concerns access-control logic inside SKILL.md files for SharePoint documents. The implied skill direction: **provenance verification, security auditing, and namespace separation for community skills.**

**Skill distribution & governance.**  
Issue [#228](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) asks for org-wide skill sharing inside Claude.ai; [#189](https://github.com/anthropics/skills/issues/189) (9 👍 — the highest 👍 count among issues) reports duplicate skills when installing both `document-skills` and `example-skills`; [#62](https://github.com/anthropics/skills/issues/62) (10 comments) reports users losing all 12 locally created skills. The implied demand: **skill library management — sharing, deduplication, persistence.**

**Reliable skill evaluation tooling.**  
Issue [#556](https://github.com/anthropics/skills/issues/556) (12 comments, 7 👍) documents the 0% trigger-rate bug in `run_eval.py`; [#202](https://github.com/anthropics/skills/issues/202) argues `skill-creator` "reads like developer documentation" and violates best practices; [#1390](https://github.com/anthropics/skills/issues/1390) and [#1362](https://github.com/anthropics/skills/issues/1362) report evaluation and bundling harnesses failing silently. The implied demand: **skills that build skills — with evaluation harnesses that can be trusted.**

**Context-window efficiency.**  
Issue [#1487](https://github.com/anthropics/skills/issues/1487) reports the `claude-api` skill eagerly injecting ~156k tokens in one tool call; [#1329](https://github.com/anthropics/skills/issues/1329) (9 comments) proposes a "compact-memory" skill using symbolic notation for agent state. The implied demand: **compact, low-overhead skill representations.**

**Safety / quality-gate meta-skills.**  
Issues [#412](https://github.com/anthropics/skills/issues/412) (agent-governance skill, closed) and [#1385](https://github.com/anthropics/skills/issues/1385) (three-gate reasoning quality pipeline) show appetite for **audit-and-verification skills that gate AI output before delivery.**

---

## 3. High-Potential Pending Skills

The ranked candidates above (#514, #1615, #486, #210, #83) are all substantive, unmerged skill proposals likely to land once the queue is processed. Several other active, open PRs also look close:

- **[#1628 — Hivemind](https://github.com/anthropics/skills/pull/1628)** — zero-cost multi-agent orchestration: Claude Code plans/reviews while headless `opencode` workers on free models execute mechanical work. Active discussion about context economics. Updated 2026-08-24.
- **[#1627 — Buffer GraphQL API Agent Skill](https://github.com/anthropics/skills/pull/1627)** — portable social-media scheduling/management skill for any agent (Claude, Cursor, Codex, n8n). Most recently touched of the pending set (2026-09-05).
- **[#1367 — self-audit v1.3.0](https://github.com/anthropics/skills/pull/1367)** — mechanical file-verification first, then a four-dimension reasoning audit ordered by damage severity. Aligns with the quality-gate demand from Issue #1385. Updated 2026-07-02.
- **[#723 — testing-patterns skill](https://github.com/anthropics/skills/pull/723)** — comprehensive coverage of testing philosophy (Testing Trophy), unit testing conventions, and React Testing Library. Updated 2026-04-21.
- **[#568 — ServiceNow platform skill](https://github.com/anthropics/skills/pull/568)** — broad enterprise platform assistant covering ITSM, ITOM, ITAM/SAM, SecOps, FSM, SPM, CSDM, and IntegrationHub. Updated 2026-08-12 — the most active long-tail enterprise proposal.

---

## 4. Skills Ecosystem Insight

Across the most active issues and PRs, the community's most concentrated demand is no longer for more content skills but for mastery of the **skill lifecycle itself** — trustworthy distribution and provenance ([#492](https://github.com/anthropics/skills/issues/492)), org-level sharing and deduplication ([#228](https://github.com/anthropics/skills/issues/228), [#189](https://github.com/anthropics/skills/issues/189)), and dependable evaluation/quality-gate tooling ([#556](https://github.com/anthropics/skills/issues/556), [#202](https://github.com/anthropics/skills/issues/202), [#1390](https://github.com/anthropics/skills/issues/1390)) — i.e., the ecosystem is asking for "skills that make skills safe, measurable, and maintainable."

---

## 1. Today's Highlights

No new release shipped in the last 24 hours, but issue activity remained heavy: the two longest-running threads — cyber-safeguard blocks affecting CVP-approved orgs ([#84352](https://github.com/anthropics/claude-code/issues/84352), 197 comments) and multi-minute CLI hangs ([#26224](https://github.com/anthropics/claude-code/issues/26224), 151 👍) — both saw fresh updates. Several high-severity reports also landed today, including a Windows data-loss incident from an MSYS-translated `rm -rf` ([#92593](https://github.com/anthropics/claude-code/issues/92593)). On the PR side, a large batch of plugin-infrastructure fixes (Windows path handling, security hardening) was closed/merged.

## 3. Hot Issues

1. **[CVP-approved Claude.ai organization still receives cyber safeguard blocks in Claude Code (#84352)](https://github.com/anthropics/claude-code/issues/84352)** — 197 comments, 27 👍. An org that previously passed Cyber Verification Program approval is blocked again, and the Verification Portal now shows "Under review" despite the prior approval. The high comment count makes this the top blocker for org-level rollout.

2. **[Claude Code hanging/freezing on prompts for 5–20+ minutes (#26224)](https://github.com/anthropics/claude-code/issues/26224)** — 130 comments, 151 👍. The most-upvoted open bug, with no resolution after roughly seven months. Core CLI responsiveness remains the #1 community pain point.

3. **[Runaway rm -rf: timeout auto-backgrounding kept destructive command running; TaskStop did not kill child process, Windows (#92593)](https://github.com/anthropics/claude-code/issues/92593)** — New today. A subagent ran `rm -rf "\"`; MSYS path translation resolved the backslash to the drive root and recursively deleted `C:\`. Raises urgent concerns about timeout auto-backgrounding and process-tree termination on Windows.

4. **[Feature request: make auto-memory MEMORY.md compaction reminder threshold configurable (#91188)](https://github.com/anthropics/claude-code/issues/91188)** — 28 comments. The hardcoded 200-line/25KB memory threshold triggers too early for some users; the request asks for configurability or independent suppression.

5. **[Custom statusLine command from settings.json not being executed (#13517)](https://github.com/anthropics/claude-code/issues/13517)** — 23 comments, 21 👍. macOS TUI bug: the configured `statusLine` command is silently ignored, breaking customized terminal workflows.

6. **[Auto-memory is inconsistently loaded in git-worktree sessions (#81833)](https://github.com/anthropics/claude-code/issues/81833)** — 19 comments. Same repo, same day: some worktree sessions load the full `MEMORY.md` index, others receive no memory content at all — a reliability problem for worktree-heavy workflows.

7. **[Marketplace update button disabled/unpressable even when version is outdated (#45810)](https://github.com/anthropics/claude-code/issues/45810)** — 17 comments, 8 👍. The plugin marketplace Update button stays greyed out, blocking users from pulling new plugin versions.

8. **[Autocompact thrashing: context refilled to the limit within 3 turns, 3 times in a row (#82131)](https://github.com/anthropics/claude-code/issues/82131)** — 13 comments. Repeated compact→refill loops destroy long-session context efficiency.

9. **[Cowork: new projects lost "Choose a folder" after Chat/Cowork merge (#76694)](https://github.com/anthropics/claude-code/issues/76694)** — 12 comments, 15 👍. The context menu was replaced with a chat-style upload-only knowledge menu, breaking folder-centric project creation.

10. **[Auto-memory instructions direct immediate MEMORY.md pointer edit, but read-before-write gate deterministically rejects it (#78569)](https://github.com/anthropics/claude-code/issues/78569)** — 11 comments. The model is instructed to update memory and then blocked by the tool's own consistency gate — a self-conflicting flow.

## 4. Key PR Progress

1. **[fix(security-guidance): make ** glob patterns match zero-depth paths (#87079)](https://github.com/anthropics/claude-code/pull/87079)** — Open. `fnmatch` delegation means `**/*.ts` requires a literal `/` and silently excludes top-level files from security-patterns rules — a quiet security-coverage gap.

2. **[fix(security-guidance): block symlink escape in extensibility config reads (#68689)](https://github.com/anthropics/claude-code/pull/68689)** — Closes a local file disclosure vector where a malicious repo symlinks `.claude/claude-security-guidance.md` to files like `~/.ssh/id_rsa`.

3. **[fix(plugin-dev): avoid shell injection in test-hook.sh via stdin redirection (#68786)](https://github.com/anthropics/claude-code/pull/68786)** — Fixes a quoting hole where `$TEST_INPUT` embedded in a `bash -c` string could lead to arbitrary command execution.

4. **[fix(hookify): add Python wrapper and normalize plugin root paths on Windows (#68699)](https://github.com/anthropics/claude-code/pull/68699)** — Works around backslash-separated `CLAUDE_PLUGIN_ROOT` values and the Microsoft Store `python3` stub that silently exits with code 49 in non-TTY contexts.

5. **[fix(security-guidance): normalize CLAUDE_PLUGIN_ROOT path separators on Windows (#68694)](https://github.com/anthropics/claude-code/pull/68694)** — Converts backslashes in all six hook commands so inline bash referencing `${CLAUDE_PLUGIN_ROOT}` works on Windows.

6. **[fix(security-guidance): strip CRLF from Python version probe on Windows (#68701)](https://github.com/anthropics/claude-code/pull/68701)** — Windows Python emits `\r\n`, which broke the version comparison; the probe now strips CRLF for correct detection.

7. **[feat(bug-reporter): add /bug command to file GitHub issues from the terminal (#68707)](https://github.com/anthropics/claude-code/pull/68707)** — New plugin and slash command that let users file issues on `anthropics/claude-code` without leaving the CLI.

8. **[fix(plugin-dev): hook JSON to stdout, tighten su\* glob, fix CI detection and JSON injection in examples (#68785)](https://github.com/anthropics/claude-code/pull/68785)** — Corrects multiple bugs in hook-development reference examples, including decision JSON going to stderr instead of stdout.

9. **[fix(scripts): add duplicate label additively, don't replace existing labels (#68693)](https://github.com/anthropics/claude-code/pull/68693)** — Prevents GitHub PATCH semantics from wiping platform/area/priority labels when an issue is closed as a duplicate.

10. **[fix(hookify): rename shadowed 'field' variable and fix inline dict comma parsing (#68686)](https://github.com/anthropics/claude-code/pull/68686)** — Fixes two config-loader correctness bugs in hookify.

## 6. Feature Request Trends

- **Memory/context lifecycle control**: Users want configurable auto-memory compaction thresholds ([#91188](https://github.com/anthropics/claude-code/issues/91188)), consistent memory loading across git worktrees ([#81833](https://github.com/anthropics/claude-code/issues/81833)), and resolution of read-before-write conflicts ([#78569](https://github.com/anthropics/claude-code/issues/78569)).
- **UI configurability**: Growing demand for opt-outs and customization — working custom statusLine commands ([#13517](https://github.com/anthropics/claude-code/issues/13517)), a setting to disable automatic VS Code editor group locking ([#80148](https://github.com/anthropics/claude-code/issues/80148)), and activity-based sorting for Cowork project chats ([#87723](https://github.com/anthropics/claude-code/issues/87723)).
- **Desktop/account workflow support**: Multi-account session resumption in Claude Desktop ([#74662](https://github.com/anthropics/claude-code/issues/74662)) and restoring folder-based project creation in Cowork ([#76694](https://github.com/anthropics/claude-code/issues/76694)).
- **Richer rendering**: Terminal graphics protocols for inline images ([#79706](https://github.com/anthropics/claude-code/issues/79706)) and inline image support in the VS Code sidebar/Remote-WSL ([#85520](https://github.com/anthropics/claude-code/issues/85520)).

## 7. Developer Pain Points

- **Stalls and context thrashing remain the top frustration**: The 5–20-minute freeze bug ([#26224](https://github.com/anthropics/claude-code/issues/26224)) is still the most-upvoted open issue, and autocompact thrashing ([#82131](https://github.com/anthropics/claude-code/issues/82131)) degrades long sessions.
- **Windows reliability and safety gaps**: Repeated Windows-specific breakage — torn hook env files permanently wedging the Bash tool ([#78146](https://github.com/anthropics/claude-code/issues/78146)), case-sensitive drive-letter comparison rejecting valid worktrees ([#91618](https://github.com/anthropics/claude-code/issues/91618)), `sandbox.enabled` leaving `$TMPDIR` read-only ([#92590](https://github.com/anthropics/claude-code/issues/92590)), and the destructive `rm -rf`/TaskStop failure ([#92593](https://github.com/anthropics/claude-code/issues/92593)).
- **Trust concerns around unwanted file operations**: Reports of deletion against explicit user instructions ([#92589](https://github.com/anthropics/claude-code/issues/92589)) and git actions taken despite prohibitions ([#77550](https://github.com/anthropics/claude-code/issues/77550)) point to safety-guard gaps.
- **Auto-memory friction**: Users hit self-conflicting behavior where memory updates are instructed and then rejected ([#78569](https://github.com/anthropics/claude-code/issues/78569)), plus unpredictable memory loading depends on session type ([#81833](https://github.com/anthropics/claude-code/issues/81833)).
- **Enterprise/org adoption blockers**: Safeguard regressions affecting CVP-approved orgs ([#84352](https://github.com/anthropics/claude-code/issues/84352)) and an unexplained model downgrade from Claude Fable 5.1 to Claude Opus 4.8 ([#92591](https://github.com/anthropics/claude-code/issues/92591)) complicate production rollouts and trust in model routing.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-09-07

## Today's Highlights

Windows desktop reliability and a VS Code extension regression dominate community attention this week. The 26.901.22334 extension breakage (`chatgpt.openSidebar` not found) generated multiple duplicate reports and was closed as a known regression, while long-running Windows issues around Remote Control enrollment (#28919), stuck PowerShell execution (#17325), and thread-history freezes (#41079) continue to accumulate comments. On the engineering side, a fast-moving batch of merged PRs added MCP user-verification flows, remote permission-profile selection, and a managed-worktree browser to the TUI.

## Hot Issues

1. **[#28919 — Windows Codex app missing "control other devices" tab](https://github.com/openai/codex/issues/28919)** — 63 comments, 59 👍. The most active issue: the Settings > Connections tab for Remote Control simply doesn’t exist on Windows build 26.611.62324, blocking device-control workflows.
2. **[#41079 — Paginated thread history stalls on duplicate ordinal](https://github.com/openai/codex/issues/41079)** — 30 comments. The canonical rollout JSONL contains all later messages, but the desktop UI shows only an older snapshot. This “history-projection stall” is becoming a recognizable failure class.
3. **[#34499 — Cannot create a local Work chat inside a ChatGPT Project](https://github.com/openai/codex/issues/34499)** — 23 comments, 15 👍. Windows Desktop users with ChatGPT Plus can’t create local Work chats scoped to a ChatGPT Project.
4. **[#42663 — Remote SSH: extension fails because VS Code server Node 22 can’t parse `using`](https://github.com/openai/codex/issues/42663)** — 10 comments. The 26.5901.22334 extension won’t activate on Remote-SSH hosts; syntax compatibility of shipped JS is the suspected culprit.
5. **[#40939 — Codex CLI can’t use Luna Reserve after standard limit is exhausted](https://github.com/openai/codex/issues/40939)** — 9 comments. Same account works in the Codex App, but the CLI doesn't see the entitlement — a rate-limit consistency problem with real workflow impact.
6. **[#42882 — VS Code regression: `chatgpt.openSidebar` command not found](https://github.com/openai/codex/issues/42882)** — Closed, 6 comments, 3 👍. One of several duplicate reports (#42969, #43085) confirming 26.901.22334 broke the sidebar entry point after update.
7. **[#42027 — Side-chat fork fails after interrupted turn: duplicate rollout ordinal](https://github.com/openai/codex/issues/42027)** — 7 comments. Same “expected ordinal N+1, got N” projection failure as #41079, but triggered via the fork path — evidence the bug spans multiple UI flows.
8. **[#17325 — VS Code on Windows fails to start PowerShell for shell_command](https://github.com/openai/codex/issues/17325)** — 7 comments, 4 👍. All local shell tool calls fail when PowerShell can’t be spawned; related Windows PATH/pwsh alias issues remain in #18937.
9. **[#43124 — macOS desktop history freezes at older turns](https://github.com/openai/codex/issues/43124)** — 5 comments. “Projection expected ordinal 3185, got 3184; migration says already_paginated.” Fresh report indicating the ordinal bug affects macOS, not just Windows.
10. **[#43344 — GPT-5.5 returns 404 model not found in brand-new conversation](https://github.com/openai/codex/issues/43344)** — 3 comments, 1 👍. Windows Pro user sees a 404 on `backend-api/codex/responses` immediately in a fresh session; new and still under triage.

## Key PR Progress

1. **[#43352 — Add opt-in MCP user-verification transport](https://github.com/openai/codex/pull/43352)** — Adds typed `openai/userVerification` elicitations carrying a title for app responses. Companion pieces: capability gating in #43289 and API contracts in #43265.
2. **[#43340 — Enable remote named permission profile selection in the TUI](https://github.com/openai/codex/pull/43340)** — The permissions picker previously showed remote profiles but disabled them; this wires selection through `thread/settings/update`.
3. **[#43330 — Preserve saved permissions when resuming or forking remote tasks](https://github.com/openai/codex/pull/43330)** — Prevents local permission settings, including named profiles, from overwriting a remote task’s saved server settings.
4. **[#43308 — Replace Windows app-server shutdown files with socket requests](https://github.com/openai/codex/pull/43308)** — Routes managed Windows shutdown through `/daemon/shutdown` on the local control socket, requiring PID acknowledgement before drain — a meaningful Windows stability fix.
5. **[#43286 — Add a managed worktree browser to the TUI](https://github.com/openai/codex/pull/43286)** — New searchable “Browse worktrees” option listing pool checkouts, owner metadata, and resume/copy actions.
6. **[#43298 — Defer managed worktree transitions to fresh TUI loop iterations](https://github.com/openai/codex/pull/43298)** — Splits setup/checkout work out of the synchronous `ChatWidget` constructor to avoid event-loop stalls.
7. **[#43248 — Connect voice-host RTP audio to speaker playback](https://github.com/openai/codex/pull/43248)** — Adds a GStreamer pipeline with jitter buffering so received RTP packets are decoded and played, not just drained.
8. **[#43178 — Allow guarded legacy resume with background migration enabled](https://github.com/openai/codex/pull/43178)** — Restores the cached legacy resume shortcut when a maintenance lock prevents migration during resume.
9. **[#43325 — Sort JSON schema object keys for consistent Cargo and Bazel output](https://github.com/openai/codex/pull/43325)** — Recursively sorts object keys before writing app-server protocol schemas so Cargo and Bazel builds produce identical artifacts.
10. **[#43304 — Isolate Bazel build commit metadata from Rust compilation inputs](https://github.com/openai/codex/pull/43304)** — Stops stamped user/host/timestamp values from poisoning remote cache reuse across developers and CI workers.

## Hot Discussions

### Ideas
1. **[#9618 — How is there not a /rewind or /revert feature?](https://github.com/openai/codex/discussions/9618)** — 119 👍, 20 comments. The highest-signal feature request: Codex needs undoable step rewind comparable to OpenCode and Claude Code; community calls the current commit-everything workflow “almost unusable.”
2. **[#14067 — Sync Codex threads and session context across devices](https://github.com/openai/codex/discussions/14067)** — 61 👍, 10 comments. Work-computer/home-computer users want thread continuity instead of locally tied sessions.
3. **[#7366 — Reference files that are gitignored](https://github.com/openai/codex/discussions/7366)** — 7 👍, 2 comments. `@`-referencing should work for lookup-able files that simply aren’t committed.
4. **[#42703 — Can history retrieval make history recursively self-referential?](https://github.com/openai/codex/discussions/42703)** — 1 👍. Raises a long-horizon failure mode for the new `history` / `notes` / `new_context` approach.

### Q&A
1. **[#40740 — Does rollout tracing capture which path produced a Declined exec status?](https://github.com/openai/codex/discussions/40740)** — Deep-dive into `rollout/src/policy.rs` and `protocol_event.rs`: approval requests are intentionally excluded from persistence.
2. **[#43257 — How does experimental context management count history lookups against usage limits?](https://github.com/openai/codex/discussions/43257)** — Multi-day task users want to know whether cross-context history retrieval consumes allowance.

### Show and tell
1. **[#41157 — CodexFuse 1.2.0](https://github.com/openai/codex/discussions/41157)** — Local Windows dashboard for Codex rate-limit state (used/available, next reset); no API key required.
2. **[#43224 — NULLYARD: public MCP board](https://github.com/openai/codex/discussions/43224)** — Operator-created plain-text MCP board plus a static integration guide and public skill.

## Feature Request Trends

- **Undo / rewind / revert**: #9618 is the single most-upvoted idea, indicating turn-level rollback is the top missing workflow primitive.
- **Cross-device continuity**: Session and thread synchronization (#14067) pairs naturally with complaints about Remote Control enrollment gaps (#28919, #39739) and Business-account failures (#42575).
- **Richer local context references**: Users want to `@`-reference gitignored files (#7366) and expect the desktop app to index personal skills from `~/.agents/skills` (#28505).
- **Remote permission fidelity**: Recent PRs (#43330, #43340) reflect demand for remote tasks to keep their own saved permission profiles rather than inheriting local overrides.
- **Transparent usage accounting**: Multiple rate-limit threads (#40939, #43136, #43341) plus the Q&A on context-management charges (#43257) show users want predictable, explainable allowance consumption.

## Developer Pain Points

- **Windows remains the rough edge**: Missing Remote Control UI (#28919), PowerShell spawn failures (#17325/#18937), Project chat creation gaps (#34499), and Business Remote enrollment failures (#42575) all land on Windows.
- **Thread-history projection bugs cause visible “data loss”**: Duplicate-ordinal failures (#41079, #42027, #43124) freeze the UI at older snapshots even though JSONL is intact — an especially confusing bug class for users.
- **Extension regressions erode trust**: 26.901.22334 removing the sidebar command (#42882/#42969) and Remote-SSH activation failing on Node 22 syntax (#42663) forced multiple rollbacks to 26.825.x.
- **Agentic behavior concerns**: Reports of a task running 4+ hours without verifying its primary objective (#43086) and repeated false-positive safety checks escalating to near-every-turn gating (#43312, #43321) signal that self-regulation and guardrail tuning remain pain points.
- **Rate-limit entitlement inconsistency**: Luna Reserve working in the app but not the CLI (#40939), plus wrong-bucket pinning (#43136), makes Pro/Plus plan behavior feel non-deterministic across surfaces.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-09-07

## Today's Highlights
Agent reliability and security hardening dominate this digest: several long-running issues around subagent hangs, misleading "GOAL success" terminations, and browser-agent failures were actively discussed, while merged PRs focused on removing a silent destructive `git diff --output` path on Windows, bumping the sandbox off EOL Node 20, and fixing CRLF/line-ending bugs that caused whole-file dumps into model context. The team also shipped the usual nightly release cadence.

## Releases
- **[v0.60.0-nightly.20260907.g85aca163f](https://github.com/google-gemini/gemini-cli/releases/tag/v0.60.0-nightly.20260907.g85aca163f)** — Automated nightly release. No manual changelog notes; see the [full changelog](https://github.com/google-gemini/gemini-cli/compare/v0.60.0-nightly.20260906.g85aca163f...v0.60.0-nightly.20260907.g85aca163f) for the diff against yesterday's build.

## Hot Issues
1. **[#22323: Subagent recovery after MAX_TURNS is reported as GOAL success, hiding interruption](https://github.com/google-gemini/gemini-cli/issues/22323)** — P1. A `codebase_investigator` subagent reports `success` + `GOAL` even when it hit `MAX_TURNS` before doing any analysis. 13 comments; this is concerning because it silently masks agent failures as wins.
2. **[#21409: Generalist agent hangs](https://github.com/google-gemini/gemini-cli/issues/21409)** — P1, 8 👍. Simple tasks like folder creation hang forever when delegated to the generalist agent (one user waited an hour); instructing the model not to delegate works around it.
3. **[#25166: Shell command execution gets stuck with "Waiting input" after command completes](https://github.com/google-gemini/gemini-cli/issues/25166)** — P1. Simple, non-interactive CLI commands remain displayed as active and awaiting input long after finishing — a core-level fidelity bug.
4. **[#21983: Browser subagent fails in Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)** — P1. Browser automation terminates immediately (reported as GOAL) on Wayland sessions, blocking browser-agent users on Linux.
5. **[#21968: Gemini does not use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)** — Community members with custom `gradle`/`git` skills report the model almost never invokes them autonomously, even for clearly related tasks.
6. **[#26525: Add deterministic redaction and reduce Auto Memory logging](https://github.com/google-gemini/gemini-cli/issues/26525)** — Security/ privacy: Auto Memory sends transcript content into model context before prompt-based redaction happens, and skill contents may be over-logged.
7. **[#26522: Stop Auto Memory from retrying low-signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)** — Sessions are only marked processed after a successful `read_file`; low-signal sessions that the extractor skips get re-surfaced forever.
8. **[#22232: Enhance browser_agent resilience — automatic session takeover and lock recovery](https://github.com/google-gemini/gemini-cli/issues/22232)** — "Fail-fast" behavior on locked browser profiles makes persistent sessions brittle; needs takeover/orphan-process recovery.
9. **[#19873: Leverage model's bash affinity via Zero-Dependency OS Sandboxing & Post-Execution Intent Routing](https://github.com/google-gemini/gemini-cli/issues/19873)** — Enhancement with 9 comments: Gemini models are native POSIX-tool users; propose safe zero-dep sandboxing so bash-style exploration/editing can be used without compromising security.
10. **[#22745: Assess the impact of AST-aware file reads, search, and mapping](https://github.com/google-gemini/gemini-cli/issues/22745)** — Epic tracking whether AST-aware tools reduce misaligned file reads, token noise, and navigation turns in large codebases.

## Key PR Progress
1. **[#28973: Bump sandbox image from EOL node:20-slim to node:22-slim](https://github.com/google-gemini/gemini-cli/pull/28973)** — P1/security, closed. Node 20 hit EOL 2026-04-30; fixes #28584 for both builder and runtime stages in the repo Dockerfile.
2. **[#29184: Validate git args in Windows sandbox to block silent `git diff --output`](https://github.com/google-gemini/gemini-cli/pull/29184)** — P1/security, open. On Windows all `git status | log | diff | show | branch` run without confirmation; `--output=<path>` could truncate arbitrary files silently.
3. **[#28971: Keep truncated MCP tool names unique](https://github.com/google-gemini/gemini-cli/pull/28971)** — Closed. First-30/last-30 truncation is non-injective; two tools agreeing on both ends collapsed into one registry name.
4. **[#28975: Keep glob results for symlinked workspace roots](https://github.com/google-gemini/gemini-cli/pull/28975)** — Closed, fixes #28416. `glob` returned "No files found" when the workspace root was reached via symlink — the default for projects under macOS `/tmp`.
5. **[#28983: Detect mixed line endings instead of flagging CRLF on a single match](https://github.com/google-gemini/gemini-cli/pull/28983)** — Closed. `detectLineEnding()` classified a mostly-LF file as CRLF if it contained even one `\r\n`.
6. **[#29132 and #29131: Normalize line endings in diff context snippets](https://github.com/google-gemini/gemini-cli/pull/29132)** — Two parallel open PRs (#29132 fixing #29130, plus #29131) that stop CRLF-vs-LF mismatches from dumping full-file diffs back into context.
7. **[#29134: Protect current session from deletion](https://github.com/google-gemini/gemini-cli/pull/29134)** — Open, fixes #29133. Active session ID is passed through `--list-sessions`/`--delete-session`, with a short-ID suffix match to avoid deleting the live session.
8. **[#28972: Guard `formatTruncatedToolOutput` against non-positive maxChars](https://github.com/google-gemini/gemini-cli/pull/28972)** — P1, closed, fixes #28620. Negative budgets produced negative `slice()` indices and silently corrupt output.
9. **[#29209: Skip non-numeric background PID lines](https://github.com/google-gemini/gemini-cli/pull/29209)** — Closed, fixes #29042. Prevents stray warnings/`NaN` from reaching `llmContent`; adds regression coverage for mixed valid PID/known sysmond lines.
10. **[#29106: Flush final SSE event on EOF without trailing blank line](https://github.com/google-gemini/gemini-cli/pull/29106)** — Closed. The SSE parser dropped the final buffered event on truncated/non-conformant streams, silently losing `finishReason` and usage metadata.

## Feature Request Trends
- **AST-aware codebase tooling**: Epics [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) and [#22746](https://github.com/google-gemini/gemini-cli/issues/22746) push for AST-level file reads/search/mapping, plus [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) asking for native file tools for the task tracker — the unifying goal is fewer, more surgical tool calls.
- **Subagents/skills that actually self-start**: [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) and [#21432](https://github.com/google-gemini/gemini-cli/issues/21432) both want the model to autonomously use the right custom skills, subagents, and accurate CLI self-knowledge without explicit prompting.
- **Safer native bash execution**: [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) (zero-dependency OS sandboxing) pairs with [#22672](https://github.com/google-gemini/gemini-cli/issues/22672) (discourage destructive `git reset`/`--force` behavior) to make raw shell power safe by default.
- **Auto Memory governance**: The #26516 cluster ([#26522](https://github.com/google-gemini/gemini-cli/issues/26522), [#26523](https://github.com/google-gemini/gemini-cli/issues/26523), [#26525](https://github.com/google-gemini/gemini-cli/issues/26525)) requests deterministic secret redaction, quarantining invalid memory patches, and ending infinite retry loops.
- **Browser-agent autonomy**: [#22232](https://github.com/google-gemini/gemini-cli/issues/22232) (lock/takeover recovery) and [#22267](https://github.com/google-gemini/gemini-cli/issues/22267) (respect `settings.json` overrides) reflect demand for resilient, configurable browser agents.
- **Subagent observability**: [#22598](https://github.com/google-gemini/gemini-cli/issues/22598) (`/chat share` including subagent trajectories) and [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) (`/bug` including subagent context) show users want visibility into what subagents actually did.

## Developer Pain Points
- **Hangs and stalls**: The generalist agent hanging indefinitely ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), shell commands stuck on "Waiting input" ([#25166](https://github.com/google-gemini/gemini-cli/issues/25166)), and getting stuck at interactive prompts like Vite scaffolding ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465)) erode trust in autonomous mode.
- **Failures disguised as success**: [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) shows a MAX_TURNS interruption being reported as GOAL/Success; [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) notes debug reports don't capture subagent internals — hard to diagnose when things go wrong.
- **Browser agent flakiness**: Wayland failures ([#21983](https://github.com/google-gemini/gemini-cli/issues/21983)), locked-profile fail-fast behavior ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232)), and ignored settings overrides ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)).
- **Context bloat**: Single CRLF matches triggering CRLF-mode rewrites, full-file diffs from line-ending mismatches (#29131/#29132), and models scattering temp scripts across directories ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)) all waste tokens and complicate clean commits.
- **Tool scalability limits**: 400 errors once tool count exceeds ~128 ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)); MCP tool-name truncation collisions (#28971) show the registry straining under plugin growth.
- **Platform papercuts**: Symlinked agent files not recognized ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)), symlinked workspace roots breaking globs (#28975), and `/compress` summaries not persisting across session resume ([#21335](https://github.com/google-gemini/gemini-cli/issues/21335)).

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-09-07

## Today's Highlights

No new Copilot CLI release shipped in the past 24 hours; activity centered on the issue tracker. The most pressing reports concern ACP (agentic) mode: a regression that auto-approves tool calls without client permission ([#4537](https://github.com/github/copilot-cli/issues/4537)), plus lifecycle bugs where `session/prompt` aborts running background sub-agents ([#4555](https://github.com/github/copilot-cli/issues/4555)) and `end_turn` fires before background work completes ([#4743](https://github.com/github/copilot-cli/issues/4743)). On the cost and resource front, BYOK users report silently disabled prompt caching (~5× cost) in 1.0.82 ([#4720](https://github.com/github/copilot-cli/issues/4720)), while WSL2 sessions are reported to consume ~31 GB RSS ([#4694](https://github.com/github/copilot-cli/issues/4694)).

## Releases

No new releases were published in the last 24 hours.

## Hot Issues

1. [**#4537 – ACP mode auto-approves tool calls again (regression of #845)**](https://github.com/github/copilot-cli/issues/4537) — [area:permissions] Since 1.0.81-1, the agent in `--acp` mode no longer sends `session/request_permission`; shell commands, file edits, and deletions execute unattended. This is a security-sensitive regression (2 👍) with direct implications for safe agentic workflows.

2. [**#4695 – MCP OAuth tokens for HTTP servers not reliably reused across sessions**](https://github.com/github/copilot-cli/issues/4695) — [area:authentication, area:mcp] The token cache creates multiple cache-key hashes for the same OAuth server, forcing repeated PKCE re-authentication. Community reports 5 comments of friction in long-running MCP setups.

3. [**#4692 – Default Enterprise model not honored by CLI**](https://github.com/github/copilot-cli/issues/4692) — [area:enterprise, area:models] Organization-managed defaults such as MAI-Code-1.1-Flash are applied correctly in VS Code/Desktop but silently ignored by the CLI, which falls back to another model — a rollout blocker for orgs standardizing on one model (4 comments).

4. [**#4527 – `copilot -p` fails with 401 on GHEC data residency (closed)**](https://github.com/github/copilot-cli/issues/4527) — Non-interactive prompt mode fetches the model catalog from `api.githubcopilot.com` instead of the tenant endpoint, breaking automation on data-residency tenants. Interactive mode worked with the same credentials. Closed after community attention (4 👍, 3 comments).

5. [**#4720 – BYOK silently disables prompt caching (~5× cost)**](https://github.com/github/copilot-cli/issues/4720) — [area:networking, area:models] Copilot CLI 1.0.82 in BYOK mode sends no cache declaration: provider usage shows `cached_tokens=0`, so every turn re-sends the full context at full price. Significant financial impact for extended sessions.

6. [**#4694 – WSL2: ~31 GB RSS and ~57% CPU consumption**](https://github.com/github/copilot-cli/issues/4694) — [area:platform-linux] Running Claude Opus 5 on High Effort in a long session caused ~31 GB RSS at ~47% context usage on WSL2 — an apparent memory-management pathology that makes long sessions impractical.

7. [**#4555 – ACP: `session/prompt` unconditionally aborts the session and background sub-agents**](https://github.com/github/copilot-cli/issues/4555) — [area:sessions, area:agents] `session.abort()` is invoked before handling the prompt request, killing tasks launched via the `task` tool with `mode=background` — inconsistent with interactive TUI behavior.

8. [**#4743 – ACP: `end_turn` precedes background-shell completion; no observable idle signal**](https://github.com/github/copilot-cli/issues/4743) — Companion to #4555: `stopReason: "end_turn"` arrives while a background shell is still running; when it finishes, the agent autonomously calls tools and emits updates after the prompt RPC already completed. Clients lack a reliable session-idle signal.

9. [**#4742 – Desktop app 1.1.15: cannot create a second Local session while one is running**](https://github.com/github/copilot-cli/issues/4742) — [triage] "This project already has an active Local workspace" blocks concurrent branch-type sessions in the same project after auto-upgrade, halting parallel multi-session workflows.

10. [**#4738 – `ask_user` form: pressing Enter early submits/cancels and permanently discards typed input**](https://github.com/github/copilot-cli/issues/4738) — [triage] Filed as high severity: accidental Enter on the elicitation form discards substantial user-authored content with no autosave or recovery path.

## Key PR Progress

Only one pull request was active in the last 24 hours:

- [**#4739 – docs: propose terminal-owned macOS notifications**](https://github.com/github/copilot-cli/pull/4739) — A reference proposal (not a change to the shipped CLI) documenting the macOS notification click-handling problem, with an original MIT-licensed terminal notification example and portable regression tests. Useful for integrators building CLI notification UIs; no review comments yet.

## Hot Discussions

No discussion data was provided for this digest period.

## Feature Request Trends

- **Richer terminal input editing**: Users are requesting GUI/Emacs-style editing behaviors — Shift+Arrow/Ctrl+A text selection ([#2644](https://github.com/github/copilot-cli/issues/2644)) and Ctrl+E to accept inline autocomplete suggestions ([#4736](https://github.com/github/copilot-cli/issues/4736)). The interactive prompt is clearly a primary surface needing parity with modern shell editing.
- **Project/repo-scoped plugins**: The now-closed issue with 18 👍 and 14 comments ([#1665](https://github.com/github/copilot-cli/issues/1665)) reflects sustained demand for per-repository plugin configuration instead of only per-user global plugins.
- **Deterministic ACP lifecycle semantics**: Multiple reports ([#4555](https://github.com/github/copilot-cli/issues/4555), [#4743](https://github.com/github/copilot-cli/issues/4743), [#4537](https://github.com/github/copilot-cli/issues/4537)) effectively request a clearer protocol contract: explicit permission re-requesting and an observable idle/done signal that covers background sub-agents.
- **Enterprise and data-residency alignment**: Reports about ignored org-managed default models ([#4692](https://github.com/github/copilot-cli/issues/4692)) and incorrect endpoint selection on GHEC data residency ([#4527](https://github.com/github/copilot-cli/issues/4527)) show that enterprise policy settings must be applied consistently across interactive, prompt, and ACP modes.

## Developer Pain Points

- **Trust regressions in agentic/ACP mode**: Silent auto-approval of shell commands and file edits ([#4537](https://github.com/github/copilot-cli/issues/4537)) undermines safe agent use; background sub-agents are either killed or outlive their protocol contract ([#4555](https://github.com/github/copilot-cli/issues/4555), [#4743](https://github.com/github/copilot-cli/issues/4743)).
- **Hidden cost and resource spikes**: Loss of prompt caching in BYOK sessions is reported at ~5× cost ([#4720](https://github.com/github/copilot-cli/issues/4720)), while WSL2's multi-GB RSS growth ([#4694](https://github.com/github/copilot-cli/issues/4694)) makes long-running sessions impractical for local development.
- **User-content loss in the interactive UI**: Pressing Enter early in `ask_user` forms permanently discards typed answers ([#4738](https://github.com/github/copilot-cli/issues/4738)); assistant text before tool calls gets folded into "Thought for Ns" and never shown ([#4735](https://github.com/github/copilot-cli/issues/4735)); responses truncated at `max_output_tokens` and their continuation requests are lost ([#4733](https://github.com/github/copilot-cli/issues/4733)).
- **Authentication and enterprise configuration friction**: MCP OAuth tokens are repeatedly re-issued due to cache-key duplication ([#4695](https://github.com/github/copilot-cli/issues/4695)), and organization-managed model defaults are silently ignored in the CLI ([#4692](https://github.com/github/copilot-cli/issues/4692)), forcing manual overrides and repeated re-authentication.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-09-07

**Source:** github.com/anomalyco/opencode

## 1. Today's Highlights

Reliability concerns dominate today's issue activity: Bedrock Luna usage is being double-counted, causing automatic compaction too often ([#47296](https://github.com/anomalyco/opencode/issues/47296)), while Together AI models report all-zero token usage ([#47716](https://github.com/anomalyco/opencode/issues/47716)). Meanwhile, a multi-PR renderer persistence overhaul from Hona — [#47704](https://github.com/anomalyco/opencode/pull/47704), [#47705](https://github.com/anomalyco/opencode/pull/47705), and [#47706](https://github.com/anomalyco/opencode/pull/47706) — was closed, moving large draft data into content-addressed chunks to reduce storage/database pressure. New reports also highlight unbounded DB growth and leaked `serve` instances as serious long-running deployment concerns.

## 2. Releases

No new releases were published in the last 24 hours.

## 3. Hot Issues

- [#1934 — Feat: Automatically run `aws sso login` when credentials need to be refreshed](https://github.com/anomalyco/opencode/issues/1934)  
  *Closed, 8 comments, 14 👍.* A widely requested workflow improvement for teams with short-lived AWS SSO sessions; still the highest-reacted issue in this window.

- [#47296 — Bedrock GPT-5.6: usage total counts cached input twice, so auto-compaction fires after every message](https://github.com/anomalyco/opencode/issues/47296)  
  *Open, 3 comments.* A subtle provider-accounting bug that effectively destroys context continuity for Bedrock Luna users. Fix PR [#47354](https://github.com/anomalyco/opencode/pull/47354) directly addresses it.

- [#31942 — Agent loops 277 times on MCP resource_list without circuit breaker](https://github.com/anomalyco/opencode/issues/31942)  
  *Closed, 3 comments.* A dramatic example of missing agent loop protection; the repeated MCP calls burned the entire workspace token budget.

- [#47729 — opencode.db grows unbounded — event table (4.1GB after ~6 months) has no retention policy or cleanup command](https://github.com/anomalyco/opencode/issues/47729)  
  *Open, 1 comment.* A serious operational concern for long-lived installs; the `event` table dominates database size with no cleanup path.

- [#47727 — serve: per-request instances are never disposed — MCP child processes accumulate until memory exhaustion](https://github.com/anomalyco/opencode/issues/47727)  
  *Open, 1 comment.* Important for users running `opencode serve` behind a polling client: instances and MCP processes leak per request.

- [#47716 — Together AI: token usage always 0 — bundled @ai-sdk/togetherai doesn't send stream_options.include_usage](https://github.com/anomalyco/opencode/issues/47716)  
  *Open, 2 comments.* Makes token/cost tracking impossible for Together AI models, including GLM and Kimi variants.

- [#47710 — opencode-desktop: Desktop GUI does not pass user shell PATH to plugin subprocesses](https://github.com/anomalyco/opencode/issues/47710)  
  *Closed, 2 comments.* Breaks plugins that rely on external CLI binaries when OpenCode is launched from the macOS desktop app.

- [#47721 — [2.0] core: request accumulates 52 images, exceeds provider max of 50](https://github.com/anomalyco/opencode/issues/47721)  
  *Open, 1 comment.* A v2 beta issue showing that attached image assets are not pruned as sessions grow, causing provider request failures.

- [#37007 — Cannot interrupt/stop long-running commands executed via the bash tool](https://github.com/anomalyco/opencode/issues/37007)  
  *Open, 2 comments, 1 👍.* Long-running foreground commands leave the TUI blocking until timeout; related to [#41753](https://github.com/anomalyco/opencode/issues/41753), where new prompts wait for running bash tools.

- [#47714 — [2.0] cli: update prompt recommends older version](https://github.com/anomalyco/opencode/issues/47714)  
  *Open, 1 comment.* Small but confusing beta-user UX bug: `opencode2` v0.0.0-beta-19215 is told to update to an older build.

## 4. Key PR Progress

- [#47354 — fix(opencode): correct Bedrock Luna token usage](https://github.com/anomalyco/opencode/pull/47354)  
  Closes [#47296](https://github.com/anomalyco/opencode/issues/47296) by avoiding double-counting cached input tokens already included by Bedrock.

- [#47704 — perf(app): cache storage namespaces and batch writes in the renderer](https://github.com/anomalyco/opencode/pull/47704)  
  First of three persistence-layer optimizations modeled after VS Code's storage architecture: one bulk load per namespace and one bulk write per flush window.

- [#47705 — perf(app): serialize persisted stores on a schedule instead of per setter call](https://github.com/anomalyco/opencode/pull/47705)  
  Second layer: replaces synchronous per-setter serialization with scheduled saves, owner-cleanup flushes, and page-hide flushing.

- [#47706 — perf(app): externalize large draft text into content-addressed chunks](https://github.com/anomalyco/opencode/pull/47706)  
  Third layer: large drafts become fixed-size content-addressed chunks so typing after a big paste does not re-upload the entire draft every save.

- [#47724 — fix(desktop): surface default path opening errors](https://github.com/anomalyco/opencode/pull/47724)  
  Closes [#47722](https://github.com/anomalyco/opencode/issues/47722). Rejects non-empty errors from Electron `shell.openPath()` so failed file-manager launches are surfaced.

- [#47720 — fix(cli): resolve plugins in Node SEA builds](https://github.com/anomalyco/opencode/pull/47720)  
  Fixes plugin resolution for Node single-executable-app builds by obtaining the native ESM resolver through the existing VM-backed importer.

- [#47719 — feat(plugin): register template commands](https://github.com/anomalyco/opencode/pull/47719)  
  Lets Promise and Effect plugins register declarative commands using `template`, `agent`, `model`, and `subagent` configuration.

- [#47663 — feat(plugin): add session title hook and request options bag](https://github.com/anomalyco/opencode/pull/47663)  
  First step toward splitting session request hooks by LLM request type; introduces a shared `SessionRequestOptions` shape.

- [#46530 — feat(plugin): expose permission assertions](https://github.com/anomalyco/opencode/pull/46530)  
  Adds `ctx.permission.assert()` for plugins and checks canonical browser URLs, server-file reads, and external-directory access before risky operations.

- [#46534 — feat(core): add firecrawl developer search provider](https://github.com/anomalyco/opencode/pull/46534)  
  Follow-up to the original Firecrawl web search provider; adds a second provider targeting developer-oriented search categories.

## 5. Hot Discussions

No discussion data was provided for this digest period.

## 6. Feature Request Trends

Across the issue data, the most requested feature directions are:

- **Session lifecycle management and data retention**  
  Users want bounded storage, automatic cleanup, and record migration: [#47729](https://github.com/anomalyco/opencode/issues/47729), [#34875](https://github.com/anomalyco/opencode/issues/34875), [#47713](https://github.com/anomalyco/opencode/issues/47713).

- **Cloud credential automation**  
  Strong demand for automatic AWS SSO login refresh rather than manual failure recovery: [#1934](https://github.com/anomalyco/opencode/issues/1934).

- **Better Linux TUI clipboard support**  
  Repeated requests to bundle or auto-detect `xclip`, `xsel`, or `wl-clipboard` so copy/paste works out of the box: [#35977](https://github.com/anomalyco/opencode/issues/35977), [#35978](https://github.com/anomalyco/opencode/issues/35978).

- **Agent/tooling guardrails**  
  Requested protections include circuit breakers for repeated MCP tool calls and safeguards against AI-driven config file corruption: [#31942](https://github.com/anomalyco/opencode/issues/31942), [#35954](https://github.com/anomalyco/opencode/issues/35954).

- **Desktop/CLI parity**  
  Users expect the desktop app to inherit the same shell environment as the CLI — including `PATH` — and want configurable project names independent of folder names: [#47710](https://github.com/anomalyco/opencode/issues/47710), [#47708](https://github.com/anomalyco/opencode/issues/47708).

## 7. Developer Pain Points

- **Provider usage accounting bugs are disrupting trust in context/cost tracking.** Bedrock Luna triggers spurious auto-compaction ([#47296](https://github.com/anomalyco/opencode/issues/47296)), while Together AI reports zero usage ([#47716](https://github.com/anomalyco/opencode/issues/47716)).

- **Unbounded growth is a recurring production risk.** The `opencode.db` event table can reach multi-GB sizes ([#47729](https://github.com/anomalyco/opencode/issues/47729)), and `opencode serve` can leak instances/MCP processes until memory exhaustion ([#47727](https://github.com/anomalyco/opencode/issues/47727)).

- **Interruptibility is insufficient.** Developers cannot reliably stop long-running bash tool commands, and new prompts are queued behind them ([#37007](https://github.com/anomalyco/opencode/issues/37007), [#41753](https://github.com/anomalyco/opencode/issues/41753)).

- **Desktop/plugin environment mismatches cause confusing breakage.** The macOS desktop app not forwarding shell `PATH` to plugin subprocesses is a notable example ([#47710](https://github.com/anomalyco/opencode/issues/47710)).

- **Agent loops and tool misuse need stronger built-in protections.** Without circuit breakers, agents can burn token budgets on repeated MCP calls ([#31942](https://github.com/anomalyco/opencode/issues/31942)); AI edits to `opencode.json` can also corrupt configuration ([#35954](https://github.com/anomalyco/opencode/issues/35954)).

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-09-07

## Today's Highlights
No release shipped in the last 24 hours, but issue and PR activity was heavy. The most visible fixes involve routing GitHub Copilot GPT models (including GPT‑6 Astra) through the Responses API ([#9253](https://github.com/earendil-works/pi/pull/9253)) and pinning undici's DNS lookup to the system resolver for MagicDNS/Tailscale-style hosts ([#9252](https://github.com/earendil-works/pi/pull/9252)). The community's two highest-energy threads remain openai-codex's `Working...` hang ([#4945](https://github.com/earendil-works/pi/issues/4945), 76 comments) and the Windows experience sink thread ([#7547](https://github.com/earendil-works/pi/issues/7547), 57 comments).

## Hot Issues
- [#4945 — openai-codex Connection Reliability Issues](https://github.com/earendil-works/pi/issues/4945) *(open, in progress)* — 76 comments, 32 👍. The most-reacted issue this cycle: `openai-codex`/`gpt-5.5` intermittently leaves the TUI stuck on `Working...` with no stream, tool call, or error. The only recovery is Escape, which records an aborted turn. Marked in progress, but clearly painful for daily users.
- [#7547 — How do you use Pi on Windows? What issues are you seeing?](https://github.com/earendil-works/pi/issues/7547) *(open)* — 57 comments. A maintainer-run coordination thread asking Windows developers to report run modes and breakages, explicitly to decide where core effort should go versus extension-owned workarounds. The reply volume confirms Windows is the platform cluster with the most unresolved friction.
- [#9052 — Fullscreen mode's fixed input box is great, but wheel scrolling is 3× slower](https://github.com/earendil-works/pi/issues/9052) *(open)* — 6 comments, 3 👍. Users adopt fullscreen mode for the pinned input box, then hit a noticeable scroll-performance regression. Small behavior gap, but it blocks a popular UI mode.
- [#8826 — Cap agent retry backoff for prolonged transient outages](https://github.com/earendil-works/pi/issues/8826) *(open)* — 4 comments. During sustained upstream outages, exponential retry runs produce long waits and repeated `503 Too many open files` errors. Requests a configurable cap so retries settle at a bounded interval.
- [#9209 — GPT‑6 Astra routed to unsupported Chat Completions endpoint](https://github.com/earendil-works/pi/issues/9209) *(closed)* — 4 comments. Copilot correctly rejects `gpt-6-astra` on `/chat/completions` with `unsupported_api_for_model`. Fixed by PR [#9253](https://github.com/earendil-works/pi/pull/9253).
- [#9229 — Windows: shell_path config is ignored, always prefer WSL bash](https://github.com/earendil-works/pi/issues/9229) *(closed)* — 4 comments. Even with the WSL Windows feature disabled, Pi prefers WSL bash over an explicit `shell_path` in `settings.json` — a surprising priority bug for Windows users running native shells.
- [#8823 — Esc during active streaming often fails to cancel the in-flight request](https://github.com/earendil-works/pi/issues/8823) *(open)* — 3 comments. The abort is registered but the HTTP request keeps running until the provider finishes naturally, so users wait anyway or end up with an aborted turn they didn't want.
- [#9165 — Claude Opus 5 via OpenRouter rejects per-message output_config](https://github.com/earendil-works/pi/issues/9165) *(closed)* — 3 comments. `openrouter/anthropic/claude-opus-5` returns a 400 for per-message `output_config`, while the same model works on the native Anthropic provider. Points to provider-specific request sanitization needs.
- [#9265 — O(n²) tool-call argument re-parsing in openai-completions streaming](https://github.com/earendil-works/pi/issues/9265) *(closed)* — 1 comment, but severe: the stream handler re-parses the entire accumulated tool-call JSON on every delta. In a single-threaded embedded daemon hosting many sessions, this can freeze the event loop.
- [#9246 — Spend the unused 4th Anthropic cache breakpoint on a stable conversation checkpoint](https://github.com/earendil-works/pi/issues/9246) *(closed)* — 3 comments. Anthropic allows four cache breakpoints; Pi currently uses only three (system prompt, last tool, last user). The proposal adds a stable fourth checkpoint to improve cache-hit economics.

## Key PR Progress
- [#9253 — fix(ai): route Copilot GPT models through Responses (fixes astra)](https://github.com/earendil-works/pi/pull/9253) — Fixes [#9209](https://github.com/earendil-works/pi/issues/9209) and removes obsolete assumptions about Copilot's GPT‑4 catalog; designed to stay correct as GitHub drops older models.
- [#9261 — feat(ai): add sendStrictToolField compat flag](https://github.com/earendil-works/pi/pull/9261) — Decouples strict-shaped tool schemas from the `strict` field itself, unblocking Anthropic-compatible gateways (e.g. AWS Bedrock proxies) that require one but reject the other.
- [#9259 — feat(coding-agent): apply a steering message promptly by interrupting the running turn](https://github.com/earendil-works/pi/pull/9259) — Lets users redirect the agent mid-tool-call ("switch to a faster mirror", "use a different approach") instead of queuing the message until a long install/build finishes.
- [#9251 — feat(coding-agent): hop to a fallback provider on transport errors](https://github.com/earendil-works/pi/pull/9251) — Adds opt-in cross-provider fallback when the active provider is unreachable (transport/timeout/DNS), addressing [#9242](https://github.com/earendil-works/pi/issues/9242). Earlier iterations: [#9248](https://github.com/earendil-works/pi/pull/9248), [#9249](https://github.com/earendil-works/pi/pull/9249).
- [#9252 — fix(coding-agent): pin undici connect lookup to system dns.lookup](https://github.com/earendil-works/pi/pull/9252) — Fixes `ENOTFOUND` for MagicDNS/split-horizon hosts by honoring the OS resolver / nsswitch. Duplicate of [#9250](https://github.com/earendil-works/pi/pull/9250).
- [#9233 — fix(coding-agent): resolve model auth live instead of from startup snapshot](https://github.com/earendil-works/pi/pull/9233) — Fixes a startup race where an unawaited background refresh leaves `hasConfiguredAuth()` empty and blocks model resolution.
- [#6881 — feat(ai): use provider-reported cost when responses include it](https://github.com/earendil-works/pi/pull/6881) *(in progress, open since July)* — Reads `usage.cost` / `cost_details.upstream_inference_cost` when present, falling back to `calculateCost` unchanged. Useful for BYOK/upstream cost accuracy.
- [#9080 — feat(tui): add jump-to-latest control](https://github.com/earendil-works/pi/pull/9080) — Adds a jump-to-latest/new-message control for long streaming transcripts, so users don't have to scroll manually to follow the agent.
- [#9227 — feat(coding-agent): add per-call tool confirmation extension](https://github.com/earendil-works/pi/pull/9227) — Enables extension-driven confirmation before individual tool calls, a natural fit for safety-conscious automation layers.
- [#9137 — feat(coding-agent): add Nix flake](https://github.com/earendil-works/pi/pull/9137) *(WIP, open)* — Adds a Nix flake for reproducible development environments; currently marked as work in progress.

## Hot Discussions
### Ideas
- [#9146 — Provide a per-repo override for API Key and ignore auth.json](https://github.com/earendil-works/pi/discussions/9146) — 2 comments, 1 👍. The author stores an OpenRouter key in 1Password and wants a per-repository API-key override rather than a single global `auth.json` entry. Useful for multi-project setups with different provider accounts.

## Feature Request Trends
- **Resilience-first agent runtime**: cross-provider fallback on transport errors ([#9241/#9242](https://github.com/earendil-works/pi/issues/9242), PR [#9251](https://github.com/earendil-works/pi/pull/9251)), bounded retry backoff during outages ([#8826](https://github.com/earendil-works/pi/issues/8826)), live auth resolution ([#9233](https://github.com/earendil-works/pi/pull/9233)), and provider-reported cost ([#6881](https://github.com/earendil-works/pi/pull/6881)) all point toward making the agent loop survive upstream flakiness without user babysitting.
- **Mid-flight user control**: interrupting a running turn with steering messages ([#9260](https://github.com/earendil-works/pi/issues/9260), PR [#9259](https://github.com/earendil-works/pi/pull/9259)), reliable Escape cancellation ([#8823](https://github.com/earendil-works/pi/issues/8823)), and jump-to-latest TUI navigation ([#9080](https://github.com/earendil-works/pi/pull/9080)).
- **Windows as a first-class platform**: the 57-comment Windows sink thread ([#7547](https://github.com/earendil-works/pi/issues/7547)) plus shell-path honoring ([#9229](https://github.com/earendil-works/pi/issues/9229)), CRLF normalization ([#9264](https://github.com/earendil-works/pi/issues/9264)), backslash-aware `find` globs ([#9262](https://github.com/earendil-works/pi/issues/9262)), and Shift+Enter handling ([#7175](https://github.com/earendil-works/pi/issues/7175)).
- **Wider model/gateway coverage**: GPT‑6 Astra support ([#9133](https://github.com/earendil-works/pi/issues/9133), [#9209](https://github.com/earendil-works/pi/issues/9209)), OpenRouter/Anthropic compatibility ([#9165](https://github.com/earendil-works/pi/issues/9165)), OpenCode Go's new `x-opencode-session` header ([#9230](https://github.com/earendil-works/pi/issues/9230), [#9237](https://github.com/earendil-works/pi/issues/9237)), strict-schema gateway compat flags ([#9263](https://github.com/earendil-works/pi/issues/9263), PR [#9261](https://github.com/earendil-works/pi/pull/9261)), and new LLM Gateway providers ([#7610](https://github.com/earendil-works/pi/pull/7610)).
- **Extension API growth**: acknowledged/idempotent user-turn delivery ([#9236](https://github.com/earendil-works/pi/issues/9236)), runtime TUI mode switching and layout roots ([#9238](https://github.com/earendil-works/pi/issues/9238)), and per-call tool confirmation hooks (PR [#9227](https://github.com/earendil-works/pi/pull/9227)).
- **Prompt-cache and cost tuning**: using Anthropic's fourth cache breakpoint ([#9246](https://github.com/earendil-works/pi/issues/9246)) and avoiding `before_agent_start` flapping that breaks prompt caching on session wake ([#8712](https://github.com/earendil-works/pi/issues/8712)).

## Developer Pain Points
- **openai-codex / gpt-5.5 hangs** are the single loudest complaint: the TUI gets stuck on `Working...` with no output and no error, forcing an Escape that discards the turn ([#4945](https://github.com/earendil-works/pi/issues/4945), 32 👍).
- **Windows friction is broad and recurring**: too many supported run modes ([#7547](https://github.com/earendil-works/pi/issues/7547)), WSL preferred over `shell_path` even when disabled ([#9229](https://github.com/earendil-works/pi/issues/9229)), CRLF leaking `\r` into tool text ([#9264](https://github.com/earendil-works/pi/issues/9264)), and `find` silently returning nothing for backslash globs ([#9262](https://github.com/earendil-works/pi/issues/9262)).
- **Cancellation is not trustworthy**: Escape during streaming often doesn't abort the HTTP request until the provider finishes ([#8823](https://github.com/earendil-works/pi/issues/8823)).
- **TUI rendering regressions**: fullscreen wheel scrolling is 3× slower than regular mode ([#9052](https://github.com/earendil-works/pi/issues/9052)), edits above the viewport trigger destructive full redraws ([#9240](https://github.com/earendil-works/pi/issues/9240)), and resumed sessions re-render saved screenshots at full inline size ([#9256](https://github.com/earendil-works/pi/issues/9256)).
- **Gateway/model whack-a-mole**: Copilot rejecting `gpt-6-astra` on Chat Completions ([#9209](https://github.com/earendil-works/pi/issues/9209)), OpenRouter rejecting per-message `output_config` for Claude Opus 5 ([#9165](https://github.com/earendil-works/pi/issues/9165)), and OpenCode Go now requiring an `x-opencode-session` header ([#9230](https://github.com/earendil-works/pi/issues/9230), [#9237](https://github.com/earendil-works/pi/issues/9237)).
- **DNS and environment pitfalls** outside typical cloud setups: undici fails to resolve MagicDNS/Tailscale hosts ([#9244](https://github.com/earendil-works/pi/issues/9244)), and `models.json` does not resolve `$ENV` placeholders in `apiKey`, sending the literal string and causing 401s ([#9258](https://github.com/earendil-works/pi/issues/9258)).
- **Performance under embedded load**: the openai-completions stream handler re-parses accumulated tool-call JSON on every delta, causing O(n²) cost and event-loop freezes in single-threaded daemons ([#9265](https://github.com/earendil-works/pi/issues/9265)).

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-09-07

## Today's Highlights

The day’s releases focus on WebShell workflow visibility: both a preview and a nightly release carry the new ability to visualize and manage dynamic workflow runs. On the stability side, a newly reported git branch rollback bug (#11253) can discard commits made by a failing `post-checkout` hook, and a matching fix (#11258) is already open. Meanwhile, ongoing community work emphasizes budget transparency, WebShell observability, and safer concurrent daemon/session handling.

## Releases

- [v0.23.1-preview.1](https://github.com/QwenLM/qwen-code/releases/tag/v0.23.1-preview.1)
- [v0.23.0-nightly.20260906.92a8a8d179](https://github.com/QwenLM/qwen-code/releases/tag/v0.23.0-nightly.20260906.92a8a8d179)

Both releases include the same two changes:

- **feat(web-shell): visualize and manage dynamic workflow runs** — [#10594](https://github.com/QwenLM/qwen-code/pull/10594)
- **perf(web-shell): derive the session workflow project**

No breaking changes were flagged in the release notes.

## Hot Issues

Only 6 issues were updated in the 24-hour window. All are included below.

- [#3361](https://github.com/QwenLM/qwen-code/issues/3361) — **[OPEN]** Agent misinterprets shell output as empty despite successful execution (OpenAI-compatible API)  
  The agent can run commands such as `git rev-parse --show-toplevel`, see visible output in the UI, yet still conclude the output is empty. That leads to incorrect assumptions about tool state. Originally created in April and still with `status/needs-triage`, the 6 comments show this remains an unresolved integration pain point.

- [#7167](https://github.com/QwenLM/qwen-code/issues/7167) — **[OPEN]** Fleet Shepherd Dashboard  
  An auto-maintained CI/bot-fleet dashboard tracking PR head states, syncs, dispatches, releases, and cleanups. This is mostly informational for maintainers, but it helps detect stale or stuck automation flows. Currently in `status/need-information`.

- [#11253](https://github.com/QwenLM/qwen-code/issues/11253) — **[OPEN]** Branch creation rollback can discard commits made by a failing post-checkout hook  
  High-impact git safety bug: when `gitCreateBranch` rolls back a failed checkout operation, it can throw away commits made by a repository’s `post-checkout` hook. The matching fix is already in progress via [#11258](https://github.com/QwenLM/qwen-code/pull/11258). Labeled P2 with 2 comments.

- [#11243](https://github.com/QwenLM/qwen-code/issues/11243) — **[OPEN]** feat(daemon): expose update status in WebShell and auto-follow nightly releases  
  Users want long-running CLI/daemon processes to expose update state and update actions inside WebShell, plus an opt-in path for automatically following nightly releases. This is a direct request for better background-automation lifecycle management.

- [#4114](https://github.com/QwenLM/qwen-code/issues/4114) — **[OPEN]** 根据返回为空 (“returns empty”)  
  Another empty-output report, this time around multi-file `read_file` exploration. The agent appears to return empty results when asked to summarize problems after reading project files. Like [#3361](https://github.com/QwenLM/qwen-code/issues/3361), it points to lingering tool-output interpretation gaps.

- [#11249](https://github.com/QwenLM/qwen-code/issues/11249) — **[OPEN]** Main CI failed: Qwen Code CI on 421393d51df6  
  A `main`-branch CI run failed before any test result was reported, in the `Test (ubuntu-latest, Node 22.x)` job. The bot is tracking the failure per commit and has already marked it `ready-for-agent` with `autofix/in-progress`.

## Key PR Progress

- [#11258](https://github.com/QwenLM/qwen-code/pull/11258) — **fix(core): preserve branch commits when checkout hooks fail**  
  Directly addresses the data-loss scenario in [#11253](https://github.com/QwenLM/qwen-code/issues/11253). The rollback disables hooks while restoring the original branch, compares the new branch ref against the expected starting tip, and only deletes when safe.

- [#11144](https://github.com/QwenLM/qwen-code/pull/11144) — **fix(cli): barrier live transcript reads behind tool-result writes**  
  Prevents concurrent session loads from observing half-written tool turns in the ACP agent path. Cleanup now goes through the shared `finalizeDanglingForRestore` helper, improving transcript consistency.

- [#10237](https://github.com/QwenLM/qwen-code/pull/10237) — **fix(core): prevent duplicate task owner dispatch**  
  Stops a leader from dispatching one in-progress task to multiple teammates. Ownership checks now happen while holding the task lock, reducing race conditions in multi-agent collaboration.

- [#10906](https://github.com/QwenLM/qwen-code/pull/10906) — **feat(web-shell): show shell and monitor task output**  
  Makes shell and monitor stdout/stderr directly readable in the WebShell task detail panel. The daemon also gains a sanitized live-session-owner-scoped output tail endpoint.

- [#11254](https://github.com/QwenLM/qwen-code/pull/11254) — **feat(web-shell): show what a Goal has spent against the window it is allowed**  
  Adds token-budget display to the WebShell status strip and Goals dialog, e.g. `1.2k / 30.0M tokens`. This gives users much clearer visibility into autonomous Goal spending.

- [#11257](https://github.com/QwenLM/qwen-code/pull/11257) — **feat(goal): carry budget figures and progress guidance in the continuation prompt**  
  Goal-driven turns now begin with remaining budget, turns elapsed, and self-check guidance. This is the prompt-side companion to [#11254](https://github.com/QwenLM/qwen-code/pull/11254) and helps keep autonomous agents within their allowed limits.

- [#11207](https://github.com/QwenLM/qwen-code/pull/11207) — **feat(serve): allow concurrent standalone daemons with session fencing**  
  Lets updated daemons share Conversations and run different standalone sessions concurrently, while keeping the mandatory single-writer lease per loaded session. Important for multi-daemon environments.

- [#11090](https://github.com/QwenLM/qwen-code/pull/11090) — **feat(ipc): let a user-minted controller token drive a session without per-message review**  
  Closed in this window. Refines the IPC review gate so trusted controller sessions can operate without per-message approval, while still blocking sessions with no review class — a meaningful security/usability trade-off improvement.

- [#10999](https://github.com/QwenLM/qwen-code/pull/10999) — **feat(core): configure model reasoning capabilities**  
  Adds declarative reasoning support to provider model definitions and threads it through ACP, session restoration, workspace previews, TUI effort controls, and the final OpenAI-compatible request. Enables the native `deepseek-v4-pro` reasoning entry.

- [#10347](https://github.com/QwenLM/qwen-code/pull/10347) — **feat(core): auto-retry transient network errors (EOF) where Ctrl+Y is unavailable**  
  Treats wrapped low-level network failures such as `400 network error ... EOF` as retryable transport errors instead of fail-fast client errors. Particularly useful for non-interactive automation contexts.

## Feature Request Trends

- **WebShell as a control plane**  
  Multiple PRs and issues push WebShell beyond chat into full session/task management: session overview improvements, split-view navigation, shell/monitor output visibility, artifact icons, context usage panels, and update-state controls. Issue [#11243](https://github.com/QwenLM/qwen-code/issues/11243) is the clearest explicit request: expose daemon update state and allow automatic nightly-follow behavior from WebShell.

- **Budget and context transparency for autonomous Goals**  
  There is a clear push toward making Goal execution financially and contextually observable. PRs [#11254](https://github.com/QwenLM/qwen-code/pull/11254) and [#11257](https://github.com/QwenLM/qwen-code/pull/11257) add visible token budgets and prompt-level budget guidance so long-running agents self-check their progress.

- **Concurrency, ownership, and rollback safety**  
  Several contributions focus on correctness under concurrent execution: preventing duplicate multi-agent task dispatch, fencing session writes across daemons, binding mesh transcripts correctly, and preserving git commits during failed rollback. This suggests the ecosystem is pushing toward more parallel and autonomous workloads where race conditions become dangerous.

- **Resilience against transient infrastructure failures**  
  Network EOF auto-retry, CI retries for flaky E2E shards, and better handling of hook-created git commits all reflect a broader desire to reduce interruptions caused by environment-level failures rather than code-level bugs.

## Developer Pain Points

- **Tool outputs are sometimes interpreted as empty**  
  Both [#3361](https://github.com/QwenLM/qwen-code/issues/3361) and [#4114](https://github.com/QwenLM/qwen-code/issues/4114) describe cases where commands or file reads visibly return data, but the agent treats the result as empty. This is particularly damaging for shell-driven workflows and has remained open for months.

- **Git operation rollback can lose work**  
  [#11253](https://github.com/QwenLM/qwen-code/issues/11253) highlights a dangerous edge case where branch creation rollback after a failing `post-checkout` hook can discard commits. For developers using git hooks, this creates a “silent data loss” risk that is hard to detect.

- **CI instability consumes maintainer attention**  
  [#11249](https://github.com/QwenLM/qwen-code/issues/11249) shows a main-branch CI failure that occurred before any test result was reported. Paired with the ongoing macOS E2E shard retry work and Prettier-gate formatting fixes, flaky and infra-level CI failures remain a recurring source of friction.

- **Long-running daemon/task observability is still incomplete**  
  Users want better ways to inspect what background agents, daemons, and Goals are doing: how much budget they have spent, whether they are waiting on review, and whether a newer nightly build is available. Until that state is fully exposed in WebShell, users must rely on logs or external processes.

- **Multi-agent and session state races are a real concern**  
  The number of PRs addressing duplicate task dispatch, transcript read/write ordering, and session fencing indicates that concurrency bugs are beginning to surface as multi-agent and daemon-based usage grows. Developers are pushing for stricter ownership and linearizable session state.

</details>