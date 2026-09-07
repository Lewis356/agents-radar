# AI CLI 工具社区动态日报 2026-09-07

> 生成时间: 2026-09-07 04:41 UTC | 覆盖工具: 7 个

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/earendil-works/pi)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## 横向对比

# 跨工具 AI CLI 对比报告 — 2026-09-07

## 1. 生态概览

本次调研的七款主流 AI CLI 工具正处于一边扩展功能覆盖面、一边偿还可靠性债务的阶段。2026-09-07 的社区注意力主要集中在破坏性文件操作事故、权限/护栏回归问题、无法终止或误报成功的后台任务，以及 Windows 特有故障上——而不是新功能需求。与此同时，从 PR 队列来看，平台投资仍在持续，方向包括管理工作树、MCP 传输验证、持久化优化、进程生命周期语义和 WebShell 控制面工具。发布节奏正在分化：Gemini CLI 每日发布 nightly，Qwen Code 在窗口期内同时发布了 preview 和 nightly，而 Claude Code、GitHub Copilot CLI、OpenCode、Pi 均无发布。总体来看，这个市场正进入“信任与加固”阶段：安全核算——权限协议、进程/会话清理、成本/上下文透明度——已成为首要竞争维度。

## 2. 活跃度对比

以下数字来自各项目 2026-09-07 的每日摘要中点名列出的值得关注事项，而非 GitHub 原始事件总数；七个项目都以 GitHub Issues/PR 作为活跃社区渠道，因此没有渠道被标为 N/A。

| 项目 | 热门 Issues | 关键 PRs | 讨论帖 | 24 小时内发布 |
|---|---|---|---|---|
| Claude Code (`anthropics/claude-code`) | 10 | 10 | 摘要未报告 | 无（明确说明） |
| OpenAI Codex (`openai/codex`) | 10 | 10 | 8（Ideas/Q&A/Show & tell） | 摘要未报告 |
| Gemini CLI (`google-gemini/gemini-cli`) | 10 | 10（列出 11 个；#29131/#29132 为一对） | 摘要未报告 | Nightly `v0.60.0-nightly.20260907.g85aca163f` |
| GitHub Copilot CLI (`github/copilot-cli`) | 10 | 1 | 无报告 | 无（明确说明） |
| OpenCode (`anomalyco/opencode`) | 10 | 10 | 无报告 | 无（明确说明） |
| Pi (`earendil-works/pi`) | 10 | 10 | 1 | 无（明确说明） |
| Qwen Code (`QwenLM/qwen-code`) | 6 | 10 | 摘要未报告 | `v0.23.1-preview.1` + `v0.23.0-nightly.20260906` |

**注：** OpenAI Codex 是本轮互动量最高的项目，讨论帖标题数量最多；Gemini CLI 与 Qwen Code 是窗口期内仅有的有发布产物的项目；GitHub Copilot CLI 尽管 Issue 量很大，却是 PR/发布层面最不活跃的项目。

## 3. 共同功能方向

- **记忆/上下文生命周期控制。** 多个社区都在要求对“什么进入上下文、以及何时进入”拥有显式控制。Claude Code 用户希望 auto-memory 压缩阈值可配置（[#91188](https://github.com/anthropics/claude-code/issues/91188)），并希望 `MEMORY.md` 在不同 git 工作树间的加载保持一致（[#81833](https://github.com/anthropics/claude-code/issues/81833)）。Gemini CLI 关于 Auto Memory 的一组 issue 则要求做确定性脱敏，并结束对低信号会话的无限重试（[#26522](https://github.com/google-gemini/gemini-cli/issues/26522)、[#26525](https://github.com/google-gemini/gemini-cli/issues/26525)）。OpenCode 则展示了当 provider 用量被误报、自动压缩每轮都触发时的故障模式（[#47296](https://github.com/anomalyco/opencode/issues/47296)）。

- **自主执行下的确定性安全。** Copilot CLI、Claude Code、Gemini CLI、Qwen Code 与 OpenCode 的社区正在提出同样的保证要求：不能静默自动批准工具调用（Copilot [#4537](https://github.com/github/copilot-cli/issues/4537)）；进程树取消要能真正终止破坏性命令（Claude Code [#92593](https://github.com/anthropics/claude-code/issues/92593)）；校验破坏性 git 参数（Gemini [#29184](https://github.com/google-gemini/gemini-cli/pull/29184)）；回滚逻辑不能丢弃 hook 创建的提交（Qwen [#11253](https://github.com/QwenLM/qwen-code/issues/11253)）；为 agent 循环设置熔断器（OpenCode [#31942](https://github.com/anomalyco/opencode/issues/31942)）。

- **用量、成本与速率限制透明度。** OpenAI Codex 用户报告应用与 CLI 之间权益不一致（[#40939](https://github.com/openai/codex/issues/40939)）；Copilot BYOK 用户报告提示缓存被静默禁用，成本约为 5 倍（[#4720](https://github.com/github/copilot-cli/issues/4720)）；Together AI 报告 OpenCode 中用量全为零（[#47716](https://github.com/anomalyco/opencode/issues/47716)）；Pi 正在增加 provider 上报成本与更好的缓存断点（[#6881](https://github.com/earendil-works/pi/pull/6881)、[#9246](https://github.com/earendil-works/pi/issues/9246)）；Qwen Code 现在会显示实时 Goal token 预算（[#11254](https://github.com/QwenLM/qwen-code/pull/11254)）。共同需求：一个可解释的核算来源，让压缩、预算与计费一致地以它为准。

- **后台任务/子代理的可观测性。** Gemini 用户希望子代理轨迹包含在 `/chat share` 与 `/bug` 报告中（[#22598](https://github.com/google-gemini/gemini-cli/issues/22598)、[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)）；Copilot ACP 用户需要一个能覆盖后台 shell 的空闲信号（[#4743](https://github.com/github/copilot-cli/issues/4743)）；OpenCode serve 用户希望请求级实例被及时释放（[#47727](https://github.com/anomalyco/opencode/issues/47727)）；Qwen WebShell 正在加入 shell/monitor 任务输出可见性（[#10906](https://github.com/QwenLM/qwen-code/pull/10906)）；OpenAI Codex 新增了管理工作树浏览器，以回答“什么运行在哪里”（[#43286](https://github.com/openai/codex/pull/43286)）。

- **Windows 与桌面环境一致性。** 这是跨工具分布最广的一簇痛点。例如：Claude Code 在 MSYS 下 `rm -rf` 导致驱动器根目录被删、hook 环境文件被写坏、拒绝大小写敏感工作树、`$TMPDIR` 只读（[#92593](https://github.com/anthropics/claude-code/issues/92593)、[#78146](https://github.com/anthropics/claude-code/issues/78146)、[#91618](https://github.com/anthropics/claude-code/issues/91618)、[#92590](https://github.com/anthropics/claude-code/issues/92590)）；OpenAI Codex 缺少 Windows 远程控制 UI、PowerShell 启动失败（[#28919](https://github.com/openai/codex/issues/28919)、[#17325](https://github.com/openai/codex/issues/17325)）；Copilot 的 WSL2 RSS 高达约 31 GB（[#4694](https://github.com/github/copilot-cli/issues/4694)）；Pi 忽略 `shell_path` 而使用 WSL bash（[#9229](https://github.com/earendil-works/pi/issues/9229)）；OpenCode 桌面版没有把用户 shell 的 `PATH` 传给插件子进程（[#47710](https://github.com/anomalyco/opencode/issues/47710)）。

- **工作树/多会话隔离作为正确性原语。** OpenAI Codex 正在投入管理工作树浏览与延迟转换（[#43286](https://github.com/openai/codex/pull/43286)、[#43298](https://github.com/openai/codex/pull/43298)）；Claude Code 受困于不同工作树间记忆加载不一致（[#81833](https://github.com/anthropics/claude-code/issues/81833)）；Copilot Desktop 阻止每个项目启动第二个 Local 会话（[#4742](https://github.com/github/copilot-cli/issues/4742)）；Qwen Code 只在会话围栏的配合下启用并发 daemon（[#11207](https://github.com/QwenLM/qwen-code/pull/11207)）。开发者显然期望以确定性的项目/会话身份作为回滚、记忆与并行的底座。

## 4. 差异化分析

- **Claude Code** 是功能面最广的“助手平台”：插件/市场基础设施、企业级网络安全验证、auto-memory、Cowork 与桌面/编辑器集成。其最高讨论串有 197 条评论，显示出巨大的存量用户群压力——多数摩擦来自组织层面（CVP 拦截、模型路由），而非基础功能。当日 PR 批次是插件基础设施加固（Windows 路径、shell 注入、符号链接逃逸），说明这是一套成熟但深受安全约束的系统。

- **OpenAI Codex** 的差异化在于运行时工程：可线性化的线程 rollout 序号、管理工作树池、远程权限配置文件的保真执行，以及 MCP 用户验证流程。它把会话历史当作事件溯源账本，因此即便 JSONL 完整无损，重复序号的投影 bug 也会被感知为数据丢失。其社区最强烈的一项呼声——turn 级 `/rewind`（[#9618](https://github.com/openai/codex/discussions/9618)，119 👍）——恰恰揭示了这一设计的代价：没有用户撤销机制的线性历史会显得不可逆。

- **Gemini CLI** 是 nightly 优先的多智能体研究/运维工具：通用型、代码库调查型和浏览器型子代理，外加沙箱与 OS 级 bash 安全讨论。它最明显的弱点是自报告不准确——`MAX_TURNS` 中断被报告成 `GOAL` 成功（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)）——以及模型对自定义技能/子代理的利用不足（[#21968](https://github.com/google-gemini/gemini-cli/issues/21968)）。其轨迹正走向“智能体团队”，因此需要更强的可观测性和诚实的终止语义。

- **GitHub Copilot CLI** 以协议与合规为中心：ACP 会话生命周期、企业默认模型、GHEC 数据驻留端点和权限重新请求行为。窗口期内只有 1 个文档 PR、没有发布，它在表层是进展最慢的项目，却承担着最重的企业策略负担。其置顶 Issue——ACP 模式下的自动批准回归（[#4537](https://github.com/github/copilot-cli/issues/4537)）——正是那种对组织推广最致命的信任破坏 bug。

- **OpenCode** 是模块化、以插件为中心的挑战者：Promise/Effect 插件 API、模板命令、权限断言、Firecrawl 开发者搜索 provider，以及参照 VS Code 存储模型重构渲染器持久化。同时它在运维层也最缺乏实战检验——数据库无界增长（[#47729](https://github.com/anomalyco/opencode/issues/47729)）与 `serve` 实例/MCP 泄漏（[#47727](https://github.com/anomalyco/opencode/issues/47727)）表明 v2.0 beta 仍在追赶长驻 daemon 工作负载的要求。

- **Pi** 是“通用连接层”：单一 agent 运行时，路由多个后端（OpenAI Codex、Copilot GPT 模型、Anthropic、OpenRouter、OpenCode Go），并吸收各网关的怪癖——端点路由、strict-schema 标志、MagicDNS 解析、SSE 解析边界情况。它的差异化来自 provider 无关的传输层，外加用于 turn 中转向、逐调用确认和后备 provider 的扩展 API。它的社区更小，但重度用户密集，而且对 Windows 尤其熟悉。

- **Qwen Code** 是控制面/WebShell 型产品：daemon 会话围栏、prompt 与 UI 中的 Goal token 预算、IPC review-gate 细化，以及 WebShell 任务输出。它也是这批开源项目中最具运维自动化能力的——CI bot 自己提交了故障报告并标为 `autofix/in-progress`，维护仪表盘追踪集群状态。其重心显然放在长时间运行的自主 Goal 上，而非交互式 IDE 的打磨。

## 5. 社区势头与成熟度

- **OpenAI Codex** 本轮综合势头最强：十个实质性合并 PR（MCP 验证、权限配置文件、工作树浏览器、Windows 关机支持），加一项各 digest 中信号最强的功能需求（119 👍 求 `/rewind`）。这是一个快速迭代的项目，社区声音响亮且对工作流如饥似渴。

- **Claude Code** 的社区按参与深度计最大——197 条评论和 151 个 👍 的讨论串是各 digest 中最高的原始数字——但 24 小时窗口内发布动作寥寥。其模式是：一个成熟的平台，却积累着企业级规模的信任债务，并正在组织验证、hang 严重性与 Windows 安全方面不断加重。

- **Gemini CLI** 是这批工具里发布最快的：一个周期内发出 nightly，并关闭多个 P1 修复（EOL Node 升级、symlink glob 回归、CRLF 误分类、SSE 末尾事件丢失）。社区活跃但相对更小；维护者用 nightly 节奏补上了这一点。

- **Qwen Code** 高速且自动化程度异常高：preview + nightly 双发布、十个实质性 PR（包括一个正在推进的 git 数据丢失修复），外加 CI 自动分诊。它的 issue 跟踪器更新最少（6 条），说明用户群更聚焦，或更多活动经由其他渠道流动，但工程节奏属于最快一档。

- **OpenCode** 呈现出集中的重构动能：一个窗口内完成了三 PR 的持久化重构，外加插件/桌面修复。社区规模看起来仍然有限（多数 issue 只有 1–3 条评论），v2.0 beta 也明显还处在加固前阶段——但它的架构投入（内容寻址存储、权限断言、模板插件）意义不小。

- **Pi** 在无发布的情况下以 10 个 PR 维持稳定、由维护者驱动的进展。其标志性讨论——76 条评论的 openai-codex hang 帖与 57 条评论的 Windows 体验帖——反映出一个规模不大但参与度高、把 Pi 当作跨多个后端日常主力 meta-agent 的社区。

- **GitHub Copilot CLI** 是窗口期内最不活跃的（1 个文档 PR、无发布），也最受企业/确定性需求制约。相对于表面进度，它的 issue 队列满载高严重性回归，也许说明发布流程偏保守，而不是投入不足。

## 6. 趋势信号

- **进程生命周期与回滚安全仍是未解之题。** 破坏性命令在超时或转入后台后继续存活、回滚删除 hook 创建的提交、后台工作还没结束 `end_turn` 就已返回——这些都指向同一个结论：agent 需要把事务性进程树和中止（abort）语义当成核心原语，而不是事后的补丁。对开发者而言，应从第一天起就把 kill、幂等性和引用比较设计进工具执行中。

- **护栏会在两个方向失效，两种失效都在侵蚀信任。** Copilot CLI 未经许可就自动批准工具调用，而 Claude Code 的 safeguard 系统把 CVP 已批准的组织挡在门外。解法不是“更多”或“更少”拦截，而是确定性、可审计、感知组织策略且有回归测试的权限协议，让行为跨会话、跨表面保持一致。

- **没有可观测性的自主是不可接受的。** 预算、速率限制权益、子代理轨迹、provider 上报成本、实际模型身份与后台任务状态，在本轮都成了社区诉求。正在成形的预期是：agent 应该提供仪表盘——花了多少钱、还剩多少 token、哪些子代理运行过、它们改了什么——而不是只给一个最终 diff。

- **成本与上下文核算如今是信任功能。** BYOK 提示缓存被静默禁用（约 5 倍成本）、缓存输入被重复计数并触发虚假压缩、provider 上报全零用量——这些都不是无关痛痒的表面问题，而是会直接打击用户对长时自主会话的信心。Provider 的用量核算需要成为单一事实来源，预算、压缩与计费界面都要一致地以它为据。

- **企业策略要跨表面贯穿，而不只是停留在 IDE。** 组织配置的默认模型被 CLI 无视、数据驻留端点选错、无法解释的模型降级、CVP 反复复审——这些都说明，缺乏跨表面一致策略执行的企业验证流程迟早会制造事故。把模型路由、权益与安全护栏当作同一个策略平面的工具团队，才能在组织推广中胜出。

- **Windows 已成为战略性的可靠性战场。** 七个工具中有六个出现密集的 Windows 专属数据丢失、PATH、shell 启动与内存增长报告，说明跨平台一致性是 AI CLI 工具当前最清晰的差异化机会。评估工具的开发者应当在其选型标准中，为 Windows 进程树终止、路径转换与桌面环境继承赋予较高权重。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — 社区亮点报告

**数据快照：** 2026-09-07 · github.com/anthropics/skills  
**数据说明：** PR 记录按评论量排序导出（共 50 条，显示前 20 条），但各 PR 的评论数显示为 `undefined`；Issue 评论数则正常显示。除非另有说明，所有 PR 均处于 **OPEN（开放）** 状态。

---

## 1. 顶级技能排名

*排名遵循数据集中“按评论数排序”的顺序。*

**#1 — [#1298](https://github.com/anthropics/skills/pull/1298)：skill-creator 评估框架修复（按评论数排名第 1）**  
修复 `skill-creator` 元技能的问题：`run_eval.py` 对每份技能描述都报告 `recall=0%`，导致 `run_loop.py` 和 `improve_description.py` “针对噪声进行优化”。该 PR 将评估产物作为真正的技能安装，并修复了 Windows 流读取、触发检测和并行 worker 的问题。这是社区最紧迫的痛点——它直接回应 Issue [#556](https://github.com/anthropics/skills/issues/556)（12 条评论，7 👍）所描述的问题，已有 10+ 个独立复现案例。配套的 Windows 修复 PR [#1099](https://github.com/anthropics/skills/pull/1099) 和 [#1050](https://github.com/anthropics/skills/pull/1050) 也位列前 20。**状态：** OPEN，更新于 2026-06-23。

**#2 — [#514](https://github.com/anthropics/skills/pull/514)：document-typography 技能**  
面向生成文档的排版质量控制新技能：孤词换行、页面底部的孤行段落 / 孤立章节标题，以及编号错位。讨论围绕一类用户很少主动要求、却几乎影响每一份 AI 生成文档的质量问题展开。**状态：** OPEN，更新于 2026-03-13。

**#3 — [#1615](https://github.com/anthropics/skills/pull/1615)：scnet-hpc 技能**  
通过基于 profile 的 SSH 和 Slurm 工作流来操作 SCNet HPC 集群的新技能：连接配置、分区 / 内存 / 模块指引、作业生成和集群发现。这表明社区对垂直领域 / 基础设施专属的操作型技能存在需求，而非通用的写作 / 编码技能。**状态：** OPEN，更新于 2026-08-24（活跃中）。

**#4 — [#538](https://github.com/anthropics/skills/pull/538)：pdf 技能大小写敏感问题修复**  
一个小但讨论度极高的修复：8 处大小写不匹配的文件引用（`REFERENCE.md` → `reference.md`、`FORMS.md` → `forms.md`）会让 PDF 技能在区分大小写的文件系统上失效。讨论反映出社区对内置技能跨平台正确性的痛点——也折射出一种担忧：即使是微不足道且正确的修复，也会在评审中长久滞留。**状态：** 自 2026-03-06 起 OPEN。

**#5 — [#486](https://github.com/anthropics/skills/pull/486)：ODT 技能**  
面向 OpenDocument 格式的新技能：创建 / 填充 / 读取 / 转换 `.odt` / `.ods`、模板填充，以及 ODT→HTML 解析。触发器覆盖范围包括 “OpenDocument”、“LibreOffice” 和 ISO 标准格式请求——明确指向企业文档互操作需求。**状态：** OPEN，更新于 2026-04-14。

**#6 — [#210](https://github.com/anthropics/skills/pull/210)：frontend-design 技能修订**  
对现有 `frontend-design` 技能进行实质性重写，目标是 “清晰、可操作、内部连贯”——即提供 Claude 在单次对话中即可遵循的指令。相关讨论是背后更大问题的缩影：一份 SKILL.md 应当有多强的指令性、多细的粒度？**状态：** OPEN，更新于 2026-03-07。

**#7 — [#83](https://github.com/anthropics/skills/pull/83)：skill-quality-analyzer + skill-security-analyzer（元技能）**  
向示例市场新增两个元技能：一个从五个维度对结构 / 文档（占 20%）、示例和资源进行评分的质量分析器，外加一个安全分析器。该 PR 提前回应了社区最大的 Issue（#492，见下文）——即在用户信任技能之前先对技能加以验证的工具需求。**状态：** OPEN，更新于 2026-01-07。

**#8 — [#541](https://github.com/anthropics/skills/pull/541)：docx 技能——跟踪修订 `w:id` 冲突修复**  
修复 DOCX 跟踪修订与现有书签发生 ID 冲突时导致的文档损坏。根因解释得很清楚：OOXML 共享同一个 `w:id` ID 空间，而技能中硬编码的低位 ID（1、2、3）会与真实书签冲突。该问题之所以高度可见，是因为生成的 DOCX 文件损坏是一种严重的、直接面向用户的故障。**状态：** OPEN，更新于 2026-04-16。

---

## 2. 社区需求趋势

*本次导出的 15 个 Issue 呈现出以下集中的需求信号：*

**安全 / 供应链信任（数据集中最强的需求）。**  
Issue [#492](https://github.com/anthropics/skills/issues/492) —— *“安全：在 anthropic/ 命名空间下分发的社区技能会引发信任边界滥用”* —— 以 **43 条评论** 居所有条目之首，也是整个导出中信号最强的讨论串。相关 Issue [#1175](https://github.com/anthropics/skills/issues/1175) 关注 SharePoint 文档中 SKILL.md 文件内的访问控制逻辑。隐含的技能方向：**社区技能的来源验证、安全审计与命名空间隔离。**

**技能分发与治理。**  
Issue [#228](https://github.com/anthropics/skills/issues/228)（16 条评论，8 👍）要求在 Claude.ai 内实现组织级技能共享；[#189](https://github.com/anthropics/skills/issues/189)（9 👍——为全部 Issue 中最高 👍 数）报告同时安装 `document-skills` 和 `example-skills` 时出现技能重复；[#62](https://github.com/anthropics/skills/issues/62)（10 条评论）报告用户丢失了全部 12 个本地创建的技能。隐含的需求：**技能库管理——共享、去重、持久化。**

**可靠的技能评估工具。**  
Issue [#556](https://github.com/anthropics/skills/issues/556)（12 条评论，7 👍）记录了 `run_eval.py` 中的 0% 触发率 bug；[#202](https://github.com/anthropics/skills/issues/202) 认为 `skill-creator` “读起来像开发者文档”，违反了最佳实践；[#1390](https://github.com/anthropics/skills/issues/1390) 和 [#1362](https://github.com/anthropics/skills/issues/1362) 报告评估与打包框架会静默失败。隐含的需求：**能够构建技能、且自带可信评估框架的技能。**

**上下文窗口效率。**  
Issue [#1487](https://github.com/anthropics/skills/issues/1487) 报告 `claude-api` 技能在一次工具调用中便急切地注入了约 156k token；[#1329](https://github.com/anthropics/skills/issues/1329)（9 条评论）提议用符号记法表示 agent 状态，打造一个 “compact-memory” 技能。隐含的需求：**紧凑、低开销的技能表示。**

**安全 / 质量门禁元技能。**  
Issue [#412](https://github.com/anthropics/skills/issues/412)（agent 治理技能，已关闭）和 [#1385](https://github.com/anthropics/skills/issues/1385)（三闸门推理质量流水线）显示出社区对**在交付前为 AI 输出把关的审计与验证技能**的兴趣。

---

## 3. 高潜力的待合并技能

上述排名靠前的候选（#514、#1615、#486、#210、#83）都是内容扎实、尚未合并的技能提案，一旦积压队列处理完毕，它们很可能被合入。其他几个活跃的开放 PR 也看起来接近就绪：

- **[#1628 — Hivemind](https://github.com/anthropics/skills/pull/1628)** —— 零成本多智能体编排：Claude Code 负责规划 / 评审，基于免费模型的无头 `opencode` worker 则执行机械性工作。关于上下文开销的讨论十分活跃。更新于 2026-08-24。
- **[#1627 — Buffer GraphQL API Agent Skill](https://github.com/anthropics/skills/pull/1627)** —— 可移植的社交媒体排程 / 管理技能，适用于任何 agent（Claude、Cursor、Codex、n8n）。在所有待合并 PR 中更新时间最近（2026-09-05）。
- **[#1367 — self-audit v1.3.0](https://github.com/anthropics/skills/pull/1367)** —— 先执行机械式文件校验，再按损害严重程度排序进行四维推理审计。与 Issue #1385 的质量门禁需求相契合。更新于 2026-07-02。
- **[#723 — testing-patterns 技能](https://github.com/anthropics/skills/pull/723)** —— 全面覆盖测试理念（Testing Trophy）、单元测试约定和 React Testing Library。更新于 2026-04-21。
- **[#568 — ServiceNow 平台技能](https://github.com/anthropics/skills/pull/568)** —— 涵盖 ITSM、ITOM、ITAM/SAM、SecOps、FSM、SPM、CSDM 和 IntegrationHub 的综合性企业平台助手。更新于 2026-08-12——最活跃的长尾企业提案。

---

## 4. 技能生态系统洞察

纵观最活跃的 Issue 和 PR，社区最集中的需求已不再是更多内容型技能，而是对技能生命周期本身的掌握——可信的分发与来源验证（[#492](https://github.com/anthropics/skills/issues/492)）、组织级共享与去重（[#228](https://github.com/anthropics/skills/issues/228)、[#189](https://github.com/anthropics/skills/issues/189)），以及可靠的评估 / 质量门禁工具（[#556](https://github.com/anthropics/skills/issues/556)、[#202](https://github.com/anthropics/skills/issues/202)、[#1390](https://github.com/anthropics/skills/issues/1390)）——也就是说，整个生态正在呼唤“让技能更安全、可衡量、可维护的技能”。

---

## 1. 今日要点

过去 24 小时内没有发布新版本，但 issue 活跃度依然很高：两条历时最长的讨论串——影响 CVP 认证组织的 cyber safeguard 拦截（[#84352](https://github.com/anthropics/claude-code/issues/84352)，197 条评论）和长达数分钟的 CLI 挂起（[#26224](https://github.com/anthropics/claude-code/issues/26224)，151 👍）——都有新进展。今天还出现了几个高严重性报告，包括一起由 MSYS 转换的 `rm -rf` 引起的 Windows 数据丢失事件（[#92593](https://github.com/anthropics/claude-code/issues/92593)）。PR 方面，一大批插件基础设施修复（Windows 路径处理、安全加固）已经关闭/合并。

## 3. 热门 Issues

1. **[通过 CVP 认证的 Claude.ai 组织在 Claude Code 中仍然收到 cyber safeguard 拦截（#84352）](https://github.com/anthropics/claude-code/issues/84352)** — 197 条评论，27 👍。一个之前已通过 Cyber Verification Program 认证的组织再次被拦截，尽管此前已获批准，验证门户现在显示"审核中"。高评论数使其成为组织级部署的头号阻碍。

2. **[Claude Code 在提示词处挂起/卡死 5–20+ 分钟（#26224）](https://github.com/anthropics/claude-code/issues/26224)** — 130 条评论，151 👍。获赞最多的未解决 bug，约七个月仍无解决方案。核心 CLI 响应能力仍然是社区最关心的痛点。

3. **[失控的 rm -rf：超时自动后台化让破坏性命令持续运行；TaskStop 未能杀死子进程，Windows（#92593）](https://github.com/anthropics/claude-code/issues/92593)** — 今日新增。一个子代理执行了 `rm -rf "\"`；MSYS 路径转换将反斜杠解析为驱动器根目录并递归删除了 `C:\`。这引发了对 Windows 上超时自动后台化和进程树终止机制的紧急关注。

4. **[功能请求：让自动记忆 MEMORY.md 压缩提醒阈值可配置（#91188）](https://github.com/anthropics/claude-code/issues/91188)** — 28 条评论。硬编码的 200 行/25KB 记忆阈值对某些用户来说触发过早；该请求要求支持配置或独立关闭。

5. **[来自 settings.json 的自定义 statusLine 命令未被执行（#13517）](https://github.com/anthropics/claude-code/issues/13517)** — 23 条评论，21 👍。macOS TUI bug：配置的 `statusLine` 命令被静默忽略，破坏了自定义终端工作流。

6. **[自动记忆在 git worktree 会话中加载不一致（#81833）](https://github.com/anthropics/claude-code/issues/81833)** — 19 条评论。同一仓库、同一天：部分 worktree 会话加载了完整的 `MEMORY.md` 索引，其他会话则完全没有收到记忆内容——对于重度使用 worktree 的工作流来说是一个可靠性问题。

7. **[即使版本已过期，Marketplace 更新按钮仍为禁用/无法点击状态（#45810）](https://github.com/anthropics/claude-code/issues/45810)** — 17 条评论，8 👍。插件市场的 Update 按钮保持灰色，阻止用户拉取新版插件。

8. **[Autocompact 抖动：上下文在 3 轮对话内被重新填满至上限，连续 3 次（#82131）](https://github.com/anthropics/claude-code/issues/82131)** — 13 条评论。重复的压缩→回填循环严重破坏了长会话的上下文效率。

9. **[Cowork：Chat/Cowork 合并后新项目丢失了"选择一个文件夹"选项（#76694）](https://github.com/anthropics/claude-code/issues/76694)** — 12 条评论，15 👍。上下文菜单被替换为聊天式纯上传知识菜单，破坏了以文件夹为中心的项目创建流程。

10. **[自动记忆指令直接引导立即编辑 MEMORY.md 指针，但读前写后一致性门控确定性地拒绝了该操作（#78569）](https://github.com/anthropics/claude-code/issues/78569)** — 11 条评论。模型被指示更新记忆，随后又被工具自身的一致性门控阻止——一个自相矛盾的工作流。

## 4. 关键 PR 进展

1. **[fix(security-guidance)：使 ** 通配模式匹配零深度路径（#87079）](https://github.com/anthropics/claude-code/pull/87079)** — 开放中。`fnmatch` 委托意味着 `**/*.ts` 需要字面 `/`，导致顶层文件被静默排除在 security-patterns 规则之外——一个隐蔽的安全覆盖缺口。

2. **[fix(security-guidance)：阻止可扩展性配置读取中的符号链接逃逸（#68689）](https://github.com/anthropics/claude-code/pull/68689)** — 关闭了一个本地文件泄露向量：恶意仓库将 `.claude/claude-security-guidance.md` 符号链接到 `~/.ssh/id_rsa` 等文件。

3. **[fix(plugin-dev)：通过 stdin 重定向避免 test-hook.sh 中的 shell 注入（#68786）](https://github.com/anthropics/claude-code/pull/68786)** — 修复了一个引号漏洞：嵌入在 `bash -c` 字符串中的 `$TEST_INPUT` 可能导致任意命令执行。

4. **[fix(hookify)：添加 Python 包装器并规范化 Windows 上的插件根路径（#68699）](https://github.com/anthropics/claude-code/pull/68699)** — 规避了反斜杠分隔的 `CLAUDE_PLUGIN_ROOT` 值，以及 Microsoft Store 的 `python3` 存根在非 TTY 环境下静默以退出码 49 退出的问题。

5. **[fix(security-guidance)：在 Windows 上规范化 CLAUDE_PLUGIN_ROOT 路径分隔符（#68694）](https://github.com/anthropics/claude-code/pull/68694)** — 转换所有六个 hook 命令中的反斜杠，使引用 `${CLAUDE_PLUGIN_ROOT}` 的内联 bash 在 Windows 上正常工作。

6. **[fix(security-guidance)：在 Windows 上从 Python 版本探测中去除 CRLF（#68701）](https://github.com/anthropics/claude-code/pull/68701)** — Windows Python 输出 `\r\n`，破坏了版本比较；探测现在去除 CRLF 以确保正确检测。

7. **[feat(bug-reporter)：添加 /bug 命令以直接从终端提交 GitHub issue（#68707）](https://github.com/anthropics/claude-code/pull/68707)** — 新插件和斜杠命令，让用户无需离开 CLI 即可在 `anthropics/claude-code` 上提交 issue。

8. **[fix(plugin-dev)：将 hook JSON 输出到 stdout，收紧 su\* 通配符，修复 CI 检测和示例中的 JSON 注入（#68785）](https://github.com/anthropics/claude-code/pull/68785)** — 修复了 hook 开发参考示例中的多个 bug，包括决策 JSON 被输出到 stderr 而非 stdout 的问题。

9. **[fix(scripts)：重复标签以增量方式添加，不替换现有标签（#68693）](https://github.com/anthropics/claude-code/pull/68693)** — 防止 issue 作为重复项关闭时 GitHub PATCH 语义清空 platform/area/priority 标签。

10. **[fix(hookify)：重命名被遮蔽的 'field' 变量并修复内联字典逗号解析（#68686）](https://github.com/anthropics/claude-code/pull/68686)** — 修复 hookify 中两个配置加载器的正确性 bug。

## 6. 功能请求趋势

- **记忆/上下文生命周期控制**：用户希望自动记忆压缩阈值可配置（[#91188](https://github.com/anthropics/claude-code/issues/91188)），跨 git worktree 会话一致加载记忆（[#81833](https://github.com/anthropics/claude-code/issues/81833)），以及解决读前写后冲突问题（[#78569](https://github.com/anthropics/claude-code/issues/78569)）。
- **UI 可配置性**：对退出选项和定制化的需求日益增长——可用的自定义 statusLine 命令（[#13517](https://github.com/anthropics/claude-code/issues/13517)）、禁用 VS Code 编辑器组自动锁定的设置（[#80148](https://github.com/anthropics/claude-code/issues/80148)）、Cowork 项目聊天按活动排序（[#87723](https://github.com/anthropics/claude-code/issues/87723)）。
- **桌面端/账户工作流支持**：Claude Desktop 中的多账户会话恢复（[#74662](https://github.com/anthropics/claude-code/issues/74662)），以及恢复 Cowork 中以文件夹为基础的项目创建方式（[#76694](https://github.com/anthropics/claude-code/issues/76694)）。
- **更丰富的渲染**：用于内联图像的终端图形协议（[#79706](https://github.com/anthropics/claude-code/issues/79706)），以及 VS Code 侧边栏/Remote-WSL 中的内联图像支持（[#85520](https://github.com/anthropics/claude-code/issues/85520)）。

## 7. 开发者痛点

- **卡顿和上下文抖动仍然是最主要的挫败感来源**：5–20 分钟的冻结 bug（[#26224](https://github.com/anthropics/claude-code/issues/26224)）仍然是获赞最多的未解决 issue，而 autocompact 抖动（[#82131](https://github.com/anthropics/claude-code/issues/82131)）会降低长会话的质量。
- **Windows 可靠性和安全缺口**：反复出现的 Windows 特定故障——撕裂的 hook 环境文件永久卡死 Bash 工具（[#78146](https://github.com/anthropics/claude-code/issues/78146)）、区分大小写的盘符比较拒绝有效 worktree（[#91618](https://github.com/anthropics/claude-code/issues/91618)）、`sandbox.enabled` 将 `$TMPDIR` 设为只读（[#92590](https://github.com/anthropics/claude-code/issues/92590)），以及破坏性的 `rm -rf`/TaskStop 失败（[#92593](https://github.com/anthropics/claude-code/issues/92593)）。
- **对非预期文件操作的信任担忧**：有报告指出系统在用户明确指示下仍执行删除操作（[#92589](https://github.com/anthropics/claude-code/issues/92589)），以及尽管有禁止指令仍执行 git 操作（[#77550](https://github.com/anthropics/claude-code/issues/77550)），这些都表明安全防护存在缺口。
- **自动记忆的摩擦**：用户遇到自相矛盾的行为——记忆更新被指示执行后又被拒绝（[#78569](https://github.com/anthropics/claude-code/issues/78569)），此外记忆加载不可预测，取决于会话类型（[#81833](https://github.com/anthropics/claude-code/issues/81833)）。
- **企业/组织采用障碍**：影响 CVP 认证组织的防护回归（[#84352](https://github.com/anthropics/claude-code/issues/84352)），以及从 Claude Fable 5.1 莫名降级到 Claude Opus 4.8 的问题（[#92591](https://github.com/anthropics/claude-code/issues/92591)），使生产部署和对模型路由的信任变得更加复杂。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区摘要 — 2026-09-07

## 今日要闻

本周社区的关注焦点集中在 Windows 桌面版可靠性和 VS Code 扩展回归问题上。26.901.22334 扩展故障（找不到 `chatgpt.openSidebar`）产生了多个重复报告，最终作为已知回归问题关闭；与此同时，围绕远程控制注册（#28919）、PowerShell 执行卡住（#17325）以及对话线程历史冻结（#41079）等长期存在的 Windows 问题仍在继续累积评论。工程方面，一批快速合并的 PR 为 TUI 新增了 MCP 用户验证流程、远程权限配置文件选择功能以及受管理 worktree 浏览器。

## 热门 Issue

1. **[#28919 — Windows Codex app missing "control other devices" tab](https://github.com/openai/codex/issues/28919)** — 63 条评论，59 👍。当前最活跃的问题：Windows 26.611.62324 构建中根本没有“Settings > Connections”里的远程控制选项卡，导致设备控制工作流受阻。
2. **[#41079 — Paginated thread history stalls on duplicate ordinal](https://github.com/openai/codex/issues/41079)** — 30 条评论。完整的 rollout JSONL 中包含所有后续消息，但桌面 UI 只显示较早的某个快照。这种“历史投影停滞”正在成为一个可辨识的故障类别。
3. **[#34499 — Cannot create a local Work chat inside a ChatGPT Project](https://github.com/openai/codex/issues/34499)** — 23 条评论，15 👍。使用 ChatGPT Plus 的 Windows 桌面版用户无法在 ChatGPT Project 内创建归属于该 Project 的本地 Work 聊天。
4. **[#42663 — Remote SSH: extension fails because VS Code server Node 22 can’t parse `using`](https://github.com/openai/codex/issues/42663)** — 10 条评论。26.5901.22334 扩展无法在 Remote-SSH 主机上激活；疑似原因是随附带 JS 的语法兼容性问题。
5. **[#40939 — Codex CLI can’t use Luna Reserve after standard limit is exhausted](https://github.com/openai/codex/issues/40939)** — 9 条评论。同一账户在 Codex App 中可用，但 CLI 识别不到该权益——这是一个会影响实际工作流的速率限制一致性问题。
6. **[#42882 — VS Code regression: `chatgpt.openSidebar` command not found](https://github.com/openai/codex/issues/42882)** — 已关闭，6 条评论，3 👍。这是确认 26.901.22334 更新后破坏侧边栏入口的多个重复报告（#42969、#43085）之一。
7. **[#42027 — Side-chat fork fails after interrupted turn: duplicate rollout ordinal](https://github.com/openai/codex/issues/42027)** — 7 条评论。与 #41079 相同的“期望序号 N+1，实际得到 N”投影失败，但经由 fork 路径触发——证明该缺陷影响多个 UI 流程。
8. **[#17325 — VS Code on Windows fails to start PowerShell for shell_command](https://github.com/openai/codex/issues/17325)** — 7 条评论，4 👍。只要 PowerShell 无法启动，所有本地 shell 工具调用都会失败；相关的 Windows PATH/pwsh 别名问题仍在 #18937 中跟踪。
9. **[#43124 — macOS desktop history freezes at older turns](https://github.com/openai/codex/issues/43124)** — 5 条评论。“投影期望序号 3185，实际得到 3184；迁移提示 already_paginated。”这份新报告表明序号问题不仅影响 Windows，macOS 同样存在。
10. **[#43344 — GPT-5.5 returns 404 model not found in brand-new conversation](https://github.com/openai/codex/issues/43344)** — 3 条评论，1 👍。Windows Pro 用户在新会话中立即在 `backend-api/codex/responses` 上遇到 404；该问题较新，仍处于初步分诊阶段。

## 重点 PR 进展

1. **[#43352 — Add opt-in MCP user-verification transport](https://github.com/openai/codex/pull/43352)** — 新增类型化的 `openai/userVerification` elicitation，用于在应用响应时附带标题信息。配套改动：#43289 中的能力门控和 #43265 中的 API 契约。
2. **[#43340 — Enable remote named permission profile selection in the TUI](https://github.com/openai/codex/pull/43340)** — 权限选择器此前会显示远程配置文件但处于禁用状态；该 PR 通过 `thread/settings/update` 接通了选择链路。
3. **[#43330 — Preserve saved permissions when resuming or forking remote tasks](https://github.com/openai/codex/pull/43330)** — 防止本地权限设置（包括命名配置文件）覆盖远程任务已保存的服务端设置。
4. **[#43308 — Replace Windows app-server shutdown files with socket requests](https://github.com/openai/codex/pull/43308)** — 将受管理的 Windows 关闭流程改为通过本地控制套接字上的 `/daemon/shutdown` 完成，并在 drain 之前要求 PID 确认——这是一次有意义的 Windows 稳定性修复。
5. **[#43286 — Add a managed worktree browser to the TUI](https://github.com/openai/codex/pull/43286)** — 新增可搜索的“Browse worktrees”选项，列出池检出项（checkouts）、所有者元数据以及恢复/复制操作。
6. **[#43298 — Defer managed worktree transitions to fresh TUI loop iterations](https://github.com/openai/codex/pull/43298)** — 将设置/检出工作从同步的 `ChatWidget` 构造函数中拆分出来，以避免事件循环卡顿。
7. **[#43248 — Connect voice-host RTP audio to speaker playback](https://github.com/openai/codex/pull/43248)** — 新增带抖动缓冲的 GStreamer 管道，让收到的 RTP 数据包能够被解码并播放，而不是仅被丢弃。
8. **[#43178 — Allow guarded legacy resume with background migration enabled](https://github.com/openai/codex/pull/43178)** — 当维护锁阻止在恢复期间执行迁移时，恢复使用被缓存的 legacy resume 快捷路径。
9. **[#43325 — Sort JSON schema object keys for consistent Cargo and Bazel output](https://github.com/openai/codex/pull/43325)** — 在写入 app-server 协议 schema 之前递归地对对象键排序，使 Cargo 和 Bazel 构建产物保持一致。
10. **[#43304 — Isolate Bazel build commit metadata from Rust compilation inputs](https://github.com/openai/codex/pull/43304)** — 避免打戳的 user/host/timestamp 值污染跨开发者、跨 CI worker 的远程缓存复用。

## 热门讨论

### 想法

1. **[#9618 — How is there not a /rewind or /revert feature?](https://github.com/openai/codex/discussions/9618)** — 119 👍，20 条评论。信号最强的功能请求：Codex 需要与 OpenCode、Claude Code 相当的可撤销步骤回退能力；社区认为当前“每步都提交”的工作流“几乎没法用”。
2. **[#14067 — Sync Codex threads and session context across devices](https://github.com/openai/codex/discussions/14067)** — 61 👍，10 条评论。在办公电脑和家用电脑之间切换的用户希望对话线程能跨设备延续，而不是将会话绑定在本地。
3. **[#7366 — Reference files that are gitignored](https://github.com/openai/codex/discussions/7366)** — 7 👍，2 条评论。`@` 引用应当对可被查找到但尚未提交的文件同样生效。
4. **[#42703 — Can history retrieval make history recursively self-referential?](https://github.com/openai/codex/discussions/42703)** — 1 👍。指出了新的 `history` / `notes` / `new_context` 方案在长周期场景下的一种失败模式。

### Q&A

1. **[#40740 — Does rollout tracing capture which path produced a Declined exec status?](https://github.com/openai/codex/discussions/40740)** — 深入剖析了 `rollout/src/policy.rs` 和 `protocol_event.rs`：审批请求被有意排除在持久化之外。
2. **[#43257 — How does experimental context management count history lookups against usage limits?](https://github.com/openai/codex/discussions/43257)** — 需要跨多日运行任务的用户想知道：跨上下文的历史检索是否会消耗配额。

### 展示与分享

1. **[#41157 — CodexFuse 1.2.0](https://github.com/openai/codex/discussions/41157)** — 面向 Windows 的本地 Codex 速率限制状态仪表盘（已用/可用、下次重置时间）；无需 API key。
2. **[#43224 — NULLYARD: public MCP board](https://github.com/openai/codex/discussions/43224)** — 由操作者创建的纯文本 MCP board，附有静态集成指南和公共技能。

## 功能需求趋势

- **撤销 / 回退 / 还原**：#9618 是获赞最多的单个想法，表明按轮次回滚是当前最缺失的工作流基础能力。
- **跨设备连续性**：会话与对话线程同步（#14067）和关于远程控制注册缺口（#28919、#39739）以及 Business 账户注册失败（#42575）的抱怨正好相互关联。
- **更丰富的本地上下文引用**：用户希望用 `@` 引用被 gitignore 的文件（#7366），也希望桌面应用能索引 `~/.agents/skills` 中的个人技能（#28505）。
- **远程权限保真**：最近的 PR（#43330、#43340）反映出用户需求：远程任务应保留自己保存的权限配置文件，而不是继承本地覆盖项。
- **透明的用量计算**：多个速率限制讨论（#40939、#43136、#43341），加上关于上下文管理计费的 Q&A（#43257），表明用户希望配额消耗可预测、可解释。

## 开发者痛点

- **Windows 仍是最大的短板**：缺失远程控制 UI（#28919）、PowerShell 启动失败（#17325/#18937）、Project 聊天创建缺口（#34499）、Business 远程控制注册失败（#42575）——这些问题全部集中在 Windows 上。
- **对话线程历史投影缺陷会造成可见的“数据丢失”**：重复序号类故障（#41079、#42027、#43124）会让 UI 冻结在较旧的快照上，即使 JSONL 本身完好无损——这类缺陷尤其让用户感到困惑。
- **扩展回归正在侵蚀信任**：26.901.22334 移除了侧边栏命令（#42882/#42969）、Remote-SSH 激活因 Node 22 语法问题而失败（#42663），迫使许多人回滚到 26.825.x。
- **智能体行为令人担忧**：有报告称某个任务运行了 4 个多小时仍未核验其主要目标（#43086）；安全审查重复出现误报，并升级到几乎每一轮都触发门控（#43312、#43321）——这表明自我调节和护栏调优仍是痛点。
- **速率限制权益不一致**：Luna Reserve 在 App 中可用、在 CLI 中却不可用（#40939），再加上限额被固定到错误的 bucket（#43136），让 Pro/Plus 套餐在不同端上的行为显得难以预判。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区摘要 — 2026-09-07

## 今日亮点

智能体可靠性与安全加固是本期摘要的主线：子智能体卡死、误导性的 “GOAL success” 终止状态以及浏览器智能体失败等几个长期悬而未决的问题成为讨论热点；与此同时，已合并的 PR 主要聚焦于移除 Windows 上静默破坏性的 `git diff --output` 执行路径、将沙箱从已停止维护的 Node 20 升级，以及修复因 CRLF/行尾缺陷而导致整个文件被倾倒进模型上下文的问题。团队也照常发布了每日构建版本。

## 版本发布

- **[v0.60.0-nightly.20260907.g85aca163f](https://github.com/google-gemini/gemini-cli/releases/tag/v0.60.0-nightly.20260907.g85aca163f)** — 自动化每日构建版本。无人工编写的更新日志说明；与昨日构建的差异请参阅[完整更新日志](https://github.com/google-gemini/gemini-cli/compare/v0.60.0-nightly.20260906.g85aca163f...v0.60.0-nightly.20260907.g85aca163f)。

## 热点问题

1. **[#22323：MAX_TURNS 后的子智能体恢复被报告为 GOAL success，掩盖了中断](https://github.com/google-gemini/gemini-cli/issues/22323)** — P1。`codebase_investigator` 子智能体在尚未做任何分析前就已命中 `MAX_TURNS`，却仍会报告 `success` + `GOAL`。13 条评论；这令人担忧，因为它会将智能体的失败静默地粉饰为成功。
2. **[#21409：通用智能体卡死](https://github.com/google-gemini/gemini-cli/issues/21409)** — P1，8 👍。创建文件夹之类的简单任务一旦委派给通用智能体就会无限期卡住（有用户等了一个小时）；指示模型不要委派即可绕过此问题。
3. **[#25166：Shell 命令执行完毕后仍卡在 “Waiting input”](https://github.com/google-gemini/gemini-cli/issues/25166)** — P1。简单、非交互式的 CLI 命令在结束很久之后仍显示为活动状态并等待输入 —— 这是一个核心级的保真度缺陷。
4. **[#21983：浏览器子智能体在 Wayland 下失败](https://github.com/google-gemini/gemini-cli/issues/21983)** — P1。在 Wayland 会话中，浏览器自动化会立即终止（被报告为 GOAL），阻塞了 Linux 上浏览器智能体用户的使用。
5. **[#21968：Gemini 对技能和子智能体的利用不够充分](https://github.com/google-gemini/gemini-cli/issues/21968)** — 拥有自定义 `gradle`/`git` 技能的社区成员报告，即使面对明确相关的任务，模型也几乎从不自动调用这些技能。
6. **[#26525：加入确定性脱敏并减少 Auto Memory 的日志输出](https://github.com/google-gemini/gemini-cli/issues/26525)** — 安全/隐私：Auto Memory 会在基于提示词的脱敏执行之前，就把会话记录内容送入模型上下文；技能内容也可能被过度记录。
7. **[#26522：阻止 Auto Memory 无限重试低信号会话](https://github.com/google-gemini/gemini-cli/issues/26522)** — 会话只有在一次成功的 `read_file` 之后才会被标记为已处理；被提取器跳过的低信号会话会永远反复出现。
8. **[#22232：增强 browser_agent 韧性 —— 自动会话接管与锁恢复](https://github.com/google-gemini/gemini-cli/issues/22232)** — 对锁定的浏览器 profile 采取 “fail-fast”（快速失败）策略会让持久会话变得脆弱；需要支持会话接管和孤儿进程恢复。
9. **[#19873：通过零依赖 OS 沙箱与执行后意图路由，发挥模型对 bash 的亲和力](https://github.com/google-gemini/gemini-cli/issues/19873)** — 增强请求，9 条评论：Gemini 模型天生就是 POSIX 工具使用者；提议采用安全的零依赖沙箱，使 bash 风格的探索/编辑既能使用，又不会牺牲安全性。
10. **[#22745：评估 AST 感知的文件读取、搜索与映射的影响](https://github.com/google-gemini/gemini-cli/issues/22745)** — Epic 议题，追踪 AST 感知工具能否减少大型代码库中的错位文件读取、token 噪声与导航轮次。

## 关键 PR 进展

1. **[#28973：将沙箱镜像从已停止维护的 node:20-slim 升级为 node:22-slim](https://github.com/google-gemini/gemini-cli/pull/28973)** — P1/安全，已合并。Node 20 已于 2026-04-30 停止维护（EOL）；修复 #28584，涉及仓库 Dockerfile 中的 builder 和 runtime 两个阶段。
2. **[#29184：在 Windows 沙箱中校验 git 参数，阻止静默执行 `git diff --output`](https://github.com/google-gemini/gemini-cli/pull/29184)** — P1/安全，开放中。在 Windows 上，所有 `git status | log | diff | show | branch` 均无需确认即可运行；`--output=<path>` 可能静默截断任意文件。
3. **[#28971：确保截断后的 MCP 工具名称保持唯一](https://github.com/google-gemini/gemini-cli/pull/28971)** — 已合并。前 30/后 30 字符截断不是一一映射；首尾两端一致的两个工具会坍缩成同一个注册名。
4. **[#28975：保留符号链接工作区根目录的 glob 结果](https://github.com/google-gemini/gemini-cli/pull/28975)** — 已合并，修复 #28416。当工作区根目录是通过符号链接访问时——macOS `/tmp` 下项目的默认情况——`glob` 会返回 “No files found”（未找到文件）。
5. **[#28983：检测混合行尾，而不是仅凭单个匹配就标记为 CRLF](https://github.com/google-gemini/gemini-cli/pull/28983)** — 已合并。`detectLineEnding()` 只要文件里有哪怕一个 `\r\n`，就会把原本以 LF 为主的文件判定为 CRLF。
6. **[#29132 与 #29131：规范化 diff 上下文片段中的行尾](https://github.com/google-gemini/gemini-cli/pull/29132)** — 两个并行的开放 PR（#29132 修复 #29130，外加 #29131），用于防止 CRLF 与 LF 的不一致把完整文件 diff 倒灌回上下文。
7. **[#29134：保护当前会话不被删除](https://github.com/google-gemini/gemini-cli/pull/29134)** — 开放中，修复 #29133。活动会话 ID 会经由 `--list-sessions`/`--delete-session` 传递，并使用短 ID 后缀匹配来避免删除正在使用的会话。
8. **[#28972：为 `formatTruncatedToolOutput` 增加对非正 maxChars 的保护](https://github.com/google-gemini/gemini-cli/pull/28972)** — P1，已合并，修复 #28620。负数预算会产生负的 `slice()` 索引，并在静默中破坏输出。
9. **[#29209：跳过非数字的后台 PID 行](https://github.com/google-gemini/gemini-cli/pull/29209)** — 已合并，修复 #29042。防止游离的警告/`NaN` 进入 `llmContent`；并为混合出现有效 PID/已知 sysmond 行的情况补充了回归测试覆盖。
10. **[#29106：在 EOF 处刷新最终的 SSE 事件，无需尾随空行](https://github.com/google-gemini/gemini-cli/pull/29106)** — 已合并。SSE 解析器在处理被截断/不合规的流时会丢弃最后一个已缓冲事件，导致 `finishReason` 和用量元数据被静默丢失。

## 功能需求趋势

- **AST 感知的代码库工具**：Epic [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) 与 [#22746](https://github.com/google-gemini/gemini-cli/issues/22746) 正推动 AST 级别的文件读取/搜索/映射，另有 [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) 要求为任务追踪器提供原生文件工具 —— 共同目标是更少、更精准的工具调用。
- **能够自主启动的子智能体/技能**：[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) 与 [#21432](https://github.com/google-gemini/gemini-cli/issues/21432) 都希望模型无需显式提示，就能自动调用合适的自定义技能和子智能体，并准确理解 CLI 自身的能力。
- **更安全的原生 bash 执行**：[#19873](https://github.com/google-gemini/gemini-cli/issues/19873)（零依赖 OS 沙箱）与 [#22672](https://github.com/google-gemini/gemini-cli/issues/22672)（抑制破坏性 `git reset`/`--force` 行为）搭配使用，让原生 shell 的强大能力在默认情况下也更安全。
- **Auto Memory 治理**：#26516 集群（[#26522](https://github.com/google-gemini/gemini-cli/issues/26522)、[#26523](https://github.com/google-gemini/gemini-cli/issues/26523)、[#26525](https://github.com/google-gemini/gemini-cli/issues/26525)）要求实现确定性的机密脱敏、隔离无效记忆补丁，并终结无限重试循环。
- **浏览器智能体自治**：[#22232](https://github.com/google-gemini/gemini-cli/issues/22232)（锁/接管恢复）与 [#22267](https://github.com/google-gemini/gemini-cli/issues/22267)（尊重 `settings.json` 覆盖配置）反映了用户对高韧性与可配置浏览器智能体的需求。
- **子智能体可观测性**：[#22598](https://github.com/google-gemini/gemini-cli/issues/22598)（`/chat share` 包含子智能体轨迹）与 [#21763](https://github.com/google-gemini/gemini-cli/issues/21763)（`/bug` 包含子智能体上下文）表明用户希望看到子智能体的实际行为。

## 开发者痛点

- **卡死与停摆**：通用智能体无限期挂起（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)）、shell 命令卡在 “Waiting input”（[#25166](https://github.com/google-gemini/gemini-cli/issues/25166)）、以及卡在 Vite 脚手架这类交互式提示处（[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)），都在侵蚀用户对自主模式的信任。
- **被包装成成功的失败**：[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) 显示 MAX_TURNS 中断会被报告为 GOAL/Success；[#21763](https://github.com/google-gemini/gemini-cli/issues/21763) 指出调试报告无法捕获子智能体内部细节 —— 一旦出错很难诊断。
- **浏览器智能体不稳定**：Wayland 失败（[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)）、profile 被锁定后的快速失败行为（[#22232](https://github.com/google-gemini/gemini-cli/issues/22232)）、以及覆盖配置被忽略（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)）。
- **上下文膨胀**：仅一个 CRLF 匹配就触发 CRLF 模式重写、行尾不一致导致完整文件 diff 被倒灌上下文（#29131/#29132）、以及模型在多个目录中随意丢弃临时脚本（[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)），都在浪费 token 并让 commit 难以保持整洁。
- **工具扩展性瓶颈**：工具数量超过约 128 个后便会出现 400 错误（[#24246](https://github.com/google-gemini/gemini-cli/issues/24246)）；MCP 工具名截断冲突（#28971）表明工具注册表在插件生态增长下已不堪重负。
- **平台层的小毛病**：符号链接形式的智能体文件无法被识别（[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)）、符号链接工作区根目录导致 glob 失效（#28975）、以及 `/compress` 摘要无法在恢复会话后保留（[#21335](https://github.com/google-gemini/gemini-cli/issues/21335)）。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区文摘 — 2026-09-07

## 今日亮点

过去 24 小时内没有发布新的 Copilot CLI 版本；活动主要集中在 issue 跟踪器上。最紧迫的报告涉及 ACP（智能体）模式：一个回归问题在未经客户端许可的情况下自动批准工具调用（[#4537](https://github.com/github/copilot-cli/issues/4537)），以及若干生命周期错误：`session/prompt` 会中止正在运行的后台子智能体（[#4555](https://github.com/github/copilot-cli/issues/4555)），并且 `end_turn` 在后台工作完成前触发（[#4743](https://github.com/github/copilot-cli/issues/4743)）。在成本与资源方面，BYOK 用户报告 1.0.82 中提示缓存被静默禁用（成本约 5 倍）（[#4720](https://github.com/github/copilot-cli/issues/4720)），而 WSL2 会话据报道消耗约 31 GB RSS（[#4694](https://github.com/github/copilot-cli/issues/4694)）。

## 发布

过去 24 小时内没有发布新版本。

## 热门 Issue

1. [**#4537 – ACP 模式再次自动批准工具调用（#845 的回归）**](https://github.com/github/copilot-cli/issues/4537) — [area:permissions] 自 1.0.81-1 起，`--acp` 模式下的智能体不再发送 `session/request_permission`；shell 命令、文件编辑和删除会在无人值守的情况下执行。这是一个安全敏感型回归（2 👍），对安全的智能体工作流有直接影响。

2. [**#4695 – HTTP 服务器的 MCP OAuth 令牌无法在会话间可靠复用**](https://github.com/github/copilot-cli/issues/4695) — [area:authentication, area:mcp] 令牌缓存会为同一 OAuth 服务器创建多个缓存键哈希，导致重复进行 PKCE 重新认证。长期运行的 MCP 设置中，社区有 5 条评论反映这一摩擦。

3. [**#4692 – CLI 未遵守默认企业模型**](https://github.com/github/copilot-cli/issues/4692) — [area:enterprise, area:models] 组织管理的默认设置（例如 MAI-Code-1.1-Flash）在 VS Code/Desktop 中可正确应用，但会被 CLI 静默忽略并回退到另一个模型——对于希望在单一模型上实现标准化组织来说，这会阻碍推广（4 条评论）。

4. [**#4527 – `copilot -p` 在 GHEC 数据驻留环境中返回 401（已关闭）**](https://github.com/github/copilot-cli/issues/4527) — 非交互式 prompt 模式从 `api.githubcopilot.com` 获取模型目录，而非租户端点，导致数据驻留租户上的自动化无法工作。使用相同凭据时，交互模式可以正常工作。该问题在社区关注后被关闭（4 👍，3 条评论）。

5. [**#4720 – BYOK 静默禁用提示缓存（成本约 5 倍）**](https://github.com/github/copilot-cli/issues/4720) — [area:networking, area:models] BYOK 模式下的 Copilot CLI 1.0.82 不会发送任何缓存声明：提供商使用情况显示 `cached_tokens=0`，因此每一轮都会以全价重新发送完整上下文。长时间会话会带来显著的财务影响。

6. [**#4694 – WSL2：约 31 GB RSS 和约 57% CPU 占用**](https://github.com/github/copilot-cli/issues/4694) — [area:platform-linux] 在 WSL2 上以 High Effort 长时间运行 Claude Opus 5，导致约 31 GB RSS、约 47% 上下文使用率——这显然是一种内存管理异常，使长时间会话变得不切实际。

7. [**#4555 – ACP：`session/prompt` 无条件中止会话及后台子智能体**](https://github.com/github/copilot-cli/issues/4555) — [area:sessions, area:agents] 在处理 prompt 请求之前会调用 `session.abort()`，从而终止通过 `task` 工具以 `mode=background` 启动的任务——这与交互式 TUI 的行为不一致。

8. [**#4743 – ACP：`end_turn` 先于后台 shell 完成；没有可观测的空闲信号**](https://github.com/github/copilot-cli/issues/4743) — 与 #4555 相关：当后台 shell 仍在运行时，`stopReason: "end_turn"` 就已到达；当该后台 shell 结束时，智能体在 prompt RPC 已经完成后自主调用工具并发出更新。客户端缺少可靠的会话空闲信号。

9. [**#4742 – 桌面应用 1.1.15：一个 Local 会话运行时无法创建第二个 Local 会话**](https://github.com/github/copilot-cli/issues/4742) — [triage] 自动升级后，“This project already has an active Local workspace”会阻止同一项目中并发的 branch 类型会话，从而使并行多会话工作流中断。

10. [**#4738 – `ask_user` 表单：过早按 Enter 会提交/取消并永久丢弃已输入内容**](https://github.com/github/copilot-cli/issues/4738) — [triage] 该问题被标记为高严重性：在 `ask_user` 表单中意外按下 Enter 会丢弃大量用户已输入内容，且没有自动保存或恢复途径。

## 关键 PR 进展

过去 24 小时内只有一个拉取请求处于活跃状态：

- [**#4739 – docs：提议由终端拥有 macOS 通知**](https://github.com/github/copilot-cli/pull/4739) — 一份参考提案（不是对已发布 CLI 的更改），记录 macOS 通知点击处理问题，附带一个原始 MIT 许可的终端通知示例和可移植回归测试。对于构建 CLI 通知 UI 的集成者很有用；目前还没有评审评论。

## 热门讨论

本摘要期间没有提供讨论数据。

## 功能需求趋势

- **更丰富的终端输入编辑**：用户希望获得 GUI/Emacs 风格的编辑行为——Shift+Arrow/Ctrl+A 文本选择（[#2644](https://github.com/github/copilot-cli/issues/2644)），以及 Ctrl+E 接受内联自动补全建议（[#4736](https://github.com/github/copilot-cli/issues/4736)）。交互式提示显然是一个主要交互面，需要与现代 shell 编辑功能看齐。

- **项目/仓库级插件**：现已关闭的问题（18 👍、14 条评论）（[#1665](https://github.com/github/copilot-cli/issues/1665)）反映出持续的需求：按仓库配置插件，而不仅是仅限按用户的全局插件。

- **确定性的 ACP 生命周期语义**：多个报告（[#4555](https://github.com/github/copilot-cli/issues/4555)、[#4743](https://github.com/github/copilot-cli/issues/4743)、[#4537](https://github.com/github/copilot-cli/issues/4537)）实际上是在要求一个更清晰的协议契约：显式地重新请求权限，并提供一个覆盖后台子智能体的可观测空闲/完成信号。

- **企业与数据驻留对齐**：关于组织管理的默认模型被忽略（[#4692](https://github.com/github/copilot-cli/issues/4692)）以及 GHEC 数据驻留中端点选择错误（[#4527](https://github.com/github/copilot-cli/issues/4527)）的报告表明，企业策略设置必须一致地应用于交互式、prompt 和 ACP 模式。

## 开发者痛点

- **智能体/ACP 模式中的信任回归**：静默自动批准 shell 命令和文件编辑（[#4537](https://github.com/github/copilot-cli/issues/4537)）破坏了智能体的安全使用；后台子智能体要么被提前杀死，要么在协议契约结束后仍在运行（[#4555](https://github.com/github/copilot-cli/issues/4555)、[#4743](https://github.com/github/copilot-cli/issues/4743)）。

- **隐藏的成本与资源飙升**：BYOK 会话中提示缓存丢失被报告为约 5 倍成本（[#4720](https://github.com/github/copilot-cli/issues/4720)）；而 WSL2 多 GB 级的 RSS 增长（[#4694](https://github.com/github/copilot-cli/issues/4694)）使长时间会话在本地开发中变得不切实际。

- **交互式 UI 中的用户内容丢失**：在 `ask_user` 表单中过早按 Enter 会永久丢弃已输入的答案（[#4738](https://github.com/github/copilot-cli/issues/4738)）；工具调用前的助手文本会被折叠为 “Thought for Ns” 而不再显示（[#4735](https://github.com/github/copilot-cli/issues/4735)）；响应在 `max_output_tokens` 处被截断，其继续请求也会丢失（[#4733](https://github.com/github/copilot-cli/issues/4733)）。

- **认证与企业配置摩擦**：由于缓存键重复，MCP OAuth 令牌会被反复重新签发（[#4695](https://github.com/github/copilot-cli/issues/4695)）；组织管理的模型默认值在 CLI 中被静默忽略（[#4692](https://github.com/github/copilot-cli/issues/4692)），迫使开发者手动覆盖并反复重新认证。

---

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区简报 — 2026-09-07

**来源：** github.com/anomalyco/opencode

## 1. 今日要点

可靠性问题是今天 issue 动态的主线：Bedrock Luna 的使用量被重复计入，导致自动压缩触发过于频繁（[#47296](https://github.com/anomalyco/opencode/issues/47296)）；而 Together AI 模型上报的 token 用量全部为零（[#47716](https://github.com/anomalyco/opencode/issues/47716)）。与此同时，Hona 的一组多 PR 渲染器持久化重构 —— [#47704](https://github.com/anomalyco/opencode/pull/47704)、[#47705](https://github.com/anomalyco/opencode/pull/47705) 和 [#47706](https://github.com/anomalyco/opencode/pull/47706) —— 已经关闭，该重构将大型草稿数据迁入内容寻址分块，以降低存储/数据库压力。新的报告还指出，数据库无界增长与泄漏的 `serve` 实例是长期运行部署中需要重视的严重问题。

## 2. 版本发布

过去 24 小时内没有发布新版本。

## 3. 热门 Issue

- [#1934 — Feat: Automatically run `aws sso login` when credentials need to be refreshed](https://github.com/anomalyco/opencode/issues/1934)  
  *已关闭，8 条评论，14 👍。* 对使用短时 AWS SSO 会话的团队来说，这是一项呼声很高的工作流改进；它仍是本窗口内反响最高的 issue。

- [#47296 — Bedrock GPT-5.6: usage total counts cached input twice, so auto-compaction fires after every message](https://github.com/anomalyco/opencode/issues/47296)  
  *开启中，3 条评论。* 这是服务商计量上一个很隐蔽的 bug，实际上会破坏 Bedrock Luna 用户的上下文连续性。修复 PR [#47354](https://github.com/anomalyco/opencode/pull/47354) 正是针对该问题。

- [#31942 — Agent loops 277 times on MCP resource_list without circuit breaker](https://github.com/anomalyco/opencode/issues/31942)  
  *已关闭，3 条评论。* 一个很能说明“缺少 Agent 循环保护”后果的极端案例；重复的 MCP 调用耗尽了整个工作区的 token 预算。

- [#47729 — opencode.db grows unbounded — event table (4.1GB after ~6 months) has no retention policy or cleanup command](https://github.com/anomalyco/opencode/issues/47729)  
  *开启中，1 条评论。* 对长期运行的安装来说，这是一个严重的运维隐患；`event` 表占据了绝大部分数据库体积，却没有任何清理途径。

- [#47727 — serve: per-request instances are never disposed — MCP child processes accumulate until memory exhaustion](https://github.com/anomalyco/opencode/issues/47727)  
  *开启中，1 条评论。* 对通过轮询式客户端运行 `opencode serve` 的用户很重要：每次请求都会泄漏实例和 MCP 子进程。

- [#47716 — Together AI: token usage always 0 — bundled @ai-sdk/togetherai doesn't send stream_options.include_usage](https://github.com/anomalyco/opencode/issues/47716)  
  *开启中，2 条评论。* 这让 Together AI 模型（包括 GLM、Kimi 变体）的 token/成本跟踪完全无法进行。

- [#47710 — opencode-desktop: Desktop GUI does not pass user shell PATH to plugin subprocesses](https://github.com/anomalyco/opencode/issues/47710)  
  *已关闭，2 条评论。* 从 macOS 桌面应用启动 OpenCode 时，依赖外部 CLI 可执行文件的插件会因此失效。

- [#47721 — [2.0] core: request accumulates 52 images, exceeds provider max of 50](https://github.com/anomalyco/opencode/issues/47721)  
  *开启中，1 条评论。* 这是 v2 beta 的一个问题：附加的图像资源不会随会话增长而清理，最终导致 provider 请求失败。

- [#37007 — Cannot interrupt/stop long-running commands executed via the bash tool](https://github.com/anomalyco/opencode/issues/37007)  
  *开启中，2 条评论，1 👍。* 长时间运行的前台命令会让 TUI 一直阻塞到超时；这与 [#41753](https://github.com/anomalyco/opencode/issues/41753) 相关，后者的新 prompt 会一直等待正在运行的 bash 工具。

- [#47714 — [2.0] cli: update prompt recommends older version](https://github.com/anomalyco/opencode/issues/47714)  
  *开启中，1 条评论。* 一个不大但会让 beta 用户感到困惑的 UX bug：`opencode2` v0.0.0-beta-19215 被提示更新到一个更旧的构建版本。

## 4. 关键 PR 进展

- [#47354 — fix(opencode): correct Bedrock Luna token usage](https://github.com/anomalyco/opencode/pull/47354)  
  通过避免重复计算 Bedrock 已计入的缓存输入 token，关闭了 [#47296](https://github.com/anomalyco/opencode/issues/47296)。

- [#47704 — perf(app): cache storage namespaces and batch writes in the renderer](https://github.com/anomalyco/opencode/pull/47704)  
  这是三个持久化层优化中的第一个，参考了 VS Code 的存储架构：每个命名空间只做一次批量加载，每个刷新窗口只做一次批量写入。

- [#47705 — perf(app): serialize persisted stores on a schedule instead of per setter call](https://github.com/anomalyco/opencode/pull/47705)  
  第二层：不再每次 setter 调用时都同步序列化，而是改为定时保存，并在 owner 清理和页面隐藏时执行 flush。

- [#47706 — perf(app): externalize large draft text into content-addressed chunks](https://github.com/anomalyco/opencode/pull/47706)  
  第三层：大型草稿被拆分为固定大小的内容寻址分块，这样在大量粘贴后继续输入时，每次保存都不必重新上传整个草稿。

- [#47724 — fix(desktop): surface default path opening errors](https://github.com/anomalyco/opencode/pull/47724)  
  关闭 [#47722](https://github.com/anomalyco/opencode/issues/47722)。现在会拒绝 Electron `shell.openPath()` 返回的非空错误，从而让文件管理器启动失败的问题暴露出来。

- [#47720 — fix(cli): resolve plugins in Node SEA builds](https://github.com/anomalyco/opencode/pull/47720)  
  通过既有的 VM 支撑导入器获取原生 ESM 解析器，修复 Node 单可执行应用（SEA）构建中的插件解析问题。

- [#47719 — feat(plugin): register template commands](https://github.com/anomalyco/opencode/pull/47719)  
  允许 Promise 和 Effect 插件通过 `template`、`agent`、`model` 和 `subagent` 配置来注册声明式命令。

- [#47663 — feat(plugin): add session title hook and request options bag](https://github.com/anomalyco/opencode/pull/47663)  
  这是按 LLM 请求类型拆分会话请求钩子的第一步；引入了一个共享的 `SessionRequestOptions` 结构。

- [#46530 — feat(plugin): expose permission assertions](https://github.com/anomalyco/opencode/pull/46530)  
  为插件新增 `ctx.permission.assert()`，并在有风险的操作之前检查规范化浏览器 URL、服务器文件读取以及外部目录访问。

- [#46534 — feat(core): add firecrawl developer search provider](https://github.com/anomalyco/opencode/pull/46534)  
  这是对原 Firecrawl 网页搜索 provider 的后续补充；新增了第二个面向开发者搜索类别的 provider。

## 5. 热门讨论

本摘要周期未提供讨论数据。

## 6. 功能需求趋势

从本期 issue 数据来看，呼声最高的功能方向是：

- **会话生命周期管理与数据保留**  
  用户希望存储有上限、能自动清理、记录可迁移：[#47729](https://github.com/anomalyco/opencode/issues/47729)、[#34875](https://github.com/anomalyco/opencode/issues/34875)、[#47713](https://github.com/anomalyco/opencode/issues/47713)。

- **云凭据自动化**  
  用户强烈要求自动刷新 AWS SSO 登录，而不是在失败后手动恢复登录：[#1934](https://github.com/anomalyco/opencode/issues/1934)。

- **更好的 Linux TUI 剪贴板支持**  
  多次有人要求内置或自动检测 `xclip`、`xsel` 或 `wl-clipboard`，让复制粘贴开箱即用：[#35977](https://github.com/anomalyco/opencode/issues/35977)、[#35978](https://github.com/anomalyco/opencode/issues/35978)。

- **Agent/工具护栏**  
  用户请求的防护措施包括：为重复的 MCP 工具调用加入熔断器，并防止 AI 导致配置文件损坏：[#31942](https://github.com/anomalyco/opencode/issues/31942)、[#35954](https://github.com/anomalyco/opencode/issues/35954)。

- **桌面端与 CLI 对齐**  
  用户期望桌面应用能像 CLI 一样继承 shell 环境（包括 `PATH`），并希望项目名称可配置、不必与文件夹名绑定：[#47710](https://github.com/anomalyco/opencode/issues/47710)、[#47708](https://github.com/anomalyco/opencode/issues/47708)。

## 7. 开发者痛点

- **模型服务商的用量统计 bug 正在破坏用户对上下文/成本跟踪的信任。** Bedrock Luna 会误触发自动压缩（[#47296](https://github.com/anomalyco/opencode/issues/47296)），而 Together AI 又上报零用量（[#47716](https://github.com/anomalyco/opencode/issues/47716)）。

- **无界增长是反复出现的生产环境风险。** `opencode.db` 的 `event` 表可达数 GB 级别（[#47729](https://github.com/anomalyco/opencode/issues/47729)），`opencode serve` 还会泄漏实例/MCP 进程直到内存耗尽（[#47727](https://github.com/anomalyco/opencode/issues/47727)）。

- **可中断性不足。** 开发者无法可靠地停止通过 bash 工具执行的长时命令，新的 prompt 只能在其后排队等待（[#37007](https://github.com/anomalyco/opencode/issues/37007)、[#41753](https://github.com/anomalyco/opencode/issues/41753)）。

- **桌面端/插件环境不一致会引发难以排查的故障。** macOS 桌面应用不将 shell `PATH` 传给插件子进程就是一个典型例子（[#47710](https://github.com/anomalyco/opencode/issues/47710)）。

- **Agent 循环与工具误用需要更强的内置保护。** 没有熔断器时，Agent 会在重复的 MCP 调用上耗尽 token 预算（[#31942](https://github.com/anomalyco/opencode/issues/31942)）；AI 编辑 `opencode.json` 还可能破坏配置（[#35954](https://github.com/anomalyco/opencode/issues/35954)）。

---

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区摘要 — 2026-09-07

## 今日亮点
过去 24 小时内没有发布新版本，但 issue 和 PR 活动非常密集。最受关注的修复包括：将 GitHub Copilot GPT 模型（含 GPT‑6 Astra）路由到 Responses API（[#9253](https://github.com/earendil-works/pi/pull/9253)），以及将 undici 的 DNS 解析固定到系统解析器，以支持 MagicDNS/Tailscale 风格的主机（[#9252](https://github.com/earendil-works/pi/pull/9252)）。社区里热度最高的两个讨论串依然是 openai-codex 卡在 `Working...` 上的问题（[#4945](https://github.com/earendil-works/pi/issues/4945)，76 条评论）和 Windows 体验问题汇总帖（[#7547](https://github.com/earendil-works/pi/issues/7547)，57 条评论）。

## 热门 Issue
- [#4945 — openai-codex Connection Reliability Issues](https://github.com/earendil-works/pi/issues/4945) *（开放中，进行中）* — 76 条评论，32 👍。这是本周期讨论热度最高的 issue：`openai-codex`/`gpt-5.5` 会间歇性地让 TUI 卡在 `Working...` 上，既没有流输出，也没有工具调用或错误提示。唯一恢复办法是按 Escape，但那会记录成一次已中止的轮次。已标记为进行中，但对日常用户来说显然非常折磨。
- [#7547 — How do you use Pi on Windows? What issues are you seeing?](https://github.com/earendil-works/pi/issues/7547) *（开放中）* — 57 条评论。这是由维护者发起的协调帖，请 Windows 开发者反馈各自的运行模式和故障点，目的很明确：决定核心工作应该投入在哪里，哪些问题又该交给扩展侧绕行解决。从回复规模来看，Windows 是目前未解决摩擦最多的平台。
- [#9052 — Fullscreen mode's fixed input box is great, but wheel scrolling is 3× slower](https://github.com/earendil-works/pi/issues/9052) *（开放中）* — 6 条评论，3 👍。用户为了固定输入框而选择全屏模式，结果却遇到明显的滚动性能回退。行为差距虽小，却挡住了一个广受欢迎的 UI 模式。
- [#8826 — Cap agent retry backoff for prolonged transient outages](https://github.com/earendil-works/pi/issues/8826) *（开放中）* — 4 条评论。在上游持续故障期间，指数退避重试会造成长时间等待并反复出现 `503 Too many open files` 错误。该 issue 请求增加可配置上限，让重试稳定在一个有界间隔。
- [#9209 — GPT‑6 Astra routed to unsupported Chat Completions endpoint](https://github.com/earendil-works/pi/issues/9209) *（已关闭）* — 4 条评论。Copilot 在 `/chat/completions` 上正确拒绝了 `gpt-6-astra`，返回 `unsupported_api_for_model`。已由 PR [#9253](https://github.com/earendil-works/pi/pull/9253) 修复。
- [#9229 — Windows: shell_path config is ignored, always prefer WSL bash](https://github.com/earendil-works/pi/issues/9229) *（已关闭）* — 4 条评论。即使关闭了 Windows 的 WSL 功能，Pi 仍会优先选择 WSL bash，而不是 `settings.json` 中显式配置的 `shell_path`——对运行原生 shell 的 Windows 用户来说，这是一个令人诧异的优先级 bug。
- [#8823 — Esc during active streaming often fails to cancel the in-flight request](https://github.com/earendil-works/pi/issues/8823) *（开放中）* — 3 条评论。Escape 已经记录中止，但 HTTP 请求会一直跑到 provider 自然结束为止，所以用户要么继续等待，要么最终得到一个本不想要的中止轮次。
- [#9165 — Claude Opus 5 via OpenRouter rejects per-message output_config](https://github.com/earendil-works/pi/issues/9165) *（已关闭）* — 3 条评论。`openrouter/anthropic/claude-opus-5` 对逐消息的 `output_config` 返回 400，而同一模型在 Anthropic 原生 provider 上正常。这说明请求在发给不同 provider 之前需要做针对性的清洗处理。
- [#9265 — O(n²) tool-call argument re-parsing in openai-completions streaming](https://github.com/earendil-works/pi/issues/9265) *（已关闭）* — 只有 1 条评论，但问题很严重：流处理器在收到每个 delta 时都会重新解析已累积的全部 tool-call JSON，在托管多个会话的单线程嵌入式守护进程中，这会让事件循环冻结。
- [#9246 — Spend the unused 4th Anthropic cache breakpoint on a stable conversation checkpoint](https://github.com/earendil-works/pi/issues/9246) *（已关闭）* — 3 条评论。Anthropic 允许设置四个缓存断点，而 Pi 目前只用了三个（system prompt、上一个工具、上一个用户消息）。提案是增加一个稳定的第四断点，以改善缓存命中的成本效益。

## 关键 PR 进展
- [#9253 — fix(ai): route Copilot GPT models through Responses (fixes astra)](https://github.com/earendil-works/pi/pull/9253) — 修复 [#9209](https://github.com/earendil-works/pi/issues/9209)，并移除对 Copilot GPT‑4 模型目录的过时假设；设计目标是在 GitHub 下架旧模型后依然保持正确。
- [#9261 — feat(ai): add sendStrictToolField compat flag](https://github.com/earendil-works/pi/pull/9261) — 把 strict 形态的 tool schema 与 `strict` 字段本身解耦，解除 Anthropic 兼容网关（如 AWS Bedrock 代理）的阻塞——这类网关往往要求前者，却拒绝后者。
- [#9259 — feat(coding-agent): apply a steering message promptly by interrupting the running turn](https://github.com/earendil-works/pi/pull/9259) — 允许用户在工具调用中途用引导消息调整 agent 的方向（比如“换一个更快的镜像”“换个思路”），而不是让这条消息一直排队等漫长的安装/构建结束。
- [#9251 — feat(coding-agent): hop to a fallback provider on transport errors](https://github.com/earendil-works/pi/pull/9251) — 在当前 provider 不可达（传输错误/超时/DNS）时，提供可选的跨 provider 回退，解决 [#9242](https://github.com/earendil-works/pi/issues/9242)。早期迭代见 [#9248](https://github.com/earendil-works/pi/pull/9248)、[#9249](https://github.com/earendil-works/pi/pull/9249)。
- [#9252 — fix(coding-agent): pin undici connect lookup to system dns.lookup](https://github.com/earendil-works/pi/pull/9252) — 通过遵守操作系统解析器/nsswitch，修复 MagicDNS/分离解析（split-horizon）主机的 `ENOTFOUND` 错误。与 [#9250](https://github.com/earendil-works/pi/pull/9250) 重复。
- [#9233 — fix(coding-agent): resolve model auth live instead of from startup snapshot](https://github.com/earendil-works/pi/pull/9233) — 修复启动期的一个竞态：未等待的后台刷新会让 `hasConfiguredAuth()` 为空，从而阻塞模型解析。
- [#6881 — feat(ai): use provider-reported cost when responses include it](https://github.com/earendil-works/pi/pull/6881) *（进行中，自 7 月起开放）* — 当响应中包含 `usage.cost` / `cost_details.upstream_inference_cost` 时读取该值，否则维持现有 `calculateCost` 回退逻辑。对 BYOK/上游成本准确性很有用。
- [#9080 — feat(tui): add jump-to-latest control](https://github.com/earendil-works/pi/pull/9080) — 为超长流式记录提供“跳到最新/新消息”的控件，用户不必手动滚动去跟上 agent。
- [#9227 — feat(coding-agent): add per-call tool confirmation extension](https://github.com/earendil-works/pi/pull/9227) — 允许扩展在单个工具调用前介入确认，天然适合对安全性有要求的自动化层。
- [#9137 — feat(coding-agent): add Nix flake](https://github.com/earendil-works/pi/pull/9137) *（WIP，开放中）* — 为可复现开发环境新增 Nix flake；目前仍标记为 WIP。

## 热门讨论
### 想法
- [#9146 — Provide a per-repo override for API Key and ignore auth.json](https://github.com/earendil-works/pi/discussions/9146) — 2 条评论，1 👍。发帖人把 OpenRouter key 存在 1Password 里，希望支持按仓库覆盖 API key，而不是只靠单个全局 `auth.json` 条目。对需要为不同项目使用不同 provider 账号的场景很有用。

## 功能需求趋势
- **更抗故障的 agent 运行时**：传输错误时的跨 provider 回退（[#9241/#9242](https://github.com/earendil-works/pi/issues/9242)、PR [#9251](https://github.com/earendil-works/pi/pull/9251)）、故障期间的有界重试退避（[#8826](https://github.com/earendil-works/pi/issues/8826)）、实时鉴权解析（[#9233](https://github.com/earendil-works/pi/pull/9233)）以及 provider 上报的成本（[#6881](https://github.com/earendil-works/pi/pull/6881)），都指向同一个目标：让 agent 主循环能扛住上游抖动，无需用户守着。
- **运行中途的用户控制**：用引导消息打断正在运行的轮次（[#9260](https://github.com/earendil-works/pi/issues/9260)、PR [#9259](https://github.com/earendil-works/pi/pull/9259)）、可靠的 Escape 取消（[#8823](https://github.com/earendil-works/pi/issues/8823)），以及跳转到最新内容的 TUI 导航（[#9080](https://github.com/earendil-works/pi/pull/9080)）。
- **把 Windows 当作一等平台**：57 条评论的 Windows 汇总帖（[#7547](https://github.com/earendil-works/pi/issues/7547)）、遵守 `shell_path` 配置（[#9229](https://github.com/earendil-works/pi/issues/9229)）、CRLF 规范化（[#9264](https://github.com/earendil-works/pi/issues/9264)）、能处理反斜杠的 `find` glob（[#9262](https://github.com/earendil-works/pi/issues/9262)），以及 Shift+Enter 处理（[#7175](https://github.com/earendil-works/pi/issues/7175)）。
- **更广的模型/网关覆盖范围**：GPT‑6 Astra 支持（[#9133](https://github.com/earendil-works/pi/issues/9133)、[#9209](https://github.com/earendil-works/pi/issues/9209)）、OpenRouter/Anthropic 兼容（[#9165](https://github.com/earendil-works/pi/issues/9165)）、OpenCode Go 新增的 `x-opencode-session` 请求头（[#9230](https://github.com/earendil-works/pi/issues/9230)、[#9237](https://github.com/earendil-works/pi/issues/9237)）、strict schema 网关兼容标志（[#9263](https://github.com/earendil-works/pi/issues/9263)、PR [#9261](https://github.com/earendil-works/pi/pull/9261)），以及新增的 LLM Gateway providers（[#7610](https://github.com/earendil-works/pi/pull/7610)）。
- **扩展 API 持续进化**：带确认且幂等的用户轮次投递（[#9236](https://github.com/earendil-works/pi/issues/9236)）、运行时 TUI 模式切换与布局根（[#9238](https://github.com/earendil-works/pi/issues/9238)）、单次工具调用确认钩子（PR [#9227](https://github.com/earendil-works/pi/pull/9227)）。
- **提示词缓存与成本调优**：利用 Anthropic 的第四个缓存断点（[#9246](https://github.com/earendil-works/pi/issues/9246)），并避免 `before_agent_start` 反复抖动破坏会话唤醒时的提示词缓存（[#8712](https://github.com/earendil-works/pi/issues/8712)）。

## 开发者痛点
- **openai-codex / gpt-5.5 卡死**是最强烈的抱怨：TUI 一直停在 `Working...`，既无输出也无报错，只能按 Escape，而这一轮次就被丢弃（[#4945](https://github.com/earendil-works/pi/issues/4945)，32 👍）。
- **Windows 的摩擦面广且反复出现**：需要支持的运行模式太多（[#7547](https://github.com/earendil-works/pi/issues/7547)）、即使在功能中关闭了 WSL 也优先选择 WSL bash 而忽略 `shell_path`（[#9229](https://github.com/earendil-works/pi/issues/9229)）、CRLF 把 `\r` 泄漏进工具文本（[#9264](https://github.com/earendil-works/pi/issues/9264)）、`find` 对反斜杠 glob 静默返回空结果（[#9262](https://github.com/earendil-works/pi/issues/9262)）。
- **取消功能不可信赖**：流式传输中按 Escape 往往不会真正中断 HTTP 请求，只能等待 provider 自然跑完（[#8823](https://github.com/earendil-works/pi/issues/8823)）。
- **TUI 渲染回退**：全屏模式的滚轮滚动比普通模式慢 3×（[#9052](https://github.com/earendil-works/pi/issues/9052)）；在视口上方编辑会触发破坏性的整屏重绘（[#9240](https://github.com/earendil-works/pi/issues/9240)）；恢复会话时保存的截图会按完整内联尺寸重新渲染（[#9256](https://github.com/earendil-works/pi/issues/9256)）。
- **网关/模型的“打地鼠”问题**：Copilot 在 Chat Completions 上拒绝 `gpt-6-astra`（[#9209](https://github.com/earendil-works/pi/issues/9209)）；OpenRouter 拒绝 Claude Opus 5 的逐消息 `output_config`（[#9165](https://github.com/earendil-works/pi/issues/9165)）；OpenCode Go 现在强制要求 `x-opencode-session` 请求头（[#9230](https://github.com/earendil-works/pi/issues/9230)、[#9237](https://github.com/earendil-works/pi/issues/9237)）。
- **典型云环境之外的 DNS 与环境陷阱**：undici 无法解析 MagicDNS/Tailscale 主机（[#9244](https://github.com/earendil-works/pi/issues/9244)）；`models.json` 不会解析 `apiKey` 中的 `$ENV` 占位符，而是把字面字符串发出去导致 401（[#9258](https://github.com/earendil-works/pi/issues/9258)）。
- **嵌入式负载下的性能**：openai-completions 流处理器会在每个 delta 上重新解析累积的 tool-call JSON，导致 O(n²) 开销，并让单线程守护进程的事件循环冻结（[#9265](https://github.com/earendil-works/pi/issues/9265)）。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区摘要 — 2026-09-07

## 今日亮点

今日发布的版本聚焦于 WebShell 工作流的可见性：预览版和 nightly 版都具备了可视化和动态管理工作流运行的新能力。稳定性方面，新报告的 git 分支回滚缺陷（#11253）可能会丢弃由失败的 `post-checkout` 钩子所产生的提交；对应的修复（#11258）已经开启。与此同时，社区持续开展的工作重点关注预算透明度、WebShell 可观测性，以及更安全的并发守护进程/会话处理。

## 版本发布

- [v0.23.1-preview.1](https://github.com/QwenLM/qwen-code/releases/tag/v0.23.1-preview.1)
- [v0.23.0-nightly.20260906.92a8a8d179](https://github.com/QwenLM/qwen-code/releases/tag/v0.23.0-nightly.20260906.92a8a8d179)

这两个版本包含相同的两项改动：

- **feat(web-shell)：可视化并管理动态工作流运行** — [#10594](https://github.com/QwenLM/qwen-code/pull/10594)
- **perf(web-shell)：推导会话工作流项目**

发布说明中未标记任何破坏性变更。

## 热门议题

24 小时窗口内仅有 6 个议题有更新，全部列在下方。

- [#3361](https://github.com/QwenLM/qwen-code/issues/3361) — **[OPEN]** Agent 在命令成功执行后仍将 shell 输出误判为空（兼容 OpenAI 的 API）
  Agent 可以执行 `git rev-parse --show-toplevel` 等命令，也能在 UI 中看到可见输出，却仍然判定输出为空，进而对工具状态做出错误假设。该议题于 4 月创建，目前仍处于 `status/needs-triage`；从 6 条评论来看，这仍是一个尚未解决的集成痛点。

- [#7167](https://github.com/QwenLM/qwen-code/issues/7167) — **[OPEN]** Fleet Shepherd Dashboard
  一个自动维护的 CI/机器人集群仪表盘，用于跟踪 PR 的 head 状态、同步、调度、发布与清理。该功能主要供维护者参考，但可以帮助发现过期或卡住的自动化流程。目前处于 `status/need-information`。

- [#11253](https://github.com/QwenLM/qwen-code/issues/11253) — **[OPEN]** 分支创建回滚可能丢弃由失败的 post-checkout 钩子产生的提交
  高影响的 git 安全缺陷：当 `gitCreateBranch` 回滚一次失败的 checkout 操作时，可能会丢弃仓库 `post-checkout` 钩子所产生的提交。对应的修复已通过 [#11258](https://github.com/QwenLM/qwen-code/pull/11258) 进行中。标记为 P2，有 2 条评论。

- [#11243](https://github.com/QwenLM/qwen-code/issues/11243) — **[OPEN]** feat(daemon)：在 WebShell 中暴露更新状态并自动跟随 nightly 发布
  用户希望长时间运行的 CLI/守护进程能在 WebShell 内暴露更新状态和更新操作，并提供可选的自动跟随 nightly 发布机制。这是对更好的后台自动化生命周期管理的直接需求。

- [#4114](https://github.com/QwenLM/qwen-code/issues/4114) — **[OPEN]** 根据返回为空 (“returns empty”)
  另一个空输出报告，这次发生在多文件 `read_file` 探索时。Agent 在读取项目文件后，若被要求总结问题，似乎会返回空结果。与 [#3361](https://github.com/QwenLM/qwen-code/issues/3361) 类似，这都指向长期存在的工具输出解读缺口。

- [#11249](https://github.com/QwenLM/qwen-code/issues/11249) — **[OPEN]** main CI 失败：Qwen Code CI @ 421393d51df6
  `main` 分支的一次 CI 运行在报告任何测试结果之前即失败，发生在 `Test (ubuntu-latest, Node 22.x)` 任务中。机器人按提交跟踪该失败，并已将其标记为 `ready-for-agent`，状态为 `autofix/in-progress`。

## 关键 PR 进展

- [#11258](https://github.com/QwenLM/qwen-code/pull/11258) — **fix(core)：当 checkout 钩子失败时保留分支提交**
  直接应对 [#11253](https://github.com/QwenLM/qwen-code/issues/11253) 中的数据丢失场景。回滚过程会禁用钩子以恢复原始分支，将新分支引用与预期起始 tip 进行比较，并且仅在安全时才执行删除。

- [#11144](https://github.com/QwenLM/qwen-code/pull/11144) — **fix(cli)：让实时转录读取排在工具结果写入之后**
  防止在 ACP agent 路径中，并发会话加载观察到只写入一半的工具轮次。现在清理流程经由共享的 `finalizeDanglingForRestore` 辅助函数完成，提升了转录一致性。

- [#10237](https://github.com/QwenLM/qwen-code/pull/10237) — **fix(core)：防止任务所有者重复分发**
  阻止 leader 将一个进行中的任务分发给多个队友。所有权检查现在会在持有任务锁时进行，从而减少多 Agent 协作中的竞态条件。

- [#10906](https://github.com/QwenLM/qwen-code/pull/10906) — **feat(web-shell)：展示 shell 与 monitor 任务输出**
  让 shell 和 monitor 的 stdout/stderr 能在 WebShell 任务详情面板中直接读取。守护进程还新增了一个经过清理、以实时会话所有者为作用域的输出 tail 端点。

- [#11254](https://github.com/QwenLM/qwen-code/pull/11254) — **feat(web-shell)：展示 Goal 已用预算与其允许额度的对比**
  在 WebShell 状态条和 Goals 对话框中添加 token 预算显示，例如 `1.2k / 30.0M tokens`。这让用户能更清楚地看到自主 Goal 的开销情况。

- [#11257](https://github.com/QwenLM/qwen-code/pull/11257) — **feat(goal)：在延续提示中携带预算数字与进度指导**
  Goal 驱动的轮次现在会以剩余预算、已执行轮数和自检指导作为开场。这是 [#11254](https://github.com/QwenLM/qwen-code/pull/11254) 的提示侧配套改动，有助于让自主 Agent 保持在允许的额度内。

- [#11207](https://github.com/QwenLM/qwen-code/pull/11207) — **feat(serve)：允许通过会话隔离并发运行独立守护进程**
  让更新后的守护进程能够共享 Conversations，并同时运行不同的独立会话，同时为每个已加载会话保留强制性的单写者租约。对多守护进程环境非常重要。

- [#11090](https://github.com/QwenLM/qwen-code/pull/11090) — **feat(ipc)：允许用户铸造的 controller token 在无需逐条审查的情况下驱动会话**
  已在本窗口内关闭。该改动细化了 IPC 审查门禁，使受信任的 controller 会话无需逐条批准即可运行，同时仍然会阻止没有审查类别的会话——这是一项有意义的安全/可用性权衡改进。

- [#10999](https://github.com/QwenLM/qwen-code/pull/10999) — **feat(core)：配置模型推理能力**
  为 provider 模型定义增加声明式推理支持，并将其贯穿到 ACP、会话恢复、工作区预览、TUI 的 effort 控件和最终的 OpenAI 兼容请求中。该改动启用了原生 `deepseek-v4-pro` 推理入口。

- [#10347](https://github.com/QwenLM/qwen-code/pull/10347) — **feat(core)：在 Ctrl+Y 不可用时自动重试瞬时网络错误（EOF）**
  将 `400 network error ... EOF` 这类包装过的底层网络故障视为可重试的传输错误，而不是快速失败的客户端错误。对非交互式自动化场景尤其有用。

## 功能需求趋势

- **WebShell 作为控制平面**
  多项 PR 和议题正在推动 WebShell 从聊天功能走向完整的会话/任务管理：会话概览改进、分栏导航、shell/monitor 输出可见性、产物图标、上下文使用面板以及更新状态控制。议题 [#11243](https://github.com/QwenLM/qwen-code/issues/11243) 是最明确的直接需求：在 WebShell 中暴露守护进程更新状态，并允许自动跟随 nightly 发布。

- **自主 Goal 的预算与上下文透明度**
  社区明显在推动让 Goal 的执行具备财务和上下文层面的可观察性。[#11254](https://github.com/QwenLM/qwen-code/pull/11254) 和 [#11257](https://github.com/QwenLM/qwen-code/pull/11257) 增加了可见的 token 预算以及提示层面的预算指导，让长时间运行的 Agent 能够自检进度。

- **并发、所有权与回滚安全**
  多项贡献聚焦于并发执行下的正确性：防止多 Agent 任务重复分发、跨守护进程隔离会话写入、正确绑定 mesh 转录，以及在失败回滚期间保留 git 提交。这表明生态正在朝向更多并行和自主的工作负载发展，而竞态条件在这些场景中将变得更加危险。

- **对瞬时基础设施故障的韧性**
  网络 EOF 自动重试、为不稳定的 E2E 分片增加 CI 重试，以及更好地处理钩子创建的 git 提交，都体现了更广泛的诉求：减少由环境级故障而非代码级缺陷所造成的中断。

## 开发者痛点

- **工具输出有时会被误判为空**
  [#3361](https://github.com/QwenLM/qwen-code/issues/3361) 和 [#4114](https://github.com/QwenLM/qwen-code/issues/4114) 都描述了命令或文件读取明显返回了数据、但 Agent 却将结果视为空的情况。这对 shell 驱动的工作流尤其有害，并且这些议题已开放数月。

- **Git 操作回滚可能丢失工作成果**
  [#11253](https://github.com/QwenLM/qwen-code/issues/11253) 突出一个危险的边界情况：在 `post-checkout` 钩子失败后进行分支创建回滚可能会丢弃提交。对于使用 git hooks 的开发者而言，这会造成难以察觉的“静默数据丢失”风险。

- **CI 不稳定持续消耗维护者精力**
  [#11249](https://github.com/QwenLM/qwen-code/issues/11249) 显示了一次 main 分支 CI 失败，发生在任何测试结果上报之前。再加上持续进行的 macOS E2E 分片重试工作以及 Prettier 门禁格式修复，不稳定及基础设施级的 CI 失败仍是反复出现的摩擦来源。

- **长时间运行的守护进程/任务可观测性仍不完整**
  用户希望有更好的方式来了解后台 Agent、守护进程和 Goal 正在做什么：已消耗多少预算、是否正在等待审查、是否存在更新的 nightly 构建。在该状态完全暴露到 WebShell 之前，用户只能依赖日志或外部进程。

- **多 Agent 与会话状态竞态是真实存在的问题**
  涉及重复任务分发、转录读写顺序和会话隔离的 PR 数量表明，随着多 Agent 和守护进程化用法的增长，并发缺陷正开始浮现。开发者正在推动更严格的所有权和可线性化的会话状态。

</details>