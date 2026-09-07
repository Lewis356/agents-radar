# AI Infrastructure Digest 2026-09-07

> Generated: 2026-09-07 04:41 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross‑Project AI Infrastructure Comparison — 2026‑09‑07

## 1. Ecosystem Overview

All six projects spent the day on the same class of problem: the newest open‑weight frontier — Qwen3.8‑Flash‑Next, DeepSeek‑V4, GLM‑5.3‑Flash, Kimi K3, MiniMax‑M3 — is built on sparse‑MLA / GDN / hybrid linear‑attention architectures whose kernels, FP8/NVFP4 quant paths, and MTP speculative decoders are not yet reliable at scale. The llama.cpp stack (GGUF + llama-server) is consolidating as the substrate for local/edge serving: both Ollama and Unsloth Studio effectively ship on top of it, while vLLM and SGLang race on datacenter‑grade support for the same architecture wave. The shared pain is now correctness, not raw throughput: temperature‑0 non‑determinism, KV‑cache reuse failures, silent tool‑call corruption, and spec‑decode divergence dominate the issue trackers. Meanwhile, control‑plane and lifecycle concerns — billing accuracy, signed images, credential scoping, config semantics — are becoming first‑class engineering work at the gateway and fine‑tuning layers.

## 2. Activity Comparison

Counts below are the trackable issue/PR identifiers explicitly referenced in each project's digest, not full repository totals.

| Project | Issues referenced | PRs referenced | Releases in last 24h | Release/activity pulse |
|---|---|---|---|---|
| vLLM | ~25 | 14 (≈11 opened today) | None | No release; heavy triage on sparse/hybrid determinism, SM120 FP8 illegal‑memory‑access, GLM‑5.3‑Flash fixes |
| SGLang | 21 | 19 | None | No release; "Config Round 6" refactor (6 PRs), FlexKV/DeepSeek‑V4 alignment, CI recovery (958 tests fixed) |
| llama.cpp | 15 | 26 | 9 builds (b10821–b10830) | Highest cadence: model support merged (Spark2.5), GDN numerics fix, aggressive Vulkan/CUDA kernel work |
| Ollama | 11 | 4 | None | Low PR churn; digest dominated by stability reports (MLX, Vulkan, Blackwell, cloud models) |
| LiteLLM | 12 | 10 | **v1.100.0** | One release today — cosign‑signed Docker images mandatory; billing correctness + Rust route‑execution migration |
| Unsloth | 12 | 16 | None | No release; Studio serving features (KV preemption, two‑Spark topology) + security/ops cleanup |

**Read-through.** llama.cpp is shipping changes multiple times per day, which matters because Ollama and Unsloth Studio inherit its behavior downstream. vLLM and SGLang are both carrying large open bug surfaces on the leading‑edge model families — neither shipped a release, and both still lack fixes for their highest‑severity issues. LiteLLM is the only project whose day‑0 activity was a release.

## 3. Model Support Race

**Merged/shaped in the last 24h:**

- **llama.cpp** is the only inference engine that merged new architecture support: **Spark2.5** (`Spark2_5ForCausalLM`) landed in b10828. It also closed Vulkan's ternary‑quantization gap (TQ1_0/TQ2_0, 1.6875 bpw) and shipped the **GDN q/k normalization fix** (`max` → `rsqrt`), aligning its output with the `flash-linear-attention` reference implementation.
- **LiteLLM** added **Synthorai** as an OpenAI‑compatible provider.

**In flight (open PRs/RFCs):**

- **vLLM**: Kimi K3 via FlashInfer KDA kernels (bf16 cache state + prefill/decode backends); GLM‑5.3‑Flash FP8 KVCache fix for SM90 sparse‑MLA; ROCm AITER attention‑sink support for GLM; batch‑invariant Mamba2 prefill; `torchaudio` audio backend; InternVL2 Transformers‑v5 port tracked as open.
- **SGLang**: DeepSeek‑V4 FlexKV mainline integration (C4/C128/indexer pools + UnifiedRadixCache composition); unified KV external linkers for UMBP and Mooncake; GLM‑5.3‑Flash on SM120 integration tracker; Apple Metal redesign RFC; T‑Head PPU (ZW810/ZW810E) upstream roadmap filed.
- **llama.cpp**: XingChen4 (TeleAI) support; Qwen4exp/Qwen3.8‑Flash‑Next draft‑head‑only MTP checkpoints; CUDA/HIP head‑size‑256 flash attention for Qwen3.5/Qwen3‑Next‑class models on RDNA4.
- **Ollama/Unsloth**: no day‑0 model adoption; Ollama showed Blackwell regressions (qwen3moe `sm_120` warmup crash), and Unsloth documented broken Qwen3.6/3.8 MLX paths plus a zero‑diff NVFP4 kernel‑selection investigation.

**Who is ahead.** llama.cpp leads on fast, broad architecture adoption and is the reference for GGUF‑format numerics (its GDN fix cascades to every downstream GGUF consumer). vLLM leads on sparse/hybrid *serving* enablement — it has the most concrete GLM‑5.3‑Flash and Kimi‑K3 kernel work, but the correctness issues on the same model families remain open. SGLang is a close second with stronger infrastructure differentiation on DeepSeek‑V4 KV transfer (FlexKV + Mooncake/UMBP linkers). Ollama and Unsloth are followers today, importing llama.cpp capability rather than defining architecture support.

## 4. Performance Frontier

Optimization effort is concentrated in **six clusters**, roughly in order of activity:

1. **KV cache management and reuse** — the most active frontier across every engine. Evidence: vLLM batch‑invariant Mamba2 prefill (PR #54993), MTP prefix‑cache misses (#53504), DFlash2+YaRN zero‑reuse on a 1.04M prompt (#54094), KV‑offload ownership fix (#53073); SGLang FlexKV + UnifiedRadixCache pools, ~50 GB idle VRAM when EAGLE/MTP is on (#29857), HiSparse offload crash (#33385); llama.cpp `--kv-unified` 42–54% prompt‑processing collapse (#28495); Ollama's MLX prefix‑restore truncation to 8192‑token multiples costing a fixed 17–27 s re‑prefill (#18267); Unsloth adding KV preemption so parallel chats share one context pool instead of evicting each other (PRs #10301/#10358). Cache reuse is becoming the primary cost lever, and every implementation still has correctness gaps.

2. **Kernels and attention backends** — SGLang's AMD DSA prefill top‑k‑v2 kernel is the day's standout benchmark: **−73% top‑k kernel cost, +4.9% token throughput/GPU, −3.5% median TPOT** at ISL 70000 (#37889). llama.cpp has substantial Vulkan work (stream‑k MUL_MAT, int8 coopmat1 MMQ for RDNA3/4, wider MoE expert limits) plus RDNA4 flash‑attention head‑size‑256 fixes. vLLM is fusing DeepEncoder relative attention bias in Triton, removing a 384 MiB BF16 temporary, and integrating FlashInfer KDA kernels for Kimi K3.

3. **Quantized execution** — FP8/NVFP4/ternary paths are the focus, and today it is mostly a correctness fight: vLLM FP8 illegal‑memory‑access on SM120 (workaround: disable FlashInfer scaled‑MM kernel), GLM FP8 KVCache dtype on SM90, Unsloth's NVFP4 kernel‑selection audit across Blackwell SM120/SM121, MiniMax‑M3 W4A16 emitting all‑NUL tokens on sm_121 (SGLang). llama.cpp closed the Vulkan ternary‑quant gap and added a `--fuse-qkv` GGUF conversion flag.

4. **Speculative decoding / MTP** — every inference engine has open correctness bugs: vLLM MTP prompt_logprobs corruption (#53488), SGLang NEXTN acceptance decaying to ~0 until restart (#37326), llama.cpp spec divergence from greedy on quantized targets (#25618) plus EOG rollback (#28232). Unsloth is building a `spec_decoding` acceptance‑rate utility to help teams decide whether a draft model is worth serving.

5. **Distributed serving / PD disaggregation** — SGLang PP prefill deadlock RCA (#34572), vLLM decode‑retract/HiSparse crash fixes (#33385/#36591), llama.cpp explicitly listing disaggregated prefill/decode as roadmap (#21266). Multi‑node deadlocks (Ray gloo) were closed in vLLM, and Unsloth shipped the "two DGX Sparks as an async replica‑routed serving topology" pattern.

6. **Batching/scheduling correctness** — Unsloth fixed batched‑vs‑single greedy divergence on Qwen3.5‑2B and Gemma‑4‑E2B (#9708); llama.cpp made MoE scale‑unpack branchless for batch > 1; SGLang's `perf_repeat_requests` now validates every repeated request rather than only the last.

## 5. Layer Positioning

| Layer | Project(s) | Position today |
|---|---|---|
| **GPU serving engines** (self‑hosted, datacenter scale) | **vLLM, SGLang** | Head‑to‑head on high‑throughput serving, tensor/PD‑disaggregation, speculative decoding, and first‑class support for sparse/hybrid frontier checkpoints. vLLM has slightly more mature multimodal/audio plumbing; SGLang is differentiating on radix/FlexKV cache architecture and hardware breadth (XPU, Apple, PPU roadmap). |
| **Local/edge runtime substrate** | **llama.cpp** | The underlying GGUF + llama-server engine, quant/format leader. Now explicitly a dependency of two other layers: Ollama's local runtime and Unsloth Studio both wrap llama-server (Unsloth maintains its own fork with `--preempt-ram` slot parking). |
| **End‑user distribution & app layer** | **Ollama** | Sits on llama.cpp and adds model library, API server, cloud models, and desktop/agent integrations. Today it imported most of its risk from upstream llama.cpp plus its own MLX/cloud‑model instabilities. |
| **Fine‑tuning / efficiency layer** | **Unsloth** | Primarily LoRA/QLoRA training + GGUF/MLX export, but increasingly end‑to‑end via Studio (serving through its llama.cpp fork). Best described as train‑to‑serve efficiency tooling with a growing ops surface. |
| **Control plane / gateway** | **LiteLLM** | Does not execute models; routes across providers, enforces budgets/keys, and reconciles usage. Today's activity — cosign‑signed images, billing fixes, Rust route‑execution migration — is classic gateway maturation. |

The important structural observation: **llama.cpp is becoming the shared substrate for the "local" half of the stack**, while vLLM/SGLang compete on the datacenter half. LiteLLM sits above both as an API‑agnostic control plane, and Unsloth spans training and serving on the GGUF side. A numerical or kernel change in llama.cpp therefore ripples through Ollama and Unsloth; a kernel gap in vLLM vs SGLang only moves workloads between the two engines.

## 6. Trend Signals

1. **The hybrid/sparse architecture wave is still maturing — pin everything.** Three engines have open temperature‑0 non‑determinism or silent‑corruption bugs on Qwen3.8‑Flash‑Next/DeepSeek‑V4/GLM‑5.3‑Flash class models. Application developers should treat bit‑exact reproducibility as unsupported on these checkpoints until the top‑k/MTP/concurrency kernels are fixed, and add golden‑output regression tests.

2. **Speculative decoding is the least reliable default feature.** Across vLLM, SGLang, and llama.cpp, MTP/draft paths silently corrupt outputs, decay to zero acceptance, or diverge from greedy on quantized targets. Anyone running MTP in production should measure acceptance rates, expect restarts to be part of operations, and consider disabling draft models for eval/RL workloads where determinism matters.

3. **Agent‑tool‑call correctness is the common weak spot.** The same symptom appears at three layers: vLLM drops Qwen3.5 tool calls when XML markup appears inside `<think>` (#39056); llama.cpp and Ollama report the identical failure pattern on Qwen3.5‑class and Gemma‑4 models (llama.cpp #20837, Ollama #18275); LiteLLM's `cache_control_injection_points` triggers a deterministic Claude tool loop (#29810). Treat HTTP 200 as insufficient validation; add timeouts, token caps, and parser‑specific regression tests.

4. **KV‑cache reuse is the new performance battleground — and the new failure mode.** Cache efficiency now differentiates engines (SGLang FlexKV, vLLM prefix cache, llama.cpp `--kv-unified`, Ollama MLX, Unsloth KV preemption), but cold‑start alignment taxes, MTP prefix misses, and offload corruption mean cache reuse can't be assumed. Plan for cache‑warmup overhead and monitor reuse metrics per workload.

5. **Supply‑chain and credential security are becoming release invariants.** LiteLLM made cosign signature verification mandatory in v1.100.0; Unsloth stopped lending backend HF_TOKENs to API‑key callers and is moving Studio to one‑time setup tokens; Ollama still has an open license‑notice compliance issue (#3185). Infrastructure teams should build signature verification and credential‑scoping audits into their pipelines now rather than later.

6. **Hardware diversity is expanding faster than correctness.** Today's evidence: SM120/SM121 Blackwell FP8 crashes (vLLM), qwen3moe warmup crash on `sm_120` (Ollama), MiniMax‑M3 all‑NUL output on `sm_121` (SGLang), Vulkan AMD iGPU regression since v0.32.12 (Ollama), ROCm attention‑kernel gaps on RX 9060 XT (Unsloth), Apple MLX still pre‑production, and T‑Head PPU only at roadmap stage. If you deploy on anything other than current datacenter NVIDIA GPUs, gate model/runtime versions per hardware and test before rollout.

7. **Gateway economics need auditing before they need scaling.** LiteLLM had a false budget lockout (#40050), double‑billed Anthropic cache reads (#40006), and uncharged streaming `/v1/responses` (#29913) all active in one day. For agent workloads making thousands of calls, billing errors are no longer cosmetic — validate cost logs before trusting them, and expect realtime/Vertex pricing fixes to raise bills materially after they merge.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-09-07

## 1. Today's Highlights
No new releases shipped in the last 24h; the day's activity is concentrated in bug fixes plus a CI/docs cleanup wave (~11 PRs opened today). On the correctness front, two tracked temperature=0 non-determinism bugs on sparse/hybrid models (Qwen3.8-Flash-Next FP8, DeepSeek-V4-Flash) remain open without a fix PR, and a fresh CUDA illegal-memory-access report on RTX PRO 5000 (SM120) under sustained FP8 load has only a workaround so far. GLM-5.3-Flash sparse-MLA issues are being actively fixed, including FP8 KVCache dtype on SM90 and video placeholder timestamp handling.

## 3. New Model & Hardware Support
- [PR #55364](https://github.com/vllm-project/vllm/pull/55364) (open): Integrates FlashInfer KDA kernels for Kimi K3 — bf16 KDA cache state plus KDA prefill/decode backends. Default backends remain unchanged; the PR includes a FlashInfer-fused-BF16 vs. Triton-BF16-fallback microbenchmark.
- [PR #55222](https://github.com/vllm-project/vllm/pull/55222) (open): Fixes GLM-5.3-Flash `--kv-cache-dtype fp8` failing to start on the SM90 sparse-MLA backend and right-sizes the indexer-prefill workspace.
- [PR #54404](https://github.com/vllm-project/vllm/pull/54404) (open): Adds attention-sink support to the ROCm AITER sparse-MLA path for GLM, with DCP-combination guardrails and exact zero-value sink semantics using log-LSE.
- [PR #54993](https://github.com/vllm-project/vllm/pull/54993) (open): Opt-in batch-invariant Mamba2 prefill/recovery for a restricted Triton SSU configuration (tracker: [Issue #27433](https://github.com/vllm-project/vllm/issues/27433)).
- [PR #52598](https://github.com/vllm-project/vllm/pull/52598) (open): Adds a `torchaudio` backend to `AudioResampler` and makes it the default over `pyav`, per maintainer discussion.
- [Issue #38425](https://github.com/vllm-project/vllm/issues/38425): InternVL2 port to Transformers v5 remains an open work item.

## 4. Performance & Optimization
- [Issue #27433](https://github.com/vllm-project/vllm/issues/27433) (86 comments): The "Batch Invariant" feature/perf tracker remains the hottest open issue; [PR #54993](https://github.com/vllm-project/vllm/pull/54993) is its first large deliverable.
- [PR #55629](https://github.com/vllm-project/vllm/pull/55629) (open): Fuses DeepEncoder relative attention bias directly in Triton, removing a dense 384 MiB BF16 `[B, heads, HW, HW]` temporary on the 64×64 global-attention path.
- [Issue #50264](https://github.com/vllm-project/vllm/issues/50264): On RDNA (ROCm), hybrid-Mamba models fall back to Triton paged attention and decode collapses at long context. Root cause identified; upstream fix #45916 (split-KV decode kernel) now admits gfx11.
- [Issue #53504](https://github.com/vllm-project/vllm/issues/53504): MTP speculative decoding misses the prefix cache entirely on the first repeat of identical prompts on hybrid Mamba/GDN models.
- [Issue #54094](https://github.com/vllm-project/vllm/issues/54094): DFlash2 + YaRN sees zero prefix-cache reuse on an identical 1.04M prompt while target-only reuse approaches ~1.039M tokens.
- [Issue #38256](https://github.com/vllm-project/vllm/issues/38256) (open): RFC for incremental MoE expert offloading (GPU cache + async pipeline); related [PR #37190](https://github.com/vllm-project/vllm/pull/37190) is open.
- [Issue #46722](https://github.com/vllm-project/vllm/issues/46722) (open): RFC for reducing multimodal payload in token-in/token-out by deferring `pixel_values` preprocessing to `/generate`.

## 5. Stability & Regressions
High severity (open, no fix PR yet):
- Determinism at temperature=0:
  - [Issue #54521](https://github.com/vllm-project/vllm/issues/54521) (33 comments) — Qwen3.8-Flash-Next FP8 returns five different completions for byte-identical greedy requests once the prompt crosses the QSA `indexer_budget` (dense→top-k switch).
  - [Issue #53257](https://github.com/vllm-project/vllm/issues/53257) — DeepSeek-V4-Flash NVFP4 + DSpark: non-determinism rate scales with concurrency.
- [Issue #55571](https://github.com/vllm-project/vllm/issues/55571) — CUDA illegal memory access (Xid 13) on RTX PRO 5000 (SM120) with FP8 under sustained load. Workaround: `VLLM_DISABLED_KERNELS=FlashInferFP8ScaledMMLinearKernel` or `--enforce-eager`. Related: [PR #55651](https://github.com/vllm-project/vllm/pull/55651) proposes skipping FlashInfer kernels that cannot run on the local CUDA toolkit.
- [Issue #39056](https://github.com/vllm-project/vllm/issues/39056) (25 comments, 13👍) — vLLM 0.19 drops Qwen3.5-35B tool calls when XML tool-call markup is emitted inside `<think>`; affects non-streaming parsing with qwen3 reasoning/tool parsers.
- [Issue #54906](https://github.com/vllm-project/vllm/issues/54906) — `thinking_token_budget` ignored by Model Runner V2 with Qwen3.8 NVFP4 + MTP; related [RFC #54864](https://github.com/vllm-project/vllm/issues/54864) proposes a truncate mode for RL rollouts.

Medium severity:
- [Issue #53488](https://github.com/vllm-project/vllm/issues/53488) — `prompt_logprobs` silently corrupted with MTP speculative decoding + chunked prefill (Qwen3.5 family; reproduced on two independent builds).
- [Issue #49896](https://github.com/vllm-project/vllm/issues/49896) — DeepSeek-V4 on SM12x: NaN MQA logits cause `top_k_per_row_prefill` to emit uninitialized smem indices → illegal memory access.
- [Issue #51977](https://github.com/vllm-project/vllm/issues/51977) — `HarmonyError: unexpected tokens remaining in message header` on gpt-oss-120b with tool calling (v0.26.0).
- [Issue #53180](https://github.com/vllm-project/vllm/issues/53180) — TurboQuant k8v4 + MTP silently produces degenerate output on hybrid GDN models (v0.27.1).
- [Issue #48745](https://github.com/vllm-project/vllm/issues/48745) — spurious `EngineDeadError` traceback during graceful shutdown; [Issue #44249](https://github.com/vllm-project/vllm/issues/44249) — LMCache connector asserts on degraded cache instead of honoring recompute fallback.
- [Issue #44889](https://github.com/vllm-project/vllm/issues/44889) — CUDA illegal memory access with Gemma-4-31B + DFlash speculator on H200.

Fix PRs in flight (opened/updated in last 24h):
- [PR #55647](https://github.com/vllm-project/vllm/pull/55647) — GLM-5.3-Flash video placeholder timestamps now come from the actual frame sampler (#55644).
- [PR #55646](https://github.com/vllm-project/vllm/pull/55646) — stops the ROCm worker-kill schedule from preempting the EngineCore cleanup grace (#55632).
- [PR #55642](https://github.com/vllm-project/vllm/pull/55642) — restores soundfile-first automatic audio decoding after #51826 made TorchCodec the default and broke speech jobs.
- [PR #54643](https://github.com/vllm-project/vllm/pull/54643) — fixes `MooncakeStoreConnector` finish-time save crash on hybrid (Kimi-K3-style) models.
- [PR #55307](https://github.com/vllm-project/vllm/pull/55307) — honors `tok_pooling_type="STEP"` in `DispatchPooler.for_seq_cls` instead of silently forcing `AllPool`.
- [PR #53073](https://github.com/vllm-project/vllm/pull/53073) — decouples shared-region creator ownership in KV offload after restart.
- Closed: [#52907](https://github.com/vllm-project/vllm/issues/52907) (Ray gloo multi-node deadlock), [#53462](https://github.com/vllm-project/vllm/issues/53462) (SM110a missing kernel image), [#41865](https://github.com/vllm-project/vllm/issues/41865) (FlashInfer GDN JIT multi-worker deadlock).

## 6. What This Means for Application Developers
- Do not rely on temperature=0 for reproducibility on the newest sparse-attention/hybrid checkpoints (Qwen3.8-Flash-Next, DeepSeek-V4-Flash) until the top-k/MTP concurrency paths are fixed. Pin versions and add golden-output regression tests before running evals.
- Reasoning + tool-calling models (Qwen3.5-35B, Qwen3.8 series) still have correctness gaps: non-streaming tool calls can be dropped when XML appears inside `<think>`, and `thinking_token_budget` may be unenforced under MRV2. Validate with your exact parser flags, and prefer streaming where possible.
- Blackwell workstation/edge users (SM120/SM121) running FP8 should keep `VLLM_DISABLED_KERNELS=FlashInferFP8ScaledMMLinearKernel` and `--enforce-eager` documented until the FlashInfer scaled-MM crash is fixed.
- Audio-pipeline users should monitor [PR #55642](https://github.com/vllm-project/vllm/pull/55642) and [PR #52598](https://github.com/vllm-project/vllm/pull/52598): the default audio backend is deliberately moving to torchaudio, with a soundfile-first restoration pending as a regression fix.
- KV-cache reuse and KV-transfer on hybrid/Mamba models still have sharp edges (first-repeat prefix misses, MooncakeStore hybrid crash) — budget for cache-warmup overhead and monitor reuse metrics, especially with MTP enabled.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-09-07

## 1. Today's Highlights

The most substantive activity is the six-part "Config Round 6" refactor by ch-wan ([#38046](https://github.com/sgl-project/sglang/pull/38046)–[#38049](https://github.com/sgl-project/sglang/pull/38049), [#38113](https://github.com/sgl-project/sglang/pull/38113), [#38114](https://github.com/sgl-project/sglang/pull/38114)), which moved server configuration out of the monolithic `ServerArgs` into namespace-scoped "bags" — review is being coordinated through the do-not-merge omnibus PR. DeepSeek-V4/FlexKV and Apple-silicon efforts also advanced: mainline FlexKV/DSV4 alignment ([#31781](https://github.com/sgl-project/sglang/pull/31781)), unified KV external linkers ([#38269](https://github.com/sgl-project/sglang/pull/38269)), and an updated Apple serving redesign RFC ([#32321](https://github.com/sgl-project/sglang/issues/32321)). CI health continues recovering — the tracking issue reports 1 broken and 8 flaky tests, with 958 recently fixed ([#17050](https://github.com/sgl-project/sglang/issues/17050)).

## 2. Releases & Breaking Changes

No new releases in the last 24 hours. Several behavior-changing PRs are in-flight (not yet merged):

- **Config semantics**: Round 6 introduces a sharp separation between the operator's input ("the record") and the effective resolved configuration ("the bags"); readers that inspect raw records will silently get pre-resolution values. `/server_info` gains a `resolved_dict()` view of what was actually chosen ([#38048](https://github.com/sgl-project/sglang/pull/38048), [#38049](https://github.com/sgl-project/sglang/pull/38049)).
- **CP v1 deprecation, 3 of 5**: generic prefill CP v1 runtime and DSA v1 in-seq-split paths are being removed; only the generic input-sharding handoff is retained ([#36228](https://github.com/sgl-project/sglang/pull/36228)).
- **Rust frontend health semantics**: `/health` and `/health_generate` will return 503 until startup warmup completes, matching Python-frontend behavior — important for orchestrators that probe readiness ([#37994](https://github.com/sgl-project/sglang/pull/37994)).

## 3. New Model & Hardware Support

- **Apple silicon**: Broader roadmap remains open ([#19137](https://github.com/sgl-project/sglang/issues/19137)). The metal redesign RFC was refreshed around a Torch-owned SRT path plus a whole-model exported MLX region, referencing implementation PR #36164 ([#32321](https://github.com/sgl-project/sglang/issues/32321)); a Metal-capture profiler bug is tracked separately ([#30550](https://github.com/sgl-project/sglang/issues/30550)). Docs now prefer the new CUDA-graph disable CLI flags in the Metal guide ([#38271](https://github.com/sgl-project/sglang/pull/38271)).
- **T-Head PPU**: Upstreaming roadmap filed for ZW810/ZW810E and ZW-M890P ([#37519](https://github.com/sgl-project/sglang/issues/37519)).
- **DeepSeek V4**: FlexKV mainline integration aligned with the production cache-transfer stack, adding C4/C128/C4-indexer/state pools and UnifiedRadixCache composition ([#31781](https://github.com/sgl-project/sglang/pull/31781)); unified KV now supported in UMBP and Mooncake direct external linkers with FP4 payload/scale split preserved ([#38269](https://github.com/sgl-project/sglang/pull/38269)).
- **Intel XPU**: Memory-saver support via upstream `torch_memory_saver` with the Level Zero VMM backend ([#29935](https://github.com/sgl-project/sglang/pull/29935)); DSV4 decode-graph capture widths ([#38272](https://github.com/sgl-project/sglang/pull/38272)).
- **GLM-5.3-Flash on SM120** has a dedicated integration/qualification tracker ([#37813](https://github.com/sgl-project/sglang/issues/37813)).

## 4. Performance & Optimization

- **AMD — GLM DSA prefill top-k v2**: Routing prefill through the v2 kernel drops the top-k kernel cost by ~73%; measured **+4.9% token throughput per GPU and −3.5% median TPOT** at ISL 70000/OSL 300 (geomean over concurrency 4–64), GSM8k 0.927 ([#37889](https://github.com/sgl-project/sglang/pull/37889)).
- **Intel XPU — DSV4 decode graphs**: The indexer sizes paged gather/logits buffers from full configured context (up to 262,144); an opt-in capture width avoids freezing huge unused widths into replayed graphs for short conversations ([#38272](https://github.com/sgl-project/sglang/pull/38272)).
- **Intel XPU — GEMM**: Fused bf16×bf16→fp32 path extended beyond CUDA ([#38266](https://github.com/sgl-project/sglang/pull/38266)).
- **Test coverage**: `perf_repeat_requests` now validates every repeated request rather than only the last output, closing a blind spot for state-corruption regressions ([#38185](https://github.com/sgl-project/sglang/pull/38185)).
- **Capacity bug with perf impact**: When EAGLE/MTP is enabled, the KV-pool profiler leaves ~50 GB VRAM idle on a hybrid GDN model (Qwen3.6-27B NVFP4), capping token capacity far below free memory ([#29857](https://github.com/sgl-project/sglang/issues/29857)).

## 5. Stability & Regressions

Ranked roughly by severity:

1. **GLM-5.3-Flash (DSA) HiCache corruption** — host-tier KV load-back corrupts generation even without speculative decoding: dropped tool calls and degenerate repetition loops on 8×H100/TP8 ([#38031](https://github.com/sgl-project/sglang/issues/38031)); tracked alongside other GLM-5.3-Flash bugs in the family tracker ([#37524](https://github.com/sgl-project/sglang/issues/37524)).
2. **PP disaggregated prefill deadlock** — abort storms diverge per-stage bootstrap queues, breaking the P2P send/recv schedule; issue includes RCA + fix series ([#34572](https://github.com/sgl-project/sglang/issues/34572)).
3. **DSPARK + EPLB crash** — `--enable-eplb` plus DSPARK crashes during draft CUDA graph capture with `scatter_add_` dimension mismatch in `on_select_experts` ([#34974](https://github.com/sgl-project/sglang/issues/34974)).
4. **Decode retract crash (HiSparse)** — `HiSparseDSATokenToKVPool` lacks `get_cpu_copy`/`load_cpu_copy`, so unconditional offload crashes during PD-disaggregation retraction; fix PR implements both methods ([#33385](https://github.com/sgl-project/sglang/issues/33385), [#36591](https://github.com/sgl-project/sglang/pull/36591)).
5. **MiniMax-M3 W4A16 on sm_121** — serves but emits token id 0 (all-NUL output) on the Triton MiniMaxSparse path on DGX Spark; same weights correct on vLLM ([#38143](https://github.com/sgl-project/sglang/issues/38143)).
6. **Speculative-decay anomaly** — NEXTN/MTP draft acceptance decays to ~0 over server uptime on Qwen3.8-Flash-Next, fully restored by restart ([#37326](https://github.com/sgl-project/sglang/issues/37326)).
7. **API inconsistency** — `/v1/responses` returns `created_at` as float in streaming events but int in non-streaming responses ([#34716](https://github.com/sgl-project/sglang/issues/34716)).
8. **Model semantic bug** — DeepSeek-V4-Flash `reasoning_effort` mapping is off by one level; `high` is a no-op and vendor `max` is unreachable ([#33185](https://github.com/sgl-project/sglang/issues/33185)).
9. **GLM-5.3 DPC crash** — newly filed, no repro details yet ([#38207](https://github.com/sgl-project/sglang/issues/38207)).
10. **Fixes worth noting** — gloo same-group deadlock in cache prefetch progress detection ([#38270](https://github.com/sgl-project/sglang/pull/38270)); AMD Lean-Attention nondeterminism fix preserving bitwise-accurate logprobs under batch changes ([#37740](https://github.com/sgl-project/sglang/pull/37740)).

A batch of older issues has been auto-closed as inactive, including some still-relevant reports such as the non-streaming raw `<tool_call>` markup leak on truncation ([#30480](https://github.com/sgl-project/sglang/issues/30480)), several speculative-decoding crashes ([#30549](https://github.com/sgl-project/sglang/issues/30549), [#30555](https://github.com/sgl-project/sglang/issues/30555)), and the EPD RFC ([#24945](https://github.com/sgl-project/sglang/issues/24945)).

## 6. What This Means for Application Developers

- **If you serve GLM-5.3-Flash with HiCache/DSA host-tier offload, validate tool-call behavior under load before trusting generations** — the corruption bug ([#38031](https://github.com/sgl-project/sglang/issues/38031)) manifests as silent semantic errors (dropped tool calls, repetition loops) rather than crashes. Pin a known-good version and watch the family tracker ([#37524](https://github.com/sgl-project/sglang/issues/37524)).
- **Speculative decoding remains the riskiest feature area**: expect DSPARK/EPLB crashes ([#34974](https://github.com/sgl-project/sglang/issues/34974)) and slow acceptance decay on NEXTN/MTP endpoints ([#37326](https://github.com/sgl-project/sglang/issues/37326)). A restart "fixes" the latter, which argues for uptime-based recycling or an investigation before long-lived deployments rely on speculative throughput.
- **API contract caveats**: don't assume `created_at` type stability across streaming and non-streaming `/v1/responses` ([#34716](https://github.com/sgl-project/sglang/issues/34716)); the `reasoning_effort` mapping bug means "high" may silently degrade DeepSeek-V4-Flash reasoning quality ([#33185](https://github.com/sgl-project/sglang/issues/33185)).
- **Plan for config-system churn**: after Round 6 lands, tooling that parses `ServerArgs` or assumes unset fields remain absent will need updating — the record-vs-bags distinction makes effective configuration queryable, but only through the new intended paths ([#38114](https://github.com/sgl-project/sglang/pull/38114)).
- **Hardware roadmap watch**: Apple Metal is still pre-production with an active contributor call ([#19137](https://github.com/sgl-project/sglang/issues/19137)); T-Head PPU is at roadmap stage ([#37519](https://github.com/sgl-project/sglang/issues/37519)); MiniMax-M3 W4A16 on Blackwell-class consumer/edge GPUs (sm_121) is not yet correct ([#38143](https://github.com/sgl-project/sglang/issues/38143)).
- **CI signal is finally green-ish**: 958 tests recently fixed with only 1 broken and 8 flaky remaining ([#17050](https://github.com/sgl-project/sglang/issues/17050)) — a reasonable window for validating upstream before adopting newer commits.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-09-07

## Today's Highlights
Nine builds shipped today across the b10821–b10830 range, led by a new `--fuse-qkv` GGUF conversion flag ([#22780](https://github.com/ggml-org/llama.cpp/pull/22780)), a correctness fix for Gated Delta Net (GDN) normalization ([#28068](https://github.com/ggml-org/llama.cpp/pull/28068)), and Spark2.5 model support ([#27868](https://github.com/ggml-org/llama.cpp/pull/27868)). On the engineering side, the hottest topics are speculative-decoding correctness ([#27694](https://github.com/ggml-org/llama.cpp/pull/27694), [#28232](https://github.com/ggml-org/llama.cpp/pull/28232)) and aggressive Vulkan kernel work (stream-k, int8 coopmat1, wider MoE expert limits). Two long-running ternary-quantization (TQ1_0/TQ2_0) Vulkan PRs also reached closure ([#19743](https://github.com/ggml-org/llama.cpp/pull/19743), [#27765](https://github.com/ggml-org/llama.cpp/pull/27765)).

## Releases & Breaking Changes
- **b10830** — `convert`: new `--fuse-qkv` flag to fuse Q/K/V into QKV during HF-to-GGUF conversion ([#22780](https://github.com/ggml-org/llama.cpp/pull/22780)).
- **b10829** — GDN q/k normalization fixed from `max` to `rsqrt`, matching flash-linear-attention's `l2norm(x) = x * rsqrt(sum(x*x) + eps)`. This is a numerical-behavior change: outputs for GDN-based hybrid/linear-attention models will differ from earlier builds and should now match the reference implementation ([#28068](https://github.com/ggml-org/llama.cpp/pull/28068)).
- **b10823** — `common`: new `--log-jsonl` option for structured JSONL logging ([#28437](https://github.com/ggml-org/llama.cpp/pull/28437)).
- **b10822** — UI assets are now embedded directly via CMake, removing the build-time C++ helper and external gzip dependency. Simplifies cross-compilation of `llama-server` with the bundled web UI ([#28445](https://github.com/ggml-org/llama.cpp/pull/28445)).

No explicit breaking API/config changes were announced, but the GDN normalization fix in b10829 will silently change generated output for affected models.

## New Model & Hardware Support
- **Spark2.5 (Spark2_5ForCausalLM)** — merged in b10828; PR was initially filed as “Spark3” then renamed to align with `spark2_5` ([#27868](https://github.com/ggml-org/llama.cpp/pull/27868)).
- **XingChen4 (TeleAI)** — open PR adds support for the China Telecom AI model family ([#28156](https://github.com/ggml-org/llama.cpp/pull/28156)).
- **Qwen4exp / Qwen3.8-Flash-Next** — open PR adds draft-head-only GGUF support for unsloth-layout MTP checkpoints and fixes a draft-loader bug where the target model path was used instead of `-md` ([#28097](https://github.com/ggml-org/llama.cpp/pull/28097)).
- **Vulkan ternary formats** — TQ1_0/TQ2_0 (1.6875 bpw) PRs were closed, indicating the Vulkan backend gap for ternary quants (MUL_MAT, mat-vec, get_rows, dequant) is resolved; validated with BitNet b1.58 models and `test-backend-ops` ([#19743](https://github.com/ggml-org/llama.cpp/pull/19743), [#27765](https://github.com/ggml-org/llama.cpp/pull/27765)).
- **CUDA/RDNA4** — open PR enables WMMA flash attention for head size 256 on RDNA4, unblocking 256-wide attention models (Qwen3.5/Qwen3-Next) that currently fall back to the slower tile kernel ([#28529](https://github.com/ggml-org/llama.cpp/pull/28529)).
- **OpenVINO** — GET_ROWS failures on quantized weights fixed by serving a weight view from the base `Constant` instead of registering a dynamic parameter ([#28381](https://github.com/ggml-org/llama.cpp/pull/28381)).
- **OpenCL / SYCL** — conv2d kernels now handle non-contiguous inputs ([#28503](https://github.com/ggml-org/llama.cpp/pull/28503)); SYCL memory-probe fix for level-zero “get mem error” landed ([#28227](https://github.com/ggml-org/llama.cpp/pull/28227)).

## Performance & Optimization
- **b10821** — Metal: remaining flash-attention vector tunings for M2 Max added ([#28458](https://github.com/ggml-org/llama.cpp/pull/28458)).
- **b10827** — OpenCL: `mul_mat` now properly selects the weights pack for q4_K/q5_K, avoiding the generic path ([#28402](https://github.com/ggml-org/llama.cpp/pull/28402)).
- **CUDA/HIP flash attention for gfx1201** — RDNA4/R9700 PRO FA tuning plus a head-size-256 bug fix; author reports long-context prefill on Qwen3.8 27B was “abysmal” before this work ([#28102](https://github.com/ggml-org/llama.cpp/pull/28102)).
- **Vulkan int8 coopmat1 MMQ** — new implementation for AMD RDNA3/RDNA4 covering q4_0…q6_k, q3_k–q6_k, mxfp4, nvfp4, and iq4_nl; prompt-processing gains reported on Strix Halo ([#27952](https://github.com/ggml-org/llama.cpp/pull/27952)).
- **CUDA MoE/mat-vec** — Spark-pattern prefetch added, and Q4_K/Q5_K scale unpack made branchless so it is no longer re-executed per column in MMVQ; improves batch sizes > 1 ([#26705](https://github.com/ggml-org/llama.cpp/pull/26705)).
- **Vulkan stream-k MUL_MAT** — implemented for scalar/cm1/cm2 paths, currently enabled for cm2; balances 256-element K chunks across all SMs with a resolve pass ([#28528](https://github.com/ggml-org/llama.cpp/pull/28528)).
- **Vulkan MoE** — coopmat1 path avoids unneeded work for empty workgroups ([#25483](https://github.com/ggml-org/llama.cpp/pull/25483)); expert row-id hoisting limit raised from 256 to 512 experts for wide-MoE models ([#28501](https://github.com/ggml-org/llama.cpp/pull/28501)).
- **ROCm reductions** — SUM/MEAN now use hipCUB `DeviceReduce` instead of the single-row `sum_rows` fallback ([#27936](https://github.com/ggml-org/llama.cpp/pull/27936)).
- **CPU/Windows** — bug report: MSVC builds fail to detect/use AVX-VNNI, leaving CPU performance on the table ([#28295](https://github.com/ggml-org/llama.cpp/issues/28295)).

## Stability & Regressions
Ranked roughly by severity; all are open unless noted.

1. **Qwen3.5 tool calls inside thinking blocks** — model emits XML tool calls inside its reasoning block and then stops; active, high-traffic discussion (60 comments, 17 👍). This is a chat-parser/eval issue affecting agentic use ([#20837](https://github.com/ggml-org/llama.cpp/issues/20837)).
2. **CUDA flash-attention fatal errors (`fattn.cu:579`)** — two open reports: llama-server crashes on Gemma 4 31B with MTP and `-sm tensor` after editing a system message ([#24440](https://github.com/ggml-org/llama.cpp/issues/24440)), and a general `ggml-cuda/fattn.cu:579` crash ([#24324](https://github.com/ggml-org/llama.cpp/issues/24324)). No fix PR referenced yet.
3. **Speculative decoding diverges from vanilla greedy on quantized targets** — draft-MTP/draft-dspark reproduce; ngram speculation matches. Fix in flight via probabilistic drafter + rejection sampling ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618), [PR #27694](https://github.com/ggml-org/llama.cpp/pull/27694)).
4. **Speculative results not truncated at EOG** — server can roll back and expose draft tokens past EOG; fix PR open ([#28232](https://github.com/ggml-org/llama.cpp/pull/28232)).
5. **`--kv-unified` prompt-processing collapse** — with `-np 2`, prompt processing drops 42–54% from the second long request onward on single GPU. Root cause identified: CUDA/HIP FA kernels skip only the tail of the KQ mask, not interior all-`-INF` blocks ([#28495](https://github.com/ggml-org/llama.cpp/issues/28495)).
6. **Hybrid/linear-attention context failure** — Qwen3.5-hybrid silently emits instant EOS beyond ~130k context on both CUDA and CPU; suspected DeltaNet recurrent-state depth × layer-count degradation ([#27756](https://github.com/ggml-org/llama.cpp/issues/27756)).
7. **MTP long-session corruption** — Qwen3.6 27B with MTP outputs repeated `////` after long sessions ([#23577](https://github.com/ggml-org/llama.cpp/issues/23577)).
8. **Backend instability reports** — SYCL garbage on the second prompt ([#26845](https://github.com/ggml-org/llama.cpp/issues/26845)); Vulkan on Intel iGPU where the kernel watchdog silently cancels queued submissions and embeddings collapse with no error ([#27634](https://github.com/ggml-org/llama.cpp/issues/27634)); SYCL/OpenCL multi-GPU crashes on unimplemented P2P ([#27168](https://github.com/ggml-org/llama.cpp/issues/27168)).
9. **Server correctness** — `tool_choice: "required"` is accepted but not enforced on Jinja templates with `supports_preserve_reasoning: true` ([#27217](https://github.com/ggml-org/llama.cpp/issues/27217)); llama-ui desktop cannot open the reasoning-level menu ([#27981](https://github.com/ggml-org/llama.cpp/issues/27981)).
10. **Fixed in today’s builds** — grammar max-repetition threshold ([#28469](https://github.com/ggml-org/llama.cpp/pull/28469)), CUDA races in mmid/mmf ([#28475](https://github.com/ggml-org/llama.cpp/pull/28475)), and the GDN normalization bug ([#28068](https://github.com/ggml-org/llama.cpp/pull/28068)). The json-schema-to-grammar issue where nested `maxLength >= 2000` produced un-parseable GBNF is also closed ([#25746](https://github.com/ggml-org/llama.cpp/issues/25746)).

## What This Means for Application Developers
- **Agent/tool-calling stacks should pin and test carefully.** Issue [#20837](https://github.com/ggml-org/llama.cpp/issues/20837) shows current Qwen3.5-class models can bury tool calls inside reasoning blocks and halt; add timeouts/stall detection and validate tool-call parsing in production.
- **`tool_choice: "required"` is not yet a hard guarantee** on reasoning-preserving templates ([#27217](https://github.com/ggml-org/llama.cpp/issues/27217)) — enforce tool-call structure at the application layer if you depend on it.
- **Speculative decoding is being reworked.** If you use `draft-mtp`/`draft-dspark` or expose MTP to end users, expect changes from the rejection-sampling drafter ([#27694](https://github.com/ggml-org/llama.cpp/pull/27694)) and EOG truncation ([#28232](https://github.com/ggml-org/llama.cpp/pull/28232)). Greedy outputs can currently diverge from non-speculative runs on quantized targets.
- **Multi-slot serving with `--kv-unified` has a real throughput caveat.** The 42–54% prompt-processing drop ([#28495](https://github.com/ggml-org/llama.cpp/issues/28495)) matters for shared-KV deployments; watch for a kernel fix before relying on unified KV under sustained load.
- **Ops gains:** `--log-jsonl` ([#28437](https://github.com/ggml-org/llama.cpp/pull/28437)) gives structured logs for ingestion pipelines, and the CMake-embedded UI simplifies cross-compiled `llama-server` builds ([#28445](https://github.com/ggml-org/llama.cpp/pull/28445)).
- **Planning ahead:** disaggregated prefill/decode is an explicit roadmap item ([#21266](https://github.com/ggml-org/llama.cpp/issues/21266)); model-side, the merged Spark2.5 support and in-flight XingChen4/Qwen4exp-draft work suggest continued momentum on Chinese open-weight architectures and hybrid-attention variants.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-09-07

## Today's Highlights

No releases were published in the last 24 hours. The most substantive open work is PR [#16998](https://github.com/ollama/ollama/pull/16998) adding a Prometheus-compatible `/metrics` endpoint and PR [#18278](https://github.com/ollama/ollama/pull/18278) raising the model-name limit from 80 to 96 characters to match Hugging Face. The issue tracker is dominated by stability reports around MLX, Vulkan, Blackwell/FlashAttention, and Cloud-model reliability; the long-running license-notice compliance issue [#3185](https://github.com/ollama/ollama/issues/3185) remains the highest-engagement non-runtime item at 272 👍.

## Releases & Breaking Changes

None in the last 24 hours. No new versions, API/config changes, or migration notes to report.

## New Model & Hardware Support

No new model, backend, or quantization support landed in the last 24 hours. Relevant open/model-adjacent work:

- [#17195](https://github.com/ollama/ollama/pull/17195) — PR: fix legacy `glmocr` GGUFs by registering `<|user|>` as EOT token to prevent runaway output.
- [#18276](https://github.com/ollama/ollama/issues/18276) — qwen3moe on Blackwell `sm_120` currently crashes during warmup when FlashAttention is auto-enabled; not a stable configuration yet.
- [#18269](https://github.com/ollama/ollama/issues/18269) — `muse-glimmer:30b-mlx` NVFP4 MLX variant repeatedly gets stuck in `Stopping...` on a 32 GB M4 Air.

## Performance & Optimization

No optimization PRs were merged in the last 24 hours. Open items with concrete impact:

- [#18267](https://github.com/ollama/ollama/issues/18267) — MLX runner prefix-cache restore is truncated to an 8192-token multiple, so up to 8191 tokens are re-prefilled after every cold prompt. This costs a fixed **17–27 s re-prefill tax** on agent workloads.
- [#18271](https://github.com/ollama/ollama/pull/18271) — PR: send renderer message delimiters to llama-server so context checkpoints can be placed at user-turn boundaries, improving cache reuse when Ollama uses `/completion`.
- [#16998](https://github.com/ollama/ollama/pull/16998) — PR: add opt-in Prometheus `/metrics` endpoint (`OLLAMA_METRICS=1`) with scheduler gauges, request counters, and per-model/token metrics. This is the implementation track for feature request [#3144](https://github.com/ollama/ollama/issues/3144).

## Stability & Regressions

Ranked by severity:

1. [#18275](https://github.com/ollama/ollama/issues/18275) — gemma4 tool-call parser cannot parse `BEGIN_ARG`/`END_ARG` syntax; repair fails and the model enters a degenerate channel loop to the token cap, returning HTTP 200 with no usable content. No fix PR yet.
2. [#18276](https://github.com/ollama/ollama/issues/18276) — qwen3moe + Blackwell `sm_120`: auto-enabled FlashAttention crashes llama-server at warmup with `CUDA error: shared object initialization failed`, even after all 49 layers fit in memory. No fix PR yet.
3. [#18193](https://github.com/ollama/ollama/issues/18193) — Cloud model `glm-5.3:cloud` can enter endless reasoning and abort tasks in OpenCode/ZCode, while the official Z.AI API works normally. No fix PR yet.
4. [#16845](https://github.com/ollama/ollama/issues/16845) — Cloud model `kimi-k2.6:cloud` shows recurring extreme latency (10+ min per request) and stream `INTERNAL_ERROR` on `/api/chat`. No fix PR yet.
5. [#18272](https://github.com/ollama/ollama/issues/18272) — Vulkan backend fails with “Not enough memory for command submission” when loading a 66 GB model on AMD iGPU; regression since v0.32.12, while v0.32.9 works. No fix PR yet.
6. [#18073](https://github.com/ollama/ollama/issues/18073) — New Claude Desktop integration does not work in version 0.33.1. No fix PR yet.
7. [#18269](https://github.com/ollama/ollama/issues/18269) — MLX `muse-glimmer:30b-mlx` NVFP4 model is repeatedly stuck in `Stopping...` state and triggers the watchdog. No fix PR yet.
8. [#18274](https://github.com/ollama/ollama/issues/18274) — Model-name validation is capped at 80 characters, blocking legitimate long `hf.co` model pulls. Fix PR [#18278](https://github.com/ollama/ollama/pull/18278) is open to raise the limit to 96.
9. [#3185](https://github.com/ollama/ollama/issues/3185) — Ollama release artifacts do not include MIT license/copyright notices for statically linked dependencies such as llama.cpp. Legal/compliance issue rather than a runtime regression; no fix PR referenced.

## What This Means for Application Developers

- **Agent tool-call users**: Do not treat HTTP 200 as success for gemma4 models. Malformed tool-call syntax can produce an empty, token-capped response. Add timeouts, token ceilings, and fallbacks. See [#18275](https://github.com/ollama/ollama/issues/18275).
- **Cloud model consumers**: `glm-5.3:cloud` and `kimi-k2.6:cloud` currently have unresolved reliability issues. If your app depends on Ollama Cloud, build in short client timeouts, retries, and provider-native fallback paths. See [#18193](https://github.com/ollama/ollama/issues/18193) and [#16845](https://github.com/ollama/ollama/issues/16845).
- **Apple Silicon agent workloads**: Watch for the MLX prefix-cache alignment issue; cold-cache restore can incur a fixed 17–27 s re-prefill after every cold prompt. See [#18267](https://github.com/ollama/ollama/issues/18267).
- **Hardware rollout targets**: Avoid v0.32.12+ for Vulkan/AMD iGPU deployments with large models, and test carefully before deploying to Blackwell `sm_120` laptops. See [#18272](https://github.com/ollama/ollama/issues/18272) and [#18276](https://github.com/ollama/ollama/issues/18276).
- **Distributors**: If you redistribute Ollama binaries, verify your license-notice obligations are met until the release-artifact compliance issue is fixed. See [#3185](https://github.com/ollama/ollama/issues/3185).
- **HF model pulls**: Wait for PR [#18278](https://github.com/ollama/ollama/pull/18278) to merge before relying on long Hugging Face repo names; until then, keep local model names at or below 80 characters.
- **Claude Desktop pairs**: The v0.33.1 integration regression is real; pin to a known-good version if you depend on it. See [#18073](https://github.com/ollama/ollama/issues/18073).

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-09-07

## 1. Today's Highlights

LiteLLM shipped **v1.100.0**, making cosign-verified Docker images mandatory for all releases. The most active engineering front is a **Rust SDK migration of route execution** ([#40070](https://github.com/BerriAI/litellm/pull/40070)), with parallel work on callback lifecycle and CI stability. Billing correctness dominated the bug tracker: a false budget-exceeded lockout on `/v1/messages`, double-billed Anthropic cache-read tokens, and uncharged streaming `/v1/responses` requests were all active in the last 24h, while several Vertex AI/realtime pricing fixes are in review.

## 2. Releases & Breaking Changes

- **[v1.100.0](https://github.com/BerriAI/litellm/releases/tag/v1.100.0)** — All LiteLLM Docker images are now signed with cosign using the key introduced in [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0). Operators should add signature verification to their pull/deploy pipeline. No API/config breaking changes were called out in the release notes.

## 3. New Model & Hardware Support

- **Synthorai** added as an OpenAI-compatible provider ([PR #38288](https://github.com/BerriAI/litellm/pull/38288), closed).
- **Gaps reported:** DashScope Qwen 3.6/3.7 still missing from the pricing map, so `dashscope/*` routes log $0 cost ([#29922](https://github.com/BerriAI/litellm/issues/29922)); Qwen3.8-27B-FP8 cannot enable `reasoning_effort` through config ([#37359](https://github.com/BerriAI/litellm/issues/37359)).

## 4. Performance & Optimization

- **[PR #40070](https://github.com/BerriAI/litellm/pull/40070)** — refactor(rust): route execution is moving into the Rust SDK; adds **retained callback invocation** so Python callbacks/loggers/guardrails receive the original shared object rather than a by-value copy once Rust replaces the Python dispatcher.
- **[PR #40073](https://github.com/BerriAI/litellm/pull/40073)** — CI fix serializing retained-callback tests to eliminate a ~20% `gc.collect` race on the Rust `release wheel` job.
- **[PR #37075](https://github.com/BerriAI/litellm/pull/37075)** — `/vertex_ai/live` passthrough now bills per-modality tokens (audio/image/camera) instead of text-only; measured under-billing was **9.8x–54.5x** on real sessions.
- **[PR #36887](https://github.com/BerriAI/litellm/pull/36887)** — image/video input tokens are now carried through the Responses usage bridge so per-modality input rates actually apply.

## 5. Stability & Regressions

Ranked by severity:

1. **False budget lockout** — [#40050](https://github.com/BerriAI/litellm/issues/40050): a key with `max_budget=100` returns 429 "Budget has been exceeded" at enforced cost 111.29 while recorded spend is far lower; blocks all Claude Code `/v1/messages` traffic. No fix PR yet.
2. **Double billing of cache reads** — [#40006](https://github.com/BerriAI/litellm/issues/40006): `custom_cost_per_token` with `cache_read_input_token_cost` bills Anthropic cache-read tokens twice.
3. **Streaming Responses never charged** — [#29913](https://github.com/BerriAI/litellm/issues/29913): streaming `/v1/responses` crashes the success logger (`'dict' object has no attribute 'usage'`); no spend-log row is written.
4. **Cache injection no-op + infinite tool loop** — [#29810](https://github.com/BerriAI/litellm/issues/29810): `cache_control_injection_points` on `/v1/responses` is a silent no-op **and** triggers a deterministic Claude tool-call loop until MaxTurns.
5. **Bedrock file cleanup broken** — [#39715](https://github.com/BerriAI/litellm/issues/39715): `DELETE /v1/files/{file_id}` returns 500 (`BedrockFilesConfig does not support file deletion`).
6. **Config reload 404** — [#30772](https://github.com/BerriAI/litellm/issues/30772): `POST /config/reload` returns 404 on `litellm-database:main-stable`.
7. **Zero-cost spend logs** — [#35691](https://github.com/BerriAI/litellm/issues/35691): custom models outside the built-in cost map log `total_cost = 0` even when `usage.estimated_cost` is correct.
8. **UI timezone shift** — [#39979](https://github.com/BerriAI/litellm/issues/39979): Request Logs date-range filter interprets local picker times as UTC.
9. **Passthrough hooks gap** — [#28444](https://github.com/BerriAI/litellm/issues/28444): Post-API hooks are not fired for passthrough endpoints.
10. **Image edits failure** — [#26552](https://github.com/BerriAI/litellm/issues/26552): `/v1/images/edits` with `mask` fails with "Attempted to access streaming request content…".

**Fix PRs in flight:** drop `stream_options` after CCR conversion for DeepSeek ([#40075](https://github.com/BerriAI/litellm/pull/40075)); send camelCase `systemInstruction` to Vertex `generateContent` ([#37030](https://github.com/BerriAI/litellm/pull/37030)); normalize tuple-form JSON Schema `items` for Vertex ([#36934](https://github.com/BerriAI/litellm/pull/36934)); apply deployment-level pricing overrides to realtime sessions ([#36958](https://github.com/BerriAI/litellm/pull/36958)); reject `n>1` and unsupported sizes on Azure MAI image generation instead of forwarding ([#40074](https://github.com/BerriAI/litellm/pull/40074)).

## 6. What This Means for Application Developers

- **Verify image signatures** for v1.100.0 in your deploy pipeline; this is now a release invariant.
- **Budget-enforced keys (especially Claude Code):** #40050 can hard-lock keys once enforced cost crosses ~111% of budget, so monitor for unexpected 429s before rolling out v1.100.0 to max-budget tenants.
- **Spend/chargeback data is unreliable** for streaming `/v1/responses` requests ([#29913](https://github.com/BerriAI/litellm/issues/29913)) and custom-priced models ([#35691](https://github.com/BerriAI/litellm/issues/35691)) — validate billing before relying on it for those paths.
- **Anthropic cache pricing:** if you use `custom_cost_per_token` with cache-read rates, audit invoices — #40006 double-counts.
- **Avoid `cache_control_injection_points` with agent SDKs on `/v1/responses`** for now ([#29810](https://github.com/BerriAI/litellm/issues/29810)); the tool-call loop can burn significant tokens until MaxTurns.
- **Watch the Rust route-execution migration** ([#40070](https://github.com/BerriAI/litellm/pull/40070)): callbacks/loggers should behave identically, but any custom Python callback relying on object identity or shared-mutation semantics deserves a regression test when it lands.
- **Realtime/Vertex users:** the in-review pricing fixes ([#37075](https://github.com/BerriAI/litellm/pull/37075), [#36958](https://github.com/BerriAI/litellm/pull/36958), [#36887](https://github.com/BerriAI/litellm/pull/36887)) will materially change what realtime sessions and multimodal Responses requests cost — expect higher, more accurate bills after merge.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-09-07

## Today's Highlights
Unsloth Studio serving is the main story: PRs add KV-cache preemption so parallel chat slots share one context pool instead of evicting each other ([#10301](https://github.com/unslothai/unsloth/pull/10301), [#10358](https://github.com/unslothai/unsloth/pull/10358)), and pair two DGX Sparks into an async replica-routed serving topology ([#10323](https://github.com/unslothai/unsloth/pull/10323)). A high-severity correctness bug where batched greedy generation diverged from one-at-a-time decoding on Qwen3.5-2B and gemma-4-E2B was closed ([#9708](https://github.com/unslothai/unsloth/issues/9708)), and a zero-diff report documents NVFP4 kernel selection across Blackwell SM120/SM121 parts ([#10417](https://github.com/unslothai/unsloth/pull/10417)).

## Releases & Breaking Changes
No new releases were cut in the last 24 hours. In-flight PRs will change Studio behavior once merged:

- Studio will stop serving the seeded admin password and instead issue a one-time setup token; the first-login "Current password" flow is unchanged ([#10387](https://github.com/unslothai/unsloth/pull/10387)).
- uv cache handling becomes consistent across `install.sh`, `install.ps1`, `unsloth studio update`, and the backend; installers will record which cache they used when content inspection is ambiguous ([#10386](https://github.com/unslothai/unsloth/pull/10386), [#10410](https://github.com/unslothai/unsloth/pull/10410)).
- The torchcodec compatibility guard will cover torch 2.11 and pin per torch minor, with a large shell-parsing rewrite for the notebook validator ([#7474](https://github.com/unslothai/unsloth/pull/7474), [#10414](https://github.com/unslothai/unsloth/pull/10414)).
- The `python`/`terminal` tool result truncation cap becomes settable per call, in addition to the install-wide `UNSLOTH_TOOL_RESULT_MAX_CHARS` ([#10398](https://github.com/unslothai/unsloth/pull/10398)).

## New Model & Hardware Support
No new model releases were announced. Notable model/backend activity:

- Qwen3.6-35B-A3B MLX API calls fail via both base64 and URL methods ([#10389](https://github.com/unslothai/unsloth/issues/10389)); Qwen3.8-27B-Uncensored-MLX crashes in `tokenizer_utils.py:229-237` ([#10385](https://github.com/unslothai/unsloth/issues/10385)).
- NVFP4 quants on Blackwell consumer/workstation GPUs (SM120/SM121: RTX 50 series, RTX PRO 6000, DGX Spark) received a full kernel-selection investigation with results recorded, no code changes ([#10417](https://github.com/unslothai/unsloth/pull/10417)).
- AMD ROCm gaps persist: Wan2.2 TI2V video generation OOMs on RX 9060 XT because no fused attention kernel is available and PyTorch SDPA math fallback is used ([#10415](https://github.com/unslothai/unsloth/issues/10415)); ROCm users also report Studio ignoring the "No RAM Offload" checkbox on Radeon PRO W7900/W7500 ([#10341](https://github.com/unslothai/unsloth/issues/10341)).
- torchcodec support is being pinned per torch minor, adding torch 2.11 to the compatibility guard ([#7474](https://github.com/unslothai/unsloth/pull/7474)).

## Performance & Optimization
- **KV preemption for parallel chats**: Studio currently launches llama-server with `--parallel N --kv-unified`, where each slot believes it owns the full context but the server only checks `prompt_tokens < slot.n_ctx` — so parallel chats evict each other. [#10301](https://github.com/unslothai/unsloth/pull/10301) adds Studio-side preemption so one shared KV cache serves all chats; [#10358](https://github.com/unslothai/unsloth/pull/10358) defers to llama-server's own `--preempt-ram` slot parking (unslothai/llama.cpp#184/#190) so chats can use the full context window with host-RAM offload.
- **Two-Spark orchestrator**: when a GGUF is loaded on paired DGX Sparks, the backend chooses among three topologies via `spark_cluster.recommend_topology` and serves through an async replica router ([#10323](https://github.com/unslothai/unsloth/pull/10323)).
- **Throughput telemetry fix**: analysis of 48,216 `engine_stats` records from a Strix Halo log bundle showed 0 tok/s reported for almost the entire generation, plus two readings at rates the hardware cannot produce; the PR makes the reported number what the engine could plausibly deliver ([#10384](https://github.com/unslothai/unsloth/pull/10384)).
- **Speculative-decoding instrumentation**: a new `unsloth/spec_decoding` utility measures draft/target acceptance rate so teams can decide whether a draft model is worth serving ([#10401](https://github.com/unslothai/unsloth/issues/10401), [#10416](https://github.com/unslothai/unsloth/pull/10416)).
- **Kaggle CI cost**: dispatching kernels and collecting results later cuts held runner time on T4 smoke jobs from ~41.5 min to ~4 min of useful build/push work ([#10183](https://github.com/unslothai/unsloth/pull/10183)).

## Stability & Regressions
Ranked by severity:

1. **[Fixed]** Batched greedy generation produced different text than one-at-a-time generation on LoRA-tuned Qwen3.5-2B and gemma-4-E2B (T4, left padding); closed after reproduction across two models ([#9708](https://github.com/unslothai/unsloth/issues/9708)).
2. **[Fix PR]** Windows Smart App Control / code-integrity blocks can make Studio launch and then fail to load a model, with `llama-server.exe — Bad Image` errors; the PR adds a runtime probe and CI bundle-signature audit ([#10408](https://github.com/unslothai/unsloth/pull/10408)).
3. **[Open]** Qwen3.5-9B never reaches the first training step, and Gemma 4 26B-A4B QLoRa OOMs at batch size 1 on 96 GB VRAM (RTX Pro 6000) ([#7203](https://github.com/unslothai/unsloth/issues/7203)).
4. **[Open]** GPT-OSS-120B-K4-KM in Studio errors with "output does not match the expected peg-native format" on default settings ([#10252](https://github.com/unslothai/unsloth/issues/10252)).
5. **[Open]** ROCm: Wan2.2 TI2V OOM on RX 9060 XT due to missing fused attention / SDPA math fallback ([#10415](https://github.com/unslothai/unsloth/issues/10415)); separately, models remain in host RAM on Radeon PRO despite "No RAM Offload" being checked ([#10341](https://github.com/unslothai/unsloth/issues/10341)).
6. **[Open]** Qwen3.6-35B-A3B MLX API unusable via both base64 and URL input ([#10389](https://github.com/unslothai/unsloth/issues/10389)); Qwen3.8-27B-Uncensored-MLX crashes in tokenizer_utils ([#10385](https://github.com/unslothai/unsloth/issues/10385)).
7. **[Open]** Studio API-key protection fails for a 238-char key with "RSAES-OAEP: input message length is too long" ([#10411](https://github.com/unslothai/unsloth/issues/10411)); keyless auth also fails for harnesses sending an empty bearer token ([#10400](https://github.com/unslothai/unsloth/issues/10400)).
8. **[Open]** `--tensor-split` is silently ignored when distributing models across devices ([#10355](https://github.com/unslothai/unsloth/issues/10355)).
9. **[Fix PR]** Two frontend regressions from the composer-icon work reddened CI; crypto-polyfill and Firefox zoom-measurement fixes are up ([#10412](https://github.com/unslothai/unsloth/pull/10412), [#10413](https://github.com/unslothai/unsloth/pull/10413)).
10. **[Closed security fix]** Studio Hub write paths no longer lend the backend's HF_TOKEN to API-key callers; read/video/audio/image paths were already covered ([#10126](https://github.com/unslothai/unsloth/issues/10126)).

## What This Means for Application Developers
- **KV cache semantics are changing**: if you build clients against Studio's local llama-server, per-slot context will no longer be a hard guarantee — when the shared pool fills, older chat slots can be parked to host RAM. Build retry/backoff into generation calls and avoid assuming a slot's declared context is always resident ([#10301](https://github.com/unslothai/unsloth/pull/10301), [#10358](https://github.com/unslothai/unsloth/pull/10358)).
- **Re-validate eval harnesses** after the batched-vs-single decoding fix lands; batched greedy runs on small Qwen/gemma-4 LoRAs were silently producing different text ([#9708](https://github.com/unslothai/unsloth/issues/9708)).
- **Studio security hardening is real**: automation that relies on the seeded admin password or on backend HF_TOKENs reaching API-key callers needs to move to the one-time setup-token flow and per-user Hub credentials ([#10387](https://github.com/unslothai/unsloth/pull/10387), [#10126](https://github.com/unslothai/unsloth/issues/10126)).
- **MLX-format Qwen is fragile today** — Qwen3.6-35B-A3B API calls fail outright and Qwen3.8-27B-MLX crashes at tokenization; validate before depending on those paths ([#10389](https://github.com/unslothai/unsloth/issues/10389), [#10385](https://github.com/unslothai/unsloth/issues/10385)).
- **Speculative decoding users** will soon be able to measure acceptance rate instead of eyeballing tok/s before paying for a draft model ([#10401](https://github.com/unslothai/unsloth/issues/10401), [#10416](https://github.com/unslothai/unsloth/pull/10416)).
- **On AMD ROCm**, attention-kernel coverage remains the main blocker for video workloads and offload controls; plan for PyTorch SDPA fallback and monitor the tracker before committing ([#10415](https://github.com/unslothai/unsloth/issues/10415), [#10341](https://github.com/unslothai/unsloth/issues/10341)).

</details>