# AI 基础设施日报 2026-09-07

> 生成时间: 2026-09-07 04:41 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# 跨项目 AI 基础设施对比 — 2026‑09‑07

## 1. 生态系统概览

六个项目今天都在处理同一类问题：最新一代开放权重前沿模型 —— Qwen3.8‑Flash‑Next、DeepSeek‑V4、GLM‑5.3‑Flash、Kimi K3、MiniMax‑M3 —— 构建在稀疏 MLA / GDN / 混合线性注意力架构之上，而这些架构的 kernel、FP8/NVFP4 量化路径和 MTP 推测解码器在大规模下还不可靠。llama.cpp 技术栈（GGUF + llama-server）正在整合为本地/边缘服务的底座：Ollama 和 Unsloth Studio 实际上都基于它发布，而 vLLM 和 SGLang 则在竞逐同一波架构的数据中心级支持。各项目共同的痛点已经从原始吞吐转向正确性：temperature-0 非确定性、KV 缓存复用失败、工具调用静默损坏和推测解码发散占据了 issue 跟踪器的主导位置。与此同时，控制面与生命周期问题 —— 计费准确性、签名镜像、凭据范围、配置语义 —— 正在网关和微调层成为头等工程事务。

## 2. 活动对比

下面的计数是各项目更新摘要中显式引用的可追踪 issue/PR 标识符数量，不是仓库完整总量。

| 项目 | 引用 Issue 数 | 引用 PR 数 | 最近 24h 发布 | 发布/活跃动态 |
|---|---|---|---|---|
| vLLM | ~25 | 14（≈11 个今日新开） | 无 | 无发布；围绕稀疏/混合架构确定性、SM120 FP8 非法内存访问、GLM‑5.3‑Flash 修复做了大量 triage |
| SGLang | 21 | 19 | 无 | 无发布；“Config Round 6” 重构（6 个 PR）、FlexKV/DeepSeek‑V4 对齐、CI 恢复（修复 958 个测试） |
| llama.cpp | 15 | 26 | 9 个构建（b10821–b10830） | 发布节奏最高：合并新模型支持（Spark2.5）、GDN 数值修复、高强度的 Vulkan/CUDA kernel 工作 |
| Ollama | 11 | 4 | 无 | PR 变动少；更新摘要以稳定性报告为主（MLX、Vulkan、Blackwell、云模型） |
| LiteLLM | 12 | 10 | **v1.100.0** | 今日有一个发布 —— cosign 签名的 Docker 镜像成为强制要求；计费正确性 + Rust 路由执行迁移 |
| Unsloth | 12 | 16 | 无 | 无发布；Studio 服务功能（KV 抢占、双 Spark 拓扑）+ 安全/运维清理 |

**解读。** llama.cpp 每天多次发布变更，这一点很关键，因为 Ollama 和 Unsloth Studio 会在下游继承它的行为。vLLM 和 SGLang 都在前沿模型家族上背着大片未关闭的 bug 面 —— 两者都没有发布版本，也都没有修复各自最高严重级的问题。LiteLLM 是唯一一个当天动作就是发版的项目。

## 3. 模型支持竞赛

**最近 24 小时内已合并/成形：**

- **llama.cpp** 是唯一合并了新架构支持的推理引擎：**Spark2.5**（`Spark2_5ForCausalLM`）在 b10828 中落地。它还补齐了 Vulkan 的三值量化缺口（TQ1_0/TQ2_0，1.6875 bpw），并发布了 **GDN q/k 归一化修复**（`max` → `rsqrt`），使输出与 `flash-linear-attention` 参考实现对齐。
- **LiteLLM** 新增 **Synthorai** 作为 OpenAI 兼容提供商。

**进行中（开放的 PR/RFC）：**

- **vLLM**：通过 FlashInfer KDA kernel（bf16 缓存状态 + prefill/decode 后端）支持 Kimi K3；GLM‑5.3‑Flash FP8 KVCache 在 SM90 稀疏 MLA 上的修复；ROCm AITER 为 GLM 提供 attention-sink 支持；batch 无关的 Mamba2 prefill；`torchaudio` 音频后端；InternVL2 到 Transformers‑v5 的移植仍标记为 open。
- **SGLang**：DeepSeek‑V4 FlexKV 主线集成（C4/C128/indexer 池 + UnifiedRadixCache 组合）；面向 UMBP 和 Mooncake 的统一 KV 外部链接器；SM120 上 GLM‑5.3‑Flash 集成跟踪；Apple Metal 重设计 RFC；T‑Head PPU（ZW810/ZW810E）上游路线图已提交。
- **llama.cpp**：XingChen4（TeleAI）支持；Qwen4exp/Qwen3.8‑Flash‑Next 的仅 draft-head MTP 检查点；面向 RDNA4 上 Qwen3.5/Qwen3‑Next 类模型的 CUDA/HIP head-size-256 flash attention。
- **Ollama/Unsloth**：当天没有新模型采纳；Ollama 出现 Blackwell 回归（qwen3moe 在 `sm_120` 上 warmup 崩溃）；Unsloth 记录了 Qwen3.6/3.8 MLX 路径损坏，以及一项 zero-diff NVFP4 kernel 选择调查。

**谁在领跑。** llama.cpp 在快速、广泛的架构采纳上领先，也是 GGUF 格式数值的参照实现（它的 GDN 修复会级联到每个下游 GGUF 消费者）。vLLM 在稀疏/混合架构的*服务*就绪度上领先 —— 它有最具体的 GLM‑5.3‑Flash 和 Kimi K3 kernel 工作，但同一模型家族上的正确性问题仍未关闭。SGLang 紧随其后，在 DeepSeek‑V4 KV 传输（FlexKV + Mooncake/UMBP 链接器）上有更强的基础设施差异化。Ollama 和 Unsloth 目前是跟随者，更多是在引入 llama.cpp 的能力，而不是定义架构支持。

## 4. 性能前沿

优化工作集中在**六个方向**，大致按活跃度排序：

1. **KV 缓存管理与复用** —— 所有引擎中最活跃的前沿方向。证据：vLLM 的 batch 无关 Mamba2 prefill（PR #54993）、MTP 前缀缓存未命中（#53504）、DFlash2+YaRN 在 1.04M prompt 上零复用（#54094）、KV offload 所有权修复（#53073）；SGLang 的 FlexKV + UnifiedRadixCache 池、启用 EAGLE/MTP 时约 50 GB 显存闲置（#29857）、HiSparse offload 崩溃（#33385）；llama.cpp 的 `--kv-unified` 使 prompt 处理耗时降低 42–54%（#28495）；Ollama 的 MLX 前缀恢复被截断到 8192 token 的倍数，造成固定 17–27 秒的重新 prefill（#18267）；Unsloth 新增 KV 抢占，让并行会话共享一个上下文池而不是互相驱逐（PR #10301/#10358）。缓存复用正在成为最主要的成本杠杆，而每个实现的正确性都还有缺口。

2. **Kernel 与注意力后端** —— SGLang 的 AMD DSA prefill top-k-v2 kernel 是当日最亮眼的基准：ISL 70000 下 **top-k kernel 成本 −73%、每 GPU token 吞吐 +4.9%、中位 TPOT −3.5%**（#37889）。llama.cpp 有大量 Vulkan 工作（stream-k MUL_MAT、面向 RDNA3/4 的 int8 coopmat1 MMQ、更宽的 MoE expert 上限），外加 RDNA4 flash-attention head-size-256 修复。vLLM 正在 Triton 中融合 DeepEncoder 相对注意力偏置，移除一个 384 MiB 的 BF16 临时张量，并为 Kimi K3 集成 FlashInfer KDA kernel。

3. **量化执行** —— FP8/NVFP4/三值路径是焦点，今天主要是在打正确性硬仗：vLLM 在 SM120 上 FP8 非法内存访问（临时绕过方案：禁用 FlashInfer scaled-MM kernel）、SM90 上 GLM FP8 KVCache 的 dtype 问题、Unsloth 对 Blackwell SM120/SM121 的 NVFP4 kernel 选择审计、MiniMax‑M3 W4A16 在 sm_121 上输出全 NUL token（SGLang）。llama.cpp 补齐了 Vulkan 三值量化缺口，并新增 `--fuse-qkv` GGUF 转换标志。

4. **推测解码 / MTP** —— 每个推理引擎都有未关闭的正确性 bug：vLLM 的 MTP prompt_logprobs 损坏（#53488）、SGLang 的 NEXTN 接受率衰减到 ~0 直到重启（#37326）、llama.cpp 在量化目标上推测结果与贪心解码发散（#25618），以及 EOG 回滚（#28232）。Unsloth 正在构建一个 `spec_decoding` 接受率工具，帮助团队判断 draft 模型是否值得上线服务。

5. **分布式服务 / PD 分离** —— SGLang 的 PP prefill 死锁根因分析（#34572）、vLLM 的 decode-retract/HiSparse 崩溃修复（#33385/#36591）、llama.cpp 明确将分离式 prefill/decode 列为路线图（#21266）。vLLM 关闭了多节点死锁（Ray gloo）；Unsloth 发布了“双 DGX Spark 作为异步副本路由服务拓扑”的模式。

6. **批处理/调度正确性** —— Unsloth 修复了 Qwen3.5‑2B 和 Gemma‑4‑E2B 上批处理与单条请求贪心解码发散的问题（#9708）；llama.cpp 让 MoE 的 scale-unpack 在 batch > 1 时变为无分支实现；SGLang 的 `perf_repeat_requests` 现在会校验每一个重复请求，而不是只校验最后一个。

## 5. 分层定位

| 层级 | 项目 | 当前定位 |
|---|---|---|
| **GPU 服务引擎**（自托管、数据中心规模） | **vLLM、SGLang** | 在高吞吐服务、tensor/PD 分离、推测解码，以及对稀疏/混合前沿 checkpoint 的一流支持上正面对决。vLLM 的多模态/音频管线稍更成熟；SGLang 通过 radix/FlexKV 缓存架构和硬件广度（XPU、Apple、PPU 路线图）形成差异化。 |
| **本地/边缘运行时底座** | **llama.cpp** | 底层 GGUF + llama-server 引擎，量化/格式领导者。现在明确成为另外两个层的依赖：Ollama 的本地运行时和 Unsloth Studio 都封装了 llama-server（Unsloth 维护自己的 fork，实现了 `--preempt-ram` 槽位驻留）。 |
| **终端用户分发与应用层** | **Ollama** | 构建在 llama.cpp 之上，增加了模型库、API 服务器、云模型和桌面/Agent 集成。如今它的大部分风险来自上游 llama.cpp，外加自身的 MLX/云模型不稳定问题。 |
| **微调/效率层** | **Unsloth** | 主要是 LoRA/QLoRA 训练 + GGUF/MLX 导出，但正通过 Studio 越来越走向端到端（经由其 llama.cpp fork 提供服务）。最恰当的描述是“从训练到服务的效率工具”，且运维面不断变大。 |
| **控制面/网关** | **LiteLLM** | 不执行模型；跨提供商路由、执行预算/密钥策略并核对用量。今天的动态 —— cosign 签名镜像、计费修复、Rust 路由执行迁移 —— 是典型的网关成熟化过程。 |

这里有一个重要的结构性观察：**llama.cpp 正在成为技术栈中“本地”那一半的共享底座**，而 vLLM/SGLang 在数据中心那一半竞争。LiteLLM 作为与 API 无关的控制面位于两者之上，Unsloth 则横跨 GGUF 侧的训练与服务。因此，llama.cpp 中的任何数值或 kernel 变更都会波及 Ollama 和 Unsloth；而 vLLM 与 SGLang 之间的 kernel 差距，只会让工作负载在这两个引擎之间迁移。

## 6. 趋势信号

1. **混合/稀疏架构浪潮仍在成熟期 —— 请钉死一切版本。** 三个引擎在 Qwen3.8‑Flash‑Next/DeepSeek‑V4/GLM‑5.3‑Flash 类模型上都有未关闭的 temperature-0 非确定性或静默损坏 bug。应用开发者应将这些 checkpoint 上的逐位精确可复现性视为不受支持，直到 top-k/MTP/并发 kernel 修复完毕，并补充 golden 输出回归测试。

2. **推测解码是最不可靠的默认功能。** 在 vLLM、SGLang 和 llama.cpp 中，MTP/draft 路径要么静默损坏输出，要么接受率衰减到零，要么在量化目标上与贪心结果发散。任何在生产环境运行 MTP 的人都应该测量接受率、把重启视为运维的一部分，并在对确定性有要求的 eval/RL 工作负载上考虑禁用 draft 模型。

3. **Agent 工具调用正确性是共同的薄弱环节。** 相同症状出现在三个层面：vLLM 在 XML 标记出现在 `<think>` 内部时会丢弃 Qwen3.5 的工具调用（#39056）；llama.cpp 和 Ollama 在 Qwen3.5 类与 Gemma‑4 模型上报告了相同的失败模式（llama.cpp #20837、Ollama #18275）；LiteLLM 的 `cache_control_injection_points` 会触发确定性的 Claude 工具循环（#29810）。不要把 HTTP 200 当作充分验证；请添加超时、token 上限以及针对解析器的回归测试。

4. **KV 缓存复用是新的性能战场 —— 也是新的故障模式。** 缓存效率正在成为引擎间的分水岭（SGLang FlexKV、vLLM prefix cache、llama.cpp `--kv-unified`、Ollama MLX、Unsloth KV 抢占），但冷启动对齐开销、MTP 前缀未命中和 offload 损坏意味着缓存复用不能被想当然。请为缓存预热开销做好规划，并按工作负载监控复用指标。

5. **供应链与凭据安全正在成为发布中必须守住的不变量。** LiteLLM 在 v1.100.0 中将 cosign 签名验证设为强制；Unsloth 不再将后端 HF_TOKEN 出借给 API 密钥调用方，并把 Studio 迁移到一次性设置 token；Ollama 仍有一个未关闭的许可证声明合规问题（#3185）。基础设施团队现在就应该把签名验证和凭据范围审计纳入流水线，而不是留到以后。

6. **硬件多样性的扩展速度快于正确性的跟进。** 今天的证据：SM120/SM121 Blackwell FP8 崩溃（vLLM）、qwen3moe 在 `sm_120` 上 warmup 崩溃（Ollama）、MiniMax‑M3 在 sm_121 上输出全 NUL token（SGLang）、Vulkan AMD iGPU 自 v0.32.12 起出现回归（Ollama）、ROCm attention kernel 在 RX 9060 XT 上的缺口（Unsloth）、Apple MLX 仍处于预生产阶段、T‑Head PPU 还停留在路线图阶段。如果你的部署目标不是当前数据中心级 NVIDIA GPU，请按硬件分别把关模型/运行时版本，并在 rollout 前逐一测试。

7. **网关经济性需要先审计，再谈扩展。** LiteLLM 在一天之内同时出现了预算误锁定（#40050）、Anthropic 缓存读取双重计费（#40006）和流式 `/v1/responses` 未计费（#29913）三个问题。对于每天发起数千次调用的 Agent 工作负载来说，计费错误不再是表面问题 —— 在信任成本日志之前先做校验，并预期 realtime/Vertex 定价修复合并后，账单金额会明显上升。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 文摘 — 2026-09-07

## 1. 今日要点
过去 24 小时没有发布新版本；当日动态集中在 bug 修复以及一波 CI/文档清理上（今天约新开 11 个 PR）。正确性方面，两个已跟踪的稀疏/混合模型 temperature=0 非确定性 bug（Qwen3.8-Flash-Next FP8、DeepSeek-V4-Flash）仍未关闭，也没有修复 PR；另有一份新的 RTX PRO 5000 (SM120) 在持续 FP8 负载下触发 CUDA 非法内存访问的报告，目前只有绕过方案。GLM-5.3-Flash 的 sparse-MLA 问题正在积极修复中，包括 SM90 上 FP8 KVCache dtype、视频占位符时间戳处理。

## 3. 新模型与硬件支持
- [PR #55364](https://github.com/vllm-project/vllm/pull/55364)（未合并）：为 Kimi K3 集成 FlashInfer KDA kernels——支持 bf16 KDA 缓存状态以及 KDA prefill/decode 后端。默认后端保持不变；该 PR 包含 FlashInfer-fused-BF16 与 Triton-BF16-fallback 的微基准比较。
- [PR #55222](https://github.com/vllm-project/vllm/pull/55222)（未合并）：修复 GLM-5.3-Flash 在 SM90 sparse-MLA 后端上 `--kv-cache-dtype fp8` 无法启动的问题，并正确调整 indexer-prefill workspace 的大小。
- [PR #54404](https://github.com/vllm-project/vllm/pull/54404)（未合并）：为 GLM 的 ROCm AITER sparse-MLA 路径添加 attention-sink 支持，包含 DCP-combination 防护措施，并利用 log-LSE 实现精确的零值 sink 语义。
- [PR #54993](https://github.com/vllm-project/vllm/pull/54993)（未合并）：为受限 Triton SSU 配置引入可选的 batch-invariant Mamba2 prefill/recovery（跟踪 issue：[Issue #27433](https://github.com/vllm-project/vllm/issues/27433)）。
- [PR #52598](https://github.com/vllm-project/vllm/pull/52598)（未合并）：根据维护者讨论，为 `AudioResampler` 添加 `torchaudio` 后端，并将其设为默认后端，取代 `pyav`。
- [Issue #38425](https://github.com/vllm-project/vllm/issues/38425)：将 InternVL2 移植到 Transformers v5 仍是待办工作项。

## 4. 性能与优化
- [Issue #27433](https://github.com/vllm-project/vllm/issues/27433)（86 条评论）：这个 “Batch Invariant” 功能/性能跟踪 issue 仍是当前最热门的未关闭 issue；[PR #54993](https://github.com/vllm-project/vllm/pull/54993) 是它的首项大型交付成果。
- [PR #55629](https://github.com/vllm-project/vllm/pull/55629)（未合并）：在 Triton 中直接融合 DeepEncoder 的相对注意力偏置，去除了 64×64 全局注意力路径上一个 384 MiB 的稠密 BF16 `[B, heads, HW, HW]` 临时张量。
- [Issue #50264](https://github.com/vllm-project/vllm/issues/50264)：在 RDNA (ROCm) 上，hybrid-Mamba 模型会回退到 Triton paged attention，长上下文下 decode 会崩溃。已定位根因；上游修复 #45916（split-KV decode kernel）现已支持 gfx11。
- [Issue #53504](https://github.com/vllm-project/vllm/issues/53504)：在 hybrid Mamba/GDN 模型上，MTP speculative decoding 在首次重复完全相同 prompt 时会完全无法命中 prefix cache。
- [Issue #54094](https://github.com/vllm-project/vllm/issues/54094)：DFlash2 + YaRN 在完全相同的 1.04M prompt 上 prefix-cache 复用为零；而 target-only 复用接近约 1.039M tokens。
- [Issue #38256](https://github.com/vllm-project/vllm/issues/38256)（未关闭）：RFC：增量式 MoE expert offloading（GPU cache + async pipeline）；相关 [PR #37190](https://github.com/vllm-project/vllm/pull/37190) 仍未合并。
- [Issue #46722](https://github.com/vllm-project/vllm/issues/46722)（未关闭）：RFC：通过将 `pixel_values` 的预处理推迟到 `/generate`，减少 token-in/token-out 中的多模态负载。

## 5. 稳定性与回归问题
高严重度（未关闭，尚无修复 PR）：

- temperature=0 时的确定性：
  - [Issue #54521](https://github.com/vllm-project/vllm/issues/54521)（33 条评论）— 一旦 prompt 超过 QSA `indexer_budget`（dense→top-k 切换），Qwen3.8-Flash-Next FP8 对字节完全相同的 greedy 请求会返回五种不同的补全结果。
  - [Issue #53257](https://github.com/vllm-project/vllm/issues/53257) — DeepSeek-V4-Flash NVFP4 + DSpark：非确定性比例随并发度提高而上升。
- [Issue #55571](https://github.com/vllm-project/vllm/issues/55571) — CUDA 非法内存访问（Xid 13）：RTX PRO 5000 (SM120) 在持续 FP8 负载下出现。规避方法：设置 `VLLM_DISABLED_KERNELS=FlashInferFP8ScaledMMLinearKernel`，或使用 `--enforce-eager`。相关 [PR #55651](https://github.com/vllm-project/vllm/pull/55651) 提议跳过本地 CUDA toolkit 无法运行的 FlashInfer kernels。
- [Issue #39056](https://github.com/vllm-project/vllm/issues/39056)（25 条评论，13👍）— 当 XML 工具调用标记在 `<think>` 内部被输出时，vLLM 0.19 会丢弃 Qwen3.5-35B 的工具调用；这影响使用 qwen3 reasoning/tool parsers 的非流式解析。
- [Issue #54906](https://github.com/vllm-project/vllm/issues/54906) — 在 Qwen3.8 NVFP4 + MTP 下，Model Runner V2 忽略了 `thinking_token_budget`；相关 [RFC #54864](https://github.com/vllm-project/vllm/issues/54864) 提议为 RL rollouts 增加 truncate 模式。

中严重度：

- [Issue #53488](https://github.com/vllm-project/vllm/issues/53488) — 在 MTP speculative decoding + chunked prefill 下，`prompt_logprobs` 会被静默破坏（Qwen3.5 系列；已在两个独立构建版本上复现）。
- [Issue #49896](https://github.com/vllm-project/vllm/issues/49896) — DeepSeek-V4 在 SM12x 上：NaN MQA logits 导致 `top_k_per_row_prefill` 输出未初始化的 smem 索引，进而触发非法内存访问。
- [Issue #51977](https://github.com/vllm-project/vllm/issues/51977) — gpt-oss-120b 使用工具调用时出现 `HarmonyError: unexpected tokens remaining in message header`（v0.26.0）。
- [Issue #53180](https://github.com/vllm-project/vllm/issues/53180) — TurboQuant k8v4 + MTP 在 hybrid GDN 模型上会静默产生退化输出（v0.27.1）。
- [Issue #48745](https://github.com/vllm-project/vllm/issues/48745) — 优雅关闭期间出现虚假的 `EngineDeadError` traceback；[Issue #44249](https://github.com/vllm-project/vllm/issues/44249) — LMCache connector 会在缓存降级时触发 assert，而不是执行 recompute 回退。
- [Issue #44889](https://github.com/vllm-project/vllm/issues/44889) — Gemma-4-31B + DFlash speculator 在 H200 上出现 CUDA 非法内存访问。

正在推进的修复 PR（过去 24 小时内新开/更新）：
- [PR #55647](https://github.com/vllm-project/vllm/pull/55647) — GLM-5.3-Flash 视频占位符时间戳现在取自实际帧采样器（#55644）。
- [PR #55646](https://github.com/vllm-project/vllm/pull/55646) — 阻止 ROCm worker-kill 调度抢占 EngineCore 清理宽限期（#55632）。
- [PR #55642](https://github.com/vllm-project/vllm/pull/55642) — 在 #51826 将 TorchCodec 设为默认并破坏语音任务后，恢复优先使用 soundfile 的自动音频解码。
- [PR #54643](https://github.com/vllm-project/vllm/pull/54643) — 修复 hybrid（Kimi-K3 风格）模型上 `MooncakeStoreConnector` 在保存结束时崩溃的问题。
- [PR #55307](https://github.com/vllm-project/vllm/pull/55307) — 让 `DispatchPooler.for_seq_cls` 遵循 `tok_pooling_type="STEP"`，而不是静默强制使用 `AllPool`。
- [PR #53073](https://github.com/vllm-project/vllm/pull/53073) — 重启后将 KV offload 中 shared-region 创建者所有权解耦。
- 已关闭：[#52907](https://github.com/vllm-project/vllm/issues/52907)（Ray gloo 多节点死锁）、[#53462](https://github.com/vllm-project/vllm/issues/53462)（SM110a 缺少 kernel image）、[#41865](https://github.com/vllm-project/vllm/issues/41865)（FlashInfer GDN JIT 多 worker 死锁）。

## 6. 这对应用开发者意味着什么
- 在 top-k/MTP 并发路径修复之前，不要依赖 temperature=0 来保证最新 sparse-attention/hybrid checkpoint（Qwen3.8-Flash-Next、DeepSeek-V4-Flash）的可复现性。请在运行评测前固定版本，并添加 golden-output 回归测试。
- 推理与工具调用模型（Qwen3.5-35B、Qwen3.8 系列）仍存在正确性缺口：当 XML 出现在 `<think>` 内部时，非流式工具调用可能会被丢弃；在 MRV2 下 `thinking_token_budget` 可能不会强制执行。请使用你自己的 parser flags 进行验证，并尽可能优先使用流式方式。
- 在 FlashInfer scaled-MM 崩溃问题修复之前，运行 FP8 的 Blackwell 工作站/边缘用户（SM120/SM121）应保留 `VLLM_DISABLED_KERNELS=FlashInferFP8ScaledMMLinearKernel` 和 `--enforce-eager` 作为规避配置。
- 音频管线用户应关注 [PR #55642](https://github.com/vllm-project/vllm/pull/55642) 和 [PR #52598](https://github.com/vllm-project/vllm/pull/52598)：默认音频后端正有意迁移到 torchaudio，同时一个恢复 soundfile-first 的回归修复仍待合并。
- hybrid/Mamba 模型上的 KV-cache 复用和 KV-transfer 仍存在不少边界问题（完全相同 prompt 首次重复时的 prefix miss、MooncakeStore hybrid 崩溃）——请为 cache-warmup 开销预留预算，并监控缓存复用指标，尤其是启用 MTP 时。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 文摘 — 2026-09-07

## 1. 今日亮点

今天最实质的进展是 ch-wan 发起的六部分“Config Round 6”重构（[#38046](https://github.com/sgl-project/sglang/pull/38046)–[#38049](https://github.com/sgl-project/sglang/pull/38049)、[#38113](https://github.com/sgl-project/sglang/pull/38113)、[#38114](https://github.com/sgl-project/sglang/pull/38114)），它把服务端配置从大而全的 `ServerArgs` 迁入命名空间作用域的 “bags”——评审正通过一个标有 do-not-merge 的汇总 PR 进行统筹。DeepSeek-V4/FlexKV 与 Apple silicon 方向的工作也有推进：主线 FlexKV/DSV4 对齐（[#31781](https://github.com/sgl-project/sglang/pull/31781)）、统一 KV 外部链接器（[#38269](https://github.com/sgl-project/sglang/pull/38269)），以及更新后的 Apple 服务端重构 RFC（[#32321](https://github.com/sgl-project/sglang/issues/32321)）。CI 健康度持续恢复——跟踪 issue 显示目前有 1 个损坏测试和 8 个不稳定测试，近期已修复 958 个（[#17050](https://github.com/sgl-project/sglang/issues/17050)）。

## 2. 版本发布与破坏性变更

过去 24 小时没有新版本发布。以下几个会改变行为的 PR 正在进行中（尚未合并）：

- **配置语义**：第 6 轮在操作者输入（即 “record”）与解析后实际生效的配置（即 “bags”）之间划出了严格界限；任何检查原始 record 的读取方都会静默地拿到解析前的值。`/server_info` 新增 `resolved_dict()` 视图，展示实际选定的配置（[#38048](https://github.com/sgl-project/sglang/pull/38048)、[#38049](https://github.com/sgl-project/sglang/pull/38049)）。
- **CP v1 弃用（3/5）**：通用预填充 CP v1 运行时与 DSA v1 序列内切分路径将被移除；仅保留通用的输入分片交接路径（[#36228](https://github.com/sgl-project/sglang/pull/36228)）。
- **Rust 前端健康检查语义**：在启动预热完成前，`/health` 和 `/health_generate` 将返回 503，与 Python 前端行为保持一致——这对需要探测就绪状态的编排器很重要（[#37994](https://github.com/sgl-project/sglang/pull/37994)）。

## 3. 新模型与硬件支持

- **Apple silicon**：整体路线图仍保持开放（[#19137](https://github.com/sgl-project/sglang/issues/19137)）。Metal 重构 RFC 已围绕“Torch 持有的 SRT 路径 + 整模型导出到 MLX 的区域”进行刷新，并引用了实现 PR #36164（[#32321](https://github.com/sgl-project/sglang/issues/32321)）；另有一个 Metal 捕获分析器 bug 被单独跟踪（[#30550](https://github.com/sgl-project/sglang/issues/30550)）。Metal 指南中的文档现在推荐使用新的 CUDA 图禁用 CLI 标志（[#38271](https://github.com/sgl-project/sglang/pull/38271)）。
- **T-Head PPU**：已为 ZW810/ZW810E 与 ZW-M890P 提交上游化路线图（[#37519](https://github.com/sgl-project/sglang/issues/37519)）。
- **DeepSeek V4**：FlexKV 主线集成已与生产环境缓存传输栈对齐，新增 C4/C128/C4-indexer/state pools 以及 UnifiedRadixCache 组合（[#31781](https://github.com/sgl-project/sglang/pull/31781)）；统一 KV 现已在 UMBP 与 Mooncake 直接外部链接器中得到支持，并保留 FP4 载荷/缩放分离（[#38269](https://github.com/sgl-project/sglang/pull/38269)）。
- **Intel XPU**：通过上游 `torch_memory_saver` 及 Level Zero VMM 后端提供 Memory-saver 支持（[#29935](https://github.com/sgl-project/sglang/pull/29935)）；DSV4 解码图捕获宽度（[#38272](https://github.com/sgl-project/sglang/pull/38272)）。
- **SM120 上的 GLM-5.3-Flash**：有专门的集成/认证跟踪器（[#37813](https://github.com/sgl-project/sglang/issues/37813)）。

## 4. 性能与优化

- **AMD——GLM DSA 预填充 top-k v2**：将预填充路由到 v2 内核后，top-k 内核成本降低约 73%；在 ISL 70000/OSL 300 下测得每 GPU token 吞吐量提升 +4.9%，中位 TPOT 降低 −3.5%（并发 4–64 的几何平均），GSM8k 0.927（[#37889](https://github.com/sgl-project/sglang/pull/37889)）。
- **Intel XPU——DSV4 解码图**：索引器按完整配置的上下文（最高 262,144）确定分页 gather/logits 缓冲区的大小；可选捕获宽度可避免短对话时将巨大的未使用宽度冻结进重放图（[#38272](https://github.com/sgl-project/sglang/pull/38272)）。
- **Intel XPU——GEMM**：融合 bf16×bf16→fp32 路径已扩展到 CUDA 之外（[#38266](https://github.com/sgl-project/sglang/pull/38266)）。
- **测试覆盖**：`perf_repeat_requests` 现在会校验每一个重复请求，而不再只校验最后一个输出，补上了状态损坏类回归的盲区（[#38185](https://github.com/sgl-project/sglang/pull/38185)）。
- **影响性能的容量 bug**：启用 EAGLE/MTP 时，KV 池分析器会在混合 GDN 模型（Qwen3.6-27B NVFP4）上闲置约 50 GB 显存，导致 token 容量远低于空闲显存本可支撑的水平（[#29857](https://github.com/sgl-project/sglang/issues/29857)）。

## 5. 稳定性与回归问题

按严重程度大致排序：

1. **GLM-5.3-Flash（DSA）HiCache 损坏**——即使未启用投机解码，主机层 KV 回载也会破坏生成结果：在 8×H100/TP8 上表现为工具调用丢失和退化性重复循环（[#38031](https://github.com/sgl-project/sglang/issues/38031)）；该问题与 GLM-5.3-Flash 的其他 bug 一起记录在系列跟踪器中（[#37524](https://github.com/sgl-project/sglang/issues/37524)）。
2. **PP 分离式预填充死锁**——中止风暴会使各阶段的引导队列产生分歧，破坏 P2P 发送/接收调度；issue 中已包含根因分析（RCA）及系列修复（[#34572](https://github.com/sgl-project/sglang/issues/34572)）。
3. **DSPARK + EPLB 崩溃**——同时启用 `--enable-eplb` 与 DSPARK 时，在草稿 CUDA 图捕获期间崩溃，`on_select_experts` 中报 `scatter_add_` 维度不匹配（[#34974](https://github.com/sgl-project/sglang/issues/34974)）。
4. **解码回缩崩溃（HiSparse）**——`HiSparseDSATokenToKVPool` 缺少 `get_cpu_copy`/`load_cpu_copy`，因此在 PD 分离式回缩期间无条件卸载会崩溃；修复 PR 实现了这两个方法（[#33385](https://github.com/sgl-project/sglang/issues/33385)、[#36591](https://github.com/sgl-project/sglang/pull/36591)）。
5. **sm_121 上的 MiniMax-M3 W4A16**——在 DGX Spark 上可正常服务，但 Triton MiniMaxSparse 路径会输出 token id 0（全 NUL 输出）；相同权重在 vLLM 上正确（[#38143](https://github.com/sgl-project/sglang/issues/38143)）。
6. **投机接受率衰减异常**——在 Qwen3.8-Flash-Next 上，NEXTN/MTP 草稿接受率会随服务器运行时间增长而衰减至约 0，重启后完全恢复（[#37326](https://github.com/sgl-project/sglang/issues/37326)）。
7. **API 不一致**——`/v1/responses` 在流式事件中以 float 返回 `created_at`，但在非流式响应中以 int 返回（[#34716](https://github.com/sgl-project/sglang/issues/34716)）。
8. **模型语义缺陷**——DeepSeek-V4-Flash 的 `reasoning_effort` 映射偏差一级：“high” 实际是空操作，厂商的 “max” 无法触达（[#33185](https://github.com/sgl-project/sglang/issues/33185)）。
9. **GLM-5.3 DPC 崩溃**——新提交的问题，暂无复现细节（[#38207](https://github.com/sgl-project/sglang/issues/38207)）。
10. **值得注意的修复**——gloo 在缓存预取进度检测中的同组死锁（[#38270](https://github.com/sgl-project/sglang/pull/38270)）；AMD Lean-Attention 不确定性修复，在 batch 变化时仍保持 logprobs 逐位精确（[#37740](https://github.com/sgl-project/sglang/pull/37740)）。

一批较老的 issue 已因不活跃被自动关闭，其中仍有一些有参考价值的报告：截断时非流式响应泄漏原始 `<tool_call>` 标记（[#30480](https://github.com/sgl-project/sglang/issues/30480)）、若干投机解码崩溃（[#30549](https://github.com/sgl-project/sglang/issues/30549)、[#30555](https://github.com/sgl-project/sglang/issues/30555)），以及 EPD RFC（[#24945](https://github.com/sgl-project/sglang/issues/24945)）。

## 6. 这对应用开发者意味着什么

- **如果你使用 HiCache/DSA 主机层卸载来服务 GLM-5.3-Flash，在信任生成结果前请先在高负载下验证工具调用行为**——该损坏缺陷（[#38031](https://github.com/sgl-project/sglang/issues/38031)）表现为静默的语义错误（工具调用丢失、重复循环），而不是崩溃。请固定一个已知正常的版本，并关注系列跟踪器（[#37524](https://github.com/sgl-project/sglang/issues/37524)）。
- **投机解码仍是最具风险的功能领域**：需警惕 DSPARK/EPLB 崩溃（[#34974](https://github.com/sgl-project/sglang/issues/34974)），以及 NEXTN/MTP 端点上草稿接受率的缓慢衰减（[#37326](https://github.com/sgl-project/sglang/issues/37326)）。重启可以“修复”后一个问题，这说明在长期运行部署依赖投机吞吐之前，应考虑按运行时长进行定期回收，或先展开调查。
- **API 契约注意事项**：不要假设 `created_at` 在流式与非流式 `/v1/responses` 中的类型保持稳定（[#34716](https://github.com/sgl-project/sglang/issues/34716)）；`reasoning_effort` 映射缺陷意味着 “high” 可能会静默降低 DeepSeek-V4-Flash 的推理质量（[#33185](https://github.com/sgl-project/sglang/issues/33185)）。
- **为配置系统变动做好准备**：第 6 轮合入后，解析 `ServerArgs`、或假定未设置字段仍保持缺失的工具都需要更新——record 与 bags 的区分让有效配置变得可查询，但只能通过新的预期路径查询（[#38114](https://github.com/sgl-project/sglang/pull/38114)）。
- **硬件路线图观察**：Apple Metal 仍处于预生产阶段，并在积极招募贡献者（[#19137](https://github.com/sgl-project/sglang/issues/19137)）；T-Head PPU 仍处于路线图阶段（[#37519](https://github.com/sgl-project/sglang/issues/37519)）；MiniMax-M3 W4A16 在 Blackwell 级消费/边缘 GPU（sm_121）上仍不正确（[#38143](https://github.com/sgl-project/sglang/issues/38143)）。
- **CI 信号终于接近全绿了**：近期修复了 958 个测试，仅剩 1 个损坏和 8 个不稳定测试（[#17050](https://github.com/sgl-project/sglang/issues/17050)）——现在是在采用更新提交之前验证上游的合理窗口。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-09-07

## 今日亮点

今天在 b10821–b10830 范围内共发布了 9 个构建，重点包括：新增的 `--fuse-qkv` GGUF 转换标志（[#22780](https://github.com/ggml-org/llama.cpp/pull/22780)）、Gated Delta Net（GDN）归一化的正确性修复（[#28068](https://github.com/ggml-org/llama.cpp/pull/28068)），以及 Spark2.5 模型支持（[#27868](https://github.com/ggml-org/llama.cpp/pull/27868)）。工程方面，最热门的议题是投机解码的正确性（[#27694](https://github.com/ggml-org/llama.cpp/pull/27694)、[#28232](https://github.com/ggml-org/llama.cpp/pull/28232)），以及一波激进的 Vulkan kernel 工作（stream-k、int8 coopmat1、更大的 MoE 专家上限）。两个长期推进的三元量化（TQ1_0/TQ2_0）Vulkan PR 也终于收尾（[#19743](https://github.com/ggml-org/llama.cpp/pull/19743)、[#27765](https://github.com/ggml-org/llama.cpp/pull/27765)）。

## 发布与破坏性变更

- **b10830** — `convert`：新增 `--fuse-qkv` 标志，用于在 HF 转 GGUF 时将 Q/K/V 融合为 QKV（[#22780](https://github.com/ggml-org/llama.cpp/pull/22780)）。
- **b10829** — GDN 的 q/k 归一化从 `max` 改为 `rsqrt`，与 flash-linear-attention 的 `l2norm(x) = x * rsqrt(sum(x*x) + eps)` 对齐。这是数值行为上的变更：基于 GDN 的混合/线性注意力模型输出会与早期构建不同，现在应与参考实现一致（[#28068](https://github.com/ggml-org/llama.cpp/pull/28068)）。
- **b10823** — `common`：新增 `--log-jsonl` 选项，用于结构化 JSONL 日志输出（[#28437](https://github.com/ggml-org/llama.cpp/pull/28437)）。
- **b10822** — UI 资源现在直接通过 CMake 内嵌，移除了构建期的 C++ 辅助程序与外部 gzip 依赖，从而简化了内置 Web UI 的 `llama-server` 交叉编译（[#28445](https://github.com/ggml-org/llama.cpp/pull/28445)）。

没有宣布任何显式的破坏性 API/配置变更，但 b10829 中的 GDN 归一化修复会静默改变受影响模型的生成输出。

## 新模型与硬件支持

- **Spark2.5（Spark2_5ForCausalLM）** — 已在 b10828 合并；该 PR 最初以“Spark3”提交，后来改名为与 `spark2_5` 保持一致（[#27868](https://github.com/ggml-org/llama.cpp/pull/27868)）。
- **XingChen4（TeleAI）** — 开放中的 PR 为中国电信 AI 模型家族添加支持（[#28156](https://github.com/ggml-org/llama.cpp/pull/28156)）。
- **Qwen4exp / Qwen3.8-Flash-Next** — 开放中的 PR 为 unsloth 布局的 MTP 检查点增加了仅 draft-head 的 GGUF 支持，并修复了草稿加载器的一个 bug：此前会使用目标模型路径，而不是 `-md` 指定的路径（[#28097](https://github.com/ggml-org/llama.cpp/pull/28097)）。
- **Vulkan 三元量化格式** — TQ1_0/TQ2_0（1.6875 bpw）的相关 PR 已关闭，表明 Vulkan 后端在三元量化（MUL_MAT、mat-vec、get_rows、dequant）上的缺口已经补齐；已用 BitNet b1.58 模型和 `test-backend-ops` 验证（[#19743](https://github.com/ggml-org/llama.cpp/pull/19743)、[#27765](https://github.com/ggml-org/llama.cpp/pull/27765)）。
- **CUDA/RDNA4** — 开放中的 PR 为 RDNA4 启用了 head size 256 的 WMMA flash attention，解除了 256 宽注意力模型（Qwen3.5/Qwen3-Next）的阻塞；这些模型目前会回退到更慢的 tile kernel（[#28529](https://github.com/ggml-org/llama.cpp/pull/28529)）。
- **OpenVINO** — 通过从基础 `Constant` 提供权重视图，而不是注册动态参数，修复了量化权重上的 GET_ROWS 失败（[#28381](https://github.com/ggml-org/llama.cpp/pull/28381)）。
- **OpenCL / SYCL** — conv2d kernel 现在可以处理非连续输入（[#28503](https://github.com/ggml-org/llama.cpp/pull/28503)）；针对 level-zero “get mem error” 的 SYCL 内存探测修复已合入（[#28227](https://github.com/ggml-org/llama.cpp/pull/28227)）。

## 性能与优化

- **b10821** — Metal：为 M2 Max 补充了剩余的 flash attention 向量调优（[#28458](https://github.com/ggml-org/llama.cpp/pull/28458)）。
- **b10827** — OpenCL：`mul_mat` 现在会为 q4_K/q5_K 选择正确的权重 pack，从而避开通用路径（[#28402](https://github.com/ggml-org/llama.cpp/pull/28402)）。
- **面向 gfx1201 的 CUDA/HIP flash attention** — RDNA4/R9700 PRO 的 FA 调优，外加一处 head size 256 的 bug 修复；作者称在这项工作之前，Qwen3.8 27B 的长上下文 prefill 表现“糟糕透顶”（[#28102](https://github.com/ggml-org/llama.cpp/pull/28102)）。
- **Vulkan int8 coopmat1 MMQ** — 面向 AMD RDNA3/RDNA4 的新实现，覆盖 q4_0…q6_k、q3_k–q6_k、mxfp4、nvfp4 和 iq4_nl；据报告在 Strix Halo 上 prompt 处理性能有所提升（[#27952](https://github.com/ggml-org/llama.cpp/pull/27952)）。
- **CUDA MoE/mat-vec** — 添加了 Spark 模式预取（prefetch），并将 Q4_K/Q5_K 的 scale 解包改为无分支实现，不再在 MMVQ 中按列重复执行；改善了 batch size > 1 的性能（[#26705](https://github.com/ggml-org/llama.cpp/pull/26705)）。
- **Vulkan stream-k MUL_MAT** — 已在 scalar/cm1/cm2 路径上实现，目前对 cm2 启用；通过一次 resolve pass 把 256 元素的 K 块均衡地分布到所有 SM（[#28528](https://github.com/ggml-org/llama.cpp/pull/28528)）。
- **Vulkan MoE** — coopmat1 路径会为空 workgroup 跳过不必要的工作（[#25483](https://github.com/ggml-org/llama.cpp/pull/25483)）；面向宽 MoE 模型，专家 row-id 提升上限从 256 个专家提高到 512 个（[#28501](https://github.com/ggml-org/llama.cpp/pull/28501)）。
- **ROCm reductions** — SUM/MEAN 现在使用 hipCUB `DeviceReduce`，而不是单行 `sum_rows` 回退路径（[#27936](https://github.com/ggml-org/llama.cpp/pull/27936)）。
- **CPU/Windows** — bug 报告：MSVC 构建无法检测/使用 AVX-VNNI，导致 CPU 性能没有被充分挖掘（[#28295](https://github.com/ggml-org/llama.cpp/issues/28295)）。

## 稳定性与回归问题

大致按严重程度排序；除另行说明外，以下均为未关闭的问题。

1. **Qwen3.5 在思考块内输出工具调用** — 模型会在其思考块中输出 XML 工具调用，然后停止生成；讨论活跃且热度很高（60 条评论，17 👍）。这是聊天解析/eval 层面的问题，影响 agentic 场景（[#20837](https://github.com/ggml-org/llama.cpp/issues/20837)）。
2. **CUDA flash attention 致命错误（`fattn.cu:579`）** — 两起未关闭的报告：llama-server 在 Gemma 4 31B 启用 MTP 与 `-sm tensor` 时，编辑系统消息后崩溃（[#24440](https://github.com/ggml-org/llama.cpp/issues/24440)）；另有通用的 `ggml-cuda/fattn.cu:579` 崩溃（[#24324](https://github.com/ggml-org/llama.cpp/issues/24324)）。目前还没有看到关联的修复 PR。
3. **量化目标上的投机解码与普通贪心不一致** — draft-MTP/draft-dspark 可复现该问题；ngram 投机则与普通贪心一致。修复正在推进：概率化 drafter + 拒绝采样（[#25618](https://github.com/ggml-org/llama.cpp/issues/25618)、[PR #27694](https://github.com/ggml-org/llama.cpp/pull/27694)）。
4. **投机结果未在 EOG 处截断** — 服务器会回滚并暴露越过 EOG 的草稿 token；修复 PR 已开放（[#28232](https://github.com/ggml-org/llama.cpp/pull/28232)）。
5. **`--kv-unified` 的 prompt 处理性能骤降** — 在 `-np 2` 下，单 GPU 上从第二个长请求开始，prompt 处理性能下降 42–54%。根因已定位：CUDA/HIP FA kernel 只跳过 KQ mask 的尾部，而没有跳过中间全 `-INF` 的块（[#28495](https://github.com/ggml-org/llama.cpp/issues/28495)）。
6. **混合/线性注意力模型的上下文失效** — Qwen3.5-hybrid 在上下文超过约 130k 后，在 CUDA 和 CPU 上都会静默地立刻输出 EOS；怀疑与 DeltaNet 循环状态深度 × 层数的退化有关（[#27756](https://github.com/ggml-org/llama.cpp/issues/27756)）。
7. **MTP 长会话输出损坏** — Qwen3.6 27B 启用 MTP 后，在长会话后会重复输出 `////`（[#23577](https://github.com/ggml-org/llama.cpp/issues/23577)）。
8. **后端不稳定报告** — SYCL 在第二个 prompt 上输出乱码（[#26845](https://github.com/ggml-org/llama.cpp/issues/26845)）；Intel iGPU 上的 Vulkan 中，内核看门狗会静默取消已排队的提交，embedding 在无报错的情况下崩溃（[#27634](https://github.com/ggml-org/llama.cpp/issues/27634)）；SYCL/OpenCL 多 GPU 在未实现的 P2P 上崩溃（[#27168](https://github.com/ggml-org/llama.cpp/issues/27168)）。
9. **服务器正确性** — `tool_choice: "required"` 在 `supports_preserve_reasoning: true` 的 Jinja 模板上会被接受，但不会被强制执行（[#27217](https://github.com/ggml-org/llama.cpp/issues/27217)）；llama-ui 桌面端无法打开推理级别菜单（[#27981](https://github.com/ggml-org/llama.cpp/issues/27981)）。
10. **已在今天的构建中修复** — grammar 最大重复阈值（[#28469](https://github.com/ggml-org/llama.cpp/pull/28469)）、mmid/mmf 中的 CUDA 竞态（[#28475](https://github.com/ggml-org/llama.cpp/pull/28475)），以及 GDN 归一化 bug（[#28068](https://github.com/ggml-org/llama.cpp/pull/28068)）。另外，json-schema-to-grammar 中嵌套 `maxLength >= 2000` 会生成无法解析的 GBNF 的问题也已关闭（[#25746](https://github.com/ggml-org/llama.cpp/issues/25746)）。

## 对应用开发者意味着什么

- **Agent/工具调用技术栈应固定版本并仔细测试。** Issue [#20837](https://github.com/ggml-org/llama.cpp/issues/20837) 显示，当前的 Qwen3.5 系列模型可能会把工具调用埋在思考块里，然后停止生成；请在生产环境加入超时/停滞检测，并对工具调用解析做校验。
- **`tool_choice: "required"` 还不能作为硬性保证**，在保留推理的模板上尤其如此（[#27217](https://github.com/ggml-org/llama.cpp/issues/27217)）——如果你的应用依赖它，请在应用层强制执行工具调用结构。
- **投机解码正在被重构。** 如果你使用 `draft-mtp`/`draft-dspark`，或向终端用户开放 MTP，请注意概率化 drafter（[#27694](https://github.com/ggml-org/llama.cpp/pull/27694)）与 EOG 截断（[#28232](https://github.com/ggml-org/llama.cpp/pull/28232)）会带来的行为变化。目前在量化目标上，贪心输出可能与未启用投机解码的运行不一致。
- **多槽位 `--kv-unified` 服务有一个需要重视的吞吐量注意事项。** 42–54% 的 prompt 处理性能下降（[#28495](https://github.com/ggml-org/llama.cpp/issues/28495)）对共享 KV 部署影响明显；在持续负载下依赖 unified KV 之前，请留意内核修复。
- **运维便利性提升：** `--log-jsonl`（[#28437](https://github.com/ggml-org/llama.cpp/pull/28437)）为日志采集管道提供结构化日志；CMake 内嵌 UI 简化了交叉编译 `llama-server` 的构建（[#28445](https://github.com/ggml-org/llama.cpp/pull/28445)）。
- **前瞻：** prefill/decode 分离已是路线图中的明确项目（[#21266](https://github.com/ggml-org/llama.cpp/issues/21266)）。模型方面，已合并的 Spark2.5 支持与进行中的 XingChen4/Qwen4exp-draft 工作表明，中国开放权重架构与混合注意力变体的发展势头仍在持续。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-09-07

## 今日亮点

过去 24 小时内没有发布任何版本。目前最有实质进展的开放工作是 PR [#16998](https://github.com/ollama/ollama/pull/16998)（新增一个兼容 Prometheus 的 `/metrics` 端点）和 PR [#18278](https://github.com/ollama/ollama/pull/18278)（将模型名称长度上限从 80 个字符提高到 96 个字符，以对齐 Hugging Face）。在问题追踪器中，占主导的是围绕 MLX、Vulkan、Blackwell/FlashAttention 以及云端模型可靠性的稳定性报告；长期未解决的许可证通知合规问题 [#3185](https://github.com/ollama/ollama/issues/3185) 仍然是参与度最高的非运行时问题，获得 272 👍。

## 发布与破坏性变更

过去 24 小时内没有任何版本发布，也没有新的 API/配置变更或迁移说明需要报告。

## 新模型与硬件支持

过去 24 小时内没有新增模型、后端或量化支持。相关开放/模型周边工作：

- [#17195](https://github.com/ollama/ollama/pull/17195) — PR：通过将 `<|user|>` 注册为 EOT 令牌来修复旧版 `glmocr` GGUF 模型的失控输出问题。
- [#18276](https://github.com/ollama/ollama/issues/18276) — qwen3moe 在 Blackwell `sm_120` 上会在 FlashAttention 自动启用时的预热阶段崩溃；目前还不是一个稳定的配置。
- [#18269](https://github.com/ollama/ollama/issues/18269) — `muse-glimmer:30b-mlx` 的 NVFP4 MLX 变体在 32 GB M4 Air 上反复卡在 `Stopping...` 状态。

## 性能与优化

过去 24 小时内没有合并任何优化 PR。以下开放事项具有实际影响：

- [#18267](https://github.com/ollama/ollama/issues/18267) — MLX 运行时的前缀缓存恢复被截断为 8192 token 的整数倍，导致每次冷提示后最多有 8191 个 token 需要重新预填充。在智能体工作负载上，这会造成固定的 **17–27 秒重新预填充开销**。
- [#18271](https://github.com/ollama/ollama/pull/18271) — PR：将渲染器消息分隔符发送至 llama-server，以便在用户轮次边界处放置上下文检查点，从而提高 Ollama 使用 `/completion` 时的缓存复用率。
- [#16998](https://github.com/ollama/ollama/pull/16998) — PR：新增由 `OLLAMA_METRICS=1` 启用的 Prometheus `/metrics` 端点，包含调度器计量指标、请求计数器以及按模型/token 统计的指标。这是功能请求 [#3144](https://github.com/ollama/ollama/issues/3144) 的实现路径。

## 稳定性与回归问题

按严重程度排序：

1. [#18275](https://github.com/ollama/ollama/issues/18275) — gemma4 的工具调用解析器无法解析 `BEGIN_ARG`/`END_ARG` 语法；解析修复失败后，模型会陷入退化的循环生成直到令牌上限，最终返回 HTTP 200 但没有任何可用内容。目前没有修复 PR。
2. [#18276](https://github.com/ollama/ollama/issues/18276) — qwen3moe + Blackwell `sm_120`：自动启用的 FlashAttention 在预热时导致 llama-server 崩溃，报错 `CUDA error: shared object initialization failed`；即使在全部 49 层都已成功装入内存后仍然如此。目前没有修复 PR。
3. [#18193](https://github.com/ollama/ollama/issues/18193) — 云端模型 `glm-5.3:cloud` 可能会陷入无休止的推理，导致 OpenCode/ZCode 中的任务中止，而官方的 Z.AI API 工作正常。目前没有修复 PR。
4. [#16845](https://github.com/ollama/ollama/issues/16845) — 云端模型 `kimi-k2.6:cloud` 反复出现极端延迟（每个请求 10 分钟以上），并在 `/api/chat` 的流式响应中返回 `INTERNAL_ERROR`。目前没有修复 PR。
5. [#18272](https://github.com/ollama/ollama/issues/18272) — Vulkan 后端在 AMD iGPU 上加载 66 GB 模型时失败并报错 “Not enough memory for command submission”；这是自 v0.32.12 以来的回归，而 v0.32.9 可以正常工作。目前没有修复 PR。
6. [#18073](https://github.com/ollama/ollama/issues/18073) — 新的 Claude Desktop 集成在 0.33.1 版本中无法工作。目前没有修复 PR。
7. [#18269](https://github.com/ollama/ollama/issues/18269) — MLX `muse-glimmer:30b-mlx` NVFP4 模型反复卡在 `Stopping...` 状态并触发看门狗。目前没有修复 PR。
8. [#18274](https://github.com/ollama/ollama/issues/18274) — 模型名称校验上限为 80 个字符，导致合法的长名称 `hf.co` 模型无法拉取。修复 PR [#18278](https://github.com/ollama/ollama/pull/18278) 已开启，拟将上限提高到 96 个字符。
9. [#3185](https://github.com/ollama/ollama/issues/3185) — Ollama 发布产物没有包含 llama.cpp 等静态链接依赖的 MIT 许可证/版权声明。这是法律/合规问题而非运行时回归；目前没有关联的修复 PR。

## 对应用开发者的影响

- **智能体工具调用用户**：对于 gemma4 模型，不要把 HTTP 200 当作成功。畸形工具调用语法可能产生空的、达到令牌上限的响应。请加入超时、令牌上限和回退方案。参见 [#18275](https://github.com/ollama/ollama/issues/18275)。
- **云端模型消费者**：`glm-5.3:cloud` 和 `kimi-k2.6:cloud` 目前存在未解决的可靠性问题。如果你的应用依赖 Ollama Cloud，请设置较短的客户端超时、重试机制，以及直达厂商原生 API 的回退路径。参见 [#18193](https://github.com/ollama/ollama/issues/18193) 和 [#16845](https://github.com/ollama/ollama/issues/16845)。
- **Apple Silicon 智能体工作负载**：注意 MLX 前缀缓存对齐问题；冷缓存恢复可能每次冷提示后都会产生固定的 17–27 秒重新预填充开销。参见 [#18267](https://github.com/ollama/ollama/issues/18267)。
- **硬件部署目标平台**：对于 Vulkan/AMD iGPU 上运行大模型的部署，请避免使用 v0.32.12 及更新版本；在部署到 Blackwell `sm_120` 笔记本电脑之前请仔细测试。参见 [#18272](https://github.com/ollama/ollama/issues/18272) 和 [#18276](https://github.com/ollama/ollama/issues/18276)。
- **分发方**：如果你正在再分发 Ollama 二进制文件，请在发布产物合规问题修复前确认已履行许可证通知义务。参见 [#3185](https://github.com/ollama/ollama/issues/3185)。
- **Hugging Face 模型拉取**：在依赖较长的 Hugging Face 仓库名称之前，请等待 PR [#18278](https://github.com/ollama/ollama/pull/18278) 合并；在此之前，请将本地模型名称控制在不超过 80 个字符。
- **Claude Desktop 集成用户**：v0.33.1 的集成回归确实存在；如果你依赖该集成，请固定到已知正常的版本。参见 [#18073](https://github.com/ollama/ollama/issues/18073)。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-09-07

## 1. 今日亮点

LiteLLM 发布了 **v1.100.0**，使 cosign 验证的 Docker 镜像成为所有发布的强制要求。当前最活跃的工程方向是**路由执行的 Rust SDK 迁移**（[#40070](https://github.com/BerriAI/litellm/pull/40070)），同时也在并行推进回调生命周期与 CI 稳定性相关的工作。计费正确性问题主导了 bug 跟踪器：`/v1/messages` 上因预算超限导致的错误锁定、Anthropic 缓存读取 token 被重复计费、以及流式 `/v1/responses` 请求未被计费，在过去 24 小时内都很活跃；同时若干 Vertex AI/实时定价修复正在审查中。

## 2. 发布与破坏性变更

- **[v1.100.0](https://github.com/BerriAI/litellm/releases/tag/v1.100.0)** — 所有 LiteLLM Docker 镜像现在均使用 [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0) 引入的密钥进行 cosign 签名。运维人员应在拉取/部署流水线中添加签名验证。发布说明中没有提及 API/配置的破坏性变更。

## 3. 新增模型与硬件支持

- **Synthorai** 已作为 OpenAI 兼容提供商添加（[PR #38288](https://github.com/BerriAI/litellm/pull/38288)，已关闭）。
- **已报告的缺口：** DashScope Qwen 3.6/3.7 仍不在定价映射表中，因此 `dashscope/*` 路由记录 $0 成本（[#29922](https://github.com/BerriAI/litellm/issues/29922)）；Qwen3.8-27B-FP8 无法通过配置启用 `reasoning_effort`（[#37359](https://github.com/BerriAI/litellm/issues/37359)）。

## 4. 性能与优化

- **[PR #40070](https://github.com/BerriAI/litellm/pull/40070)** — refactor(rust)：路由执行正在迁移到 Rust SDK；新增**保留式回调调用（retained callback invocation）**，以便在 Rust 取代 Python 调度器后，Python 回调/日志器/防护栏（guardrails）仍能收到原始的共享对象，而不是按值复制的副本。
- **[PR #40073](https://github.com/BerriAI/litellm/pull/40073)** — CI 修复：将保留式回调测试串行化，以消除 Rust `release wheel` 任务上约 20% 的 `gc.collect` 竞争条件。
- **[PR #37075](https://github.com/BerriAI/litellm/pull/37075)** — `/vertex_ai/live` 透传现在按模态 token（audio/image/camera）计费，而不再仅按文本计费；实测真实会话中的少计费幅度为 **9.8x–54.5x**。
- **[PR #36887](https://github.com/BerriAI/litellm/pull/36887)** — 图像/视频输入 token 现在会通过 Responses usage 桥接传递，因此按模态划分的输入费率能够真正生效。

## 5. 稳定性与回归问题

按严重程度排序：

1. **错误的预算锁定** — [#40050](https://github.com/BerriAI/litellm/issues/40050)：`max_budget=100` 的密钥在强制校验成本为 111.29 时返回 429 “Budget has been exceeded”，而记录到的消费远低于该值；这会阻断所有 Claude Code 的 `/v1/messages` 流量。尚无修复 PR。
2. **缓存读取重复计费** — [#40006](https://github.com/BerriAI/litellm/issues/40006)：`custom_cost_per_token` 配合 `cache_read_input_token_cost` 时，Anthropic 缓存读取 token 会被计费两次。
3. **流式 Responses 始终未被计费** — [#29913](https://github.com/BerriAI/litellm/issues/29913)：流式 `/v1/responses` 会导致成功日志记录器崩溃（`'dict' object has no attribute 'usage'`）；不会写入任何消费日志行。
4. **缓存注入无效 + 工具调用无限循环** — [#29810](https://github.com/BerriAI/litellm/issues/29810)：`/v1/responses` 上的 `cache_control_injection_points` 既静默地不生效，**又**会触发确定性的 Claude 工具调用循环，直到达到 MaxTurns。
5. **Bedrock 文件清理故障** — [#39715](https://github.com/BerriAI/litellm/issues/39715)：`DELETE /v1/files/{file_id}` 返回 500（`BedrockFilesConfig does not support file deletion`）。
6. **配置重载 404** — [#30772](https://github.com/BerriAI/litellm/issues/30772)：在 `litellm-database:main-stable` 上 `POST /config/reload` 返回 404。
7. **零成本消费日志** — [#35691](https://github.com/BerriAI/litellm/issues/35691)：不在内置成本映射中的自定义模型会记录 `total_cost = 0`，即使 `usage.estimated_cost` 是正确的。
8. **UI 时区偏移** — [#39979](https://github.com/BerriAI/litellm/issues/39979)：请求日志的日期范围筛选器将本地选择器时间当作 UTC 处理。
9. **透传钩子缺口** — [#28444](https://github.com/BerriAI/litellm/issues/28444)：透传端点不会触发 Post-API 钩子。
10. **图像编辑失败** — [#26552](https://github.com/BerriAI/litellm/issues/26552)：带 `mask` 的 `/v1/images/edits` 请求失败，错误为 “Attempted to access streaming request content…” 。

**进行中的修复 PR：** DeepSeek 在 CCR 转换后丢弃 `stream_options`（[#40075](https://github.com/BerriAI/litellm/pull/40075)）；向 Vertex `generateContent` 发送驼峰式 `systemInstruction`（[#37030](https://github.com/BerriAI/litellm/pull/37030)）；为 Vertex 规范化 JSON Schema 中元组形式的 `items`（[#36934](https://github.com/BerriAI/litellm/pull/36934)）；将部署级定价覆盖应用于实时会话（[#36958](https://github.com/BerriAI/litellm/pull/36958)）；拒绝 Azure MAI 图像生成中的 `n>1` 和不支持的尺寸，而不是继续转发（[#40074](https://github.com/BerriAI/litellm/pull/40074)）。

## 6. 对应用开发者的意义

- **在部署流水线中验证 v1.100.0 的镜像签名**；这已经成为发布流程中不可省略的一环。
- **受预算限制的密钥（尤其是 Claude Code）：** #40050 在强制成本超过预算约 111% 时可能会硬性锁定密钥，因此在向设置了预算上限的租户发布 v1.100.0 之前，请监控是否有意外出现的 429。
- **流式 `/v1/responses` 请求（[#29913](https://github.com/BerriAI/litellm/issues/29913)）和自定义定价模型（[#35691](https://github.com/BerriAI/litellm/issues/35691)）的消费/分摊数据并不可靠**——在依赖这些路径的数据前，请先验证计费是否准确。
- **Anthropic 缓存定价：** 如果你在 `custom_cost_per_token` 中设置了缓存读取费率，请审计账单——#40006 会重复计算。
- **暂时避免在 `/v1/responses` 上配合 agent SDK 使用 `cache_control_injection_points`**（[#29810](https://github.com/BerriAI/litellm/issues/29810)）；工具调用循环会在达到 MaxTurns 前消耗大量 token。
- **关注 Rust 路由执行迁移**（[#40070](https://github.com/BerriAI/litellm/pull/40070)）：回调/日志器的行为应该保持一致，但任何依赖对象同一性或共享修改语义的自定义 Python 回调，在该迁移落地后都值得添加回归测试。
- **Realtime/Vertex 用户：** 正在审查中的定价修复（[#37075](https://github.com/BerriAI/litellm/pull/37075)、[#36958](https://github.com/BerriAI/litellm/pull/36958)、[#36887](https://github.com/BerriAI/litellm/pull/36887)）将显著改变实时会话和多模态 Responses 请求的费用——合并后预计费用会更高且更准确。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-09-07

## 今日要点
Unsloth Studio 服务是主要关注点：多个 PR 添加了 KV 缓存抢占，让并行聊天槽位共享一个上下文池而不是互相逐出（[#10301](https://github.com/unslothai/unsloth/pull/10301), [#10358](https://github.com/unslothai/unsloth/pull/10358)），并将两台 DGX Spark 配对为异步副本路由服务拓扑（[#10323](https://github.com/unslothai/unsloth/pull/10323)）。一个高严重性正确性 bug 已关闭：在 Qwen3.5-2B 和 gemma-4-E2B 上，批量贪心解码与逐个解码结果不一致（[#9708](https://github.com/unslothai/unsloth/issues/9708)）。此外，一份零差异报告记录了 Blackwell SM120/SM121 部件上的 NVFP4 内核选择结果（[#10417](https://github.com/unslothai/unsloth/pull/10417)）。

## 版本发布与破坏性变更
过去 24 小时内没有发布新版本。正在推进的 PR 一旦合并将改变 Studio 行为：

- Studio 将不再提供预设的管理员密码，而是改为签发一次性设置令牌；首次登录的“当前密码”流程保持不变（[#10387](https://github.com/unslothai/unsloth/pull/10387)）。
- uv 缓存处理在 `install.sh`、`install.ps1`、`unsloth studio update` 和后端之间保持一致；当内容检查存在歧义时，安装程序会记录它们使用的缓存（[#10386](https://github.com/unslothai/unsloth/pull/10386), [#10410](https://github.com/unslothai/unsloth/pull/10410)）。
- torchcodec 兼容性守卫将覆盖 torch 2.11，并按 torch 次版本固定，同时笔记本验证器有大规模 shell 解析重写（[#7474](https://github.com/unslothai/unsloth/pull/7474), [#10414](https://github.com/unslothai/unsloth/pull/10414)）。
- `python`/`terminal` 工具结果的截断上限现在可以按每次调用设置，而不只是使用安装级别的 `UNSLOTH_TOOL_RESULT_MAX_CHARS`（[#10398](https://github.com/unslothai/unsloth/pull/10398)）。

## 新模型与硬件支持
没有宣布新模型发布。值得注意的模型/后端动态：

- Qwen3.6-35B-A3B MLX API 调用通过 base64 和 URL 两种方式均失败（[#10389](https://github.com/unslothai/unsloth/issues/10389)）；Qwen3.8-27B-Uncensored-MLX 在 `tokenizer_utils.py:229-237` 中崩溃（[#10385](https://github.com/unslothai/unsloth/issues/10385)）。
- Blackwell 消费级/工作站 GPU（SM120/SM121：RTX 50 系列、RTX PRO 6000、DGX Spark）上的 NVFP4 量化模型获得了一次完整的内核选择调研，结果已记录，未改动代码（[#10417](https://github.com/unslothai/unsloth/pull/10417)）。
- AMD ROCm 的缺口仍然存在：Wan2.2 TI2V 视频生成在 RX 9060 XT 上会 OOM，因为没有可用的融合注意力内核，只能使用 PyTorch SDPA 数学回退（[#10415](https://github.com/unslothai/unsloth/issues/10415)）；ROCm 用户还报告 Studio 在 Radeon PRO W7900/W7500 上忽略“No RAM Offload”复选框（[#10341](https://github.com/unslothai/unsloth/issues/10341)）。
- torchcodec 支持正按 torch 次版本固定，并将 torch 2.11 加入兼容性守卫（[#7474](https://github.com/unslothai/unsloth/pull/7474)）。

## 性能与优化
- **并行聊天的 KV 抢占**：Studio 目前以 `--parallel N --kv-unified` 启动 llama-server，每个槽位都认为自己拥有完整上下文，但服务器只检查 `prompt_tokens < slot.n_ctx`——因此并行聊天会互相逐出。[#10301](https://github.com/unslothai/unsloth/pull/10301) 添加了 Studio 侧的抢占，让一个共享 KV 缓存服务所有聊天；[#10358](https://github.com/unslothai/unsloth/pull/10358) 则交由 llama-server 自己的 `--preempt-ram` 槽位停放（unslothai/llama.cpp#184/#190）处理，使聊天可以在使用完整上下文窗口的同时将数据卸载到主机内存。
- **双 Spark 编排器**：当 GGUF 在一对 DGX Spark 上加载时，后端通过 `spark_cluster.recommend_topology` 在三种拓扑中选择，并通过异步副本路由器提供服务（[#10323](https://github.com/unslothai/unsloth/pull/10323)）。
- **吞吐量遥测修复**：对来自 Strix Halo 日志包中 48,216 条 `engine_stats` 记录的分析显示，几乎整个生成过程上报的速率均为 0 tok/s，还有两次读数达到硬件不可能达到的速率；该 PR 使上报数字变为引擎实际可能达到的水平（[#10384](https://github.com/unslothai/unsloth/pull/10384)）。
- **推测解码插桩**：新增的 `unsloth/spec_decoding` 工具用于测量草稿/目标接受率，帮助团队判断某个草稿模型是否值得部署（[#10401](https://github.com/unslothai/unsloth/issues/10401), [#10416](https://github.com/unslothai/unsloth/pull/10416)）。
- **Kaggle CI 成本**：改为分派内核并在事后收集结果，将 T4 冒烟测试作业占用的 runner 时间从约 41.5 分钟缩短到约 4 分钟的有效构建/推送工作（[#10183](https://github.com/unslothai/unsloth/pull/10183)）。

## 稳定性与回归
按严重程度排序：

1. **[已修复]** 在 LoRA 微调的 Qwen3.5-2B 和 gemma-4-E2B 上（T4，左填充），批量贪心生成产生了与逐个生成不同的文本；在两个模型上复现后关闭（[#9708](https://github.com/unslothai/unsloth/issues/9708)）。
2. **[修复 PR]** Windows 智能应用控制 / 代码完整性阻止可能导致 Studio 启动后无法加载模型，并出现 `llama-server.exe — Bad Image` 错误；该 PR 增加了运行时探测和 CI 包签名审计（[#10408](https://github.com/unslothai/unsloth/pull/10408)）。
3. **[未解决]** Qwen3.5-9B 始终无法进入第一个训练步骤；Gemma 4 26B-A4B QLoRa 在 96 GB 显存（RTX Pro 6000）上、batch size 为 1 时便 OOM（[#7203](https://github.com/unslothai/unsloth/issues/7203)）。
4. **[未解决]** GPT-OSS-120B-K4-KM 在 Studio 中使用默认设置会报错“output does not match the expected peg-native format”（[#10252](https://github.com/unslothai/unsloth/issues/10252)）。
5. **[未解决]** ROCm：由于缺少融合注意力 / 只能使用 SDPA 数学回退，Wan2.2 TI2V 在 RX 9060 XT 上 OOM（[#10415](https://github.com/unslothai/unsloth/issues/10415)）；另外，在 Radeon PRO 上即使勾选了“No RAM Offload”，模型仍然留在主机内存中（[#10341](https://github.com/unslothai/unsloth/issues/10341)）。
6. **[未解决]** Qwen3.6-35B-A3B MLX API 通过 base64 和 URL 输入均不可用（[#10389](https://github.com/unslothai/unsloth/issues/10389)）；Qwen3.8-27B-Uncensored-MLX 在 tokenizer_utils 中崩溃（[#10385](https://github.com/unslothai/unsloth/issues/10385)）。
7. **[未解决]** Studio API 密钥保护对 238 个字符的密钥失败，报错“RSAES-OAEP: input message length is too long”（[#10411](https://github.com/unslothai/unsloth/issues/10411)）；对发送空 bearer token 的测试框架，无密钥认证也会失败（[#10400](https://github.com/unslothai/unsloth/issues/10400)）。
8. **[未解决]** 在多设备间分布模型时，`--tensor-split` 被静默忽略（[#10355](https://github.com/unslothai/unsloth/issues/10355)）。
9. **[修复 PR]** composer-icon 工作引入的两个前端回归导致 CI 变红；已提交 crypto-polyfill 和 Firefox 缩放测量修复（[#10412](https://github.com/unslothai/unsloth/pull/10412), [#10413](https://github.com/unslothai/unsloth/pull/10413)）。
10. **[已关闭安全修复]** Studio Hub 写入路径不再将后端的 HF_TOKEN 借给 API 密钥调用者；读取/视频/音频/图像路径已在此前覆盖（[#10126](https://github.com/unslothai/unsloth/issues/10126)）。

## 对应用开发者的意义
- **KV 缓存语义正在改变**：如果你是针对 Studio 本地 llama-server 构建客户端，请注意每槽上下文不再是硬性保证——当共享池满时，较早的聊天槽可能会被停放至主机内存。请在生成调用中加入重试/退避，不要假设某个槽位声明的上下文始终驻留（[#10301](https://github.com/unslothai/unsloth/pull/10301), [#10358](https://github.com/unslothai/unsloth/pull/10358)）。
- **批量 vs 单条解码修复落地后重新验证评测框架**：此前小规模 Qwen/gemma-4 LoRA 上的批量贪心运行会静默产生不同的文本（[#9708](https://github.com/unslothai/unsloth/issues/9708)）。
- **Studio 安全加固是实打实的**：依赖预设管理员密码或依赖后端 HF_TOKEN 传递给 API 密钥调用者的自动化，需要迁移到一次性设置令牌流程和按用户分配的 Hub 凭据（[#10387](https://github.com/unslothai/unsloth/pull/10387), [#10126](https://github.com/unslothai/unsloth/issues/10126)）。
- **MLX 格式的 Qwen 目前还很脆弱**——Qwen3.6-35B-A3B API 调用直接失败，Qwen3.8-27B-MLX 在分词阶段崩溃；在依赖这些路径之前请先验证（[#10389](https://github.com/unslothai/unsloth/issues/10389), [#10385](https://github.com/unslothai/unsloth/issues/10385)）。
- **推测解码用户**很快就能测量接受率，而不是在为一个草稿模型付费前靠肉眼判断 tok/s（[#10401](https://github.com/unslothai/unsloth/issues/10401), [#10416](https://github.com/unslothai/unsloth/pull/10416)）。
- **在 AMD ROCm 上**，注意力内核覆盖仍是视频负载和卸载控制的主要阻碍；在投入生产前请做好使用 PyTorch SDPA 回退的准备，并关注跟踪器进展（[#10415](https://github.com/unslothai/unsloth/issues/10415), [#10341](https://github.com/unslothai/unsloth/issues/10341)）。

</details>