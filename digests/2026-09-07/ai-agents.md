# OpenClaw 生态日报 2026-09-07

> Issues: 144 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-09-07 04:41 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-09-07

## 1. 今日概览

2026-09-07 的 OpenClaw 活跃度非常高：500 个 PR 被更新，其中 **225 个合并/关闭**；144 个 issue 被变更（**89 个关闭**，55 个开放/活跃）。今天没有发布新版本；issue 报告显示当前稳定版为 **2026.9.2**；发布验证 CI 中引用了一个 **2026.9.3 候选版本**，其阻塞由 PR [#140794](https://github.com/openclaw/openclaw/pull/140794) 解除。大量 issue 关闭来自 stale 生命周期自动化（例如 `stale`、`clawsweeper:no-new-fix-pr`、`clawsweeper:needs-product-decision` 等标签），这表明团队在积极清理积压事项，但被推迟的产品决策也在不断增加。开发工作主要集中在 Agent/会话生命周期正确性（子 Agent 分发、回复权、工具顺序执行强制），渠道修复（Discord、WhatsApp/iMessage/Signal 审批），认证/目录加固（xAI/Grok OAuth），以及客户端打磨（Android、macOS、iOS）。

## 2. 发布

无。2026-09-07 没有发布任何新版本。下一个预期版本是 **2026.9.3**，目前正在进行完整发布验证；PR [#140794](https://github.com/openclaw/openclaw/pull/140794)（修复发布扫描器接收已复核清理测试夹具的问题）已提交，以解除该候选版本的阻塞。

## 3. 项目进展

今天整体合并/关闭吞吐量很高（225 个 PR 合并/关闭）。在前 30 个 PR 列表中，值得注意的已关闭项包括：

- **工作树诊断修复**：PR [#140251](https://github.com/openclaw/openclaw/pull/140251) 保留精确的 “no commits” 错误，而不是通用的 “not a git checkout” 消息（关闭 issue [#140204](https://github.com/openclaw/openclaw/issues/140204)）。
- **CLI**：PR [#140789](https://github.com/openclaw/openclaw/pull/140789) 澄清了 `openclaw telemetry show` 只是本地预览。
- **CI**：PR [#140751](https://github.com/openclaw/openclaw/pull/140751) 避免 iOS 截图 CI 中重复构建 Watch。
- **macOS 应用**：PR [#140756](https://github.com/openclaw/openclaw/pull/140756) 将手工样式的 Connection 窗口重构为标准 Settings 窗口。

仍开放并等待合并/审查的主要修复和功能：

- **子 Agent 生命周期**：PR [#140137](https://github.com/openclaw/openclaw/pull/140137) 在结算时派发嵌套请求者的 yield-batch 唤醒（关闭 #139963）；PR [#140158](https://github.com/openclaw/openclaw/pull/140158) 修复当已捕获回复操作失效时的回复权回退逻辑（关闭 #139847）。
- **Code Mode**：PR [#140767](https://github.com/openclaw/openclaw/pull/140767) 使 Code Mode 在并行调用时仍然遵循“仅顺序执行工具”限制。
- **渠道**：PR [#140645](https://github.com/openclaw/openclaw/pull/140645) 在 WhatsApp/iMessage/Signal 之间共享审批 reaction 的结算逻辑（关闭 #131004）；PR [#140798](https://github.com/openclaw/openclaw/pull/140798) 增加显式受信任的 Discord 管理员；PR [#140653](https://github.com/openclaw/openclaw/pull/140653) 让 Grok 令牌发现仍走订阅路由。
- **会话**：PR [#140748](https://github.com/openclaw/openclaw/pull/140748) 将置顶限制为根会话；PR [#140793](https://github.com/openclaw/openclaw/pull/140793) 区分删除会话与重置会话的归档保留策略；PR [#140733](https://github.com/openclaw/openclaw/pull/140733) 避免为会话存储加载完整 Agent 运行时。
- **客户端**：PR [#140796](https://github.com/openclaw/openclaw/pull/140796) 和 [#140759](https://github.com/openclaw/openclaw/pull/140759) 修复 Android 模型设置过期，以及流式输出期间阅读位置丢失的问题。
- **新能力**：PR [#139850](https://github.com/openclaw/openclaw/pull/139850) 增加 Team Reports 插件，用于生成 GitHub/Discord 活动报告；PR [#119773](https://github.com/openclaw/openclaw/pull/119773) 为支持视觉的 provider 增加共享的结构化图像提取功能。
- **基础设施/维护**：PR [#140278](https://github.com/openclaw/openclaw/pull/140278) 将托管 llama.cpp 升级到 b10809；PR [#140781](https://github.com/openclaw/openclaw/pull/140781) 在 OpenAI-completions 传输层应用 Anthropic 缓存标记；PR [#138405](https://github.com/openclaw/openclaw/pull/138405) 让 memory-core 的 dream-diary 超时可配置化。

## 4. 社区热门话题

- **[#79077 — Telegram bot-to-bot 与访客 bot 支持](https://github.com/openclaw/openclaw/issues/79077)** — 14 条评论，**8 个 👍（反应数最高项）**。用户希望与 Telegram 2026 年 5 月的平台发布保持功能对齐（访客 bot、bot-to-bot 通信）。该 issue 已作为 `stale` 关闭，等待产品决策；这是数据中给出的最明确路线图需求信号。
- **[#97616 — 僵尸子进程泄漏](https://github.com/openclaw/openclaw/issues/97616)** — 14 条评论，P1 回归。hook/工具子进程（bash、codex、hooks）从未被回收，在主 gateway 下不断累积僵尸进程，导致运行时性能下降。正在等待更多信息（`clawsweeper:needs-info`）；尚无修复 PR。
- **[#140010 — Windows 睡眠/恢复后重连失败](https://github.com/openclaw/openclaw/issues/140010)** — 9 条评论，P1。系统唤醒后，UI/WebSocket 重连会因 gateway 忙碌、事件循环停顿而卡住 30–60 秒以上。尚无修复 PR。
- **[#140535 — Discord `/new` 返回“No reply was generated”](https://github.com/openclaw/openclaw/issues/140535)** — 7 条评论，P1。内置 `/new` 命令会收到确认，但不会重置频道会话。已关闭。
- **[#78963 — WhatsApp 仅监听/仅 hooks 模式](https://github.com/openclaw/openclaw/issues/78963)** — 7 条评论，1 个 👍。用户反复提出需要仅入站消息摄取能力（用于归档/ETL），不运行 Agent，也不调用 LLM；已作为 stale 关闭，等待产品决策。
- **[#140497 — Discord 设置把 application ID 当作 bot token 接受](https://github.com/openclaw/openclaw/issues/140497)** — 在一个 **P0 开放 issue** 上有 5 条评论；设置过程显示为 “configured/stopped” 且 `lastError: null`，看起来像卡住而不是失败。
- **[#140129 — Anthropic 长会话缓存卡在约 46k 前缀](https://github.com/openclaw/openclaw/issues/140129)** — 5 条评论；每一轮都会重写完整历史（`cache_create` 377k–386k），意味着大量成本与延迟浪费。

底层需求：可靠的长时运行进程/会话语义、平台功能对齐，以及失败时应明确报错的上手引导 UX。

## 5. 缺陷与稳定性

按严重程度排序（P0 优先）。有修复 PR 的项已注明。

**P0**
- [开放] [#140497](https://github.com/openclaw/openclaw/issues/140497)：Discord 设置过程把 application ID 当作 bot token 保存；频道显示为 enabled/configured/stopped，没有错误。上手流程会让用户陷入静默卡死。需要线上复现。
- [已关闭] [#140482](https://github.com/openclaw/openclaw/issues/140482)：xAI OAuth 登录用 xAI API 目录覆盖了正常可用的 Grok OAuth 目录（`baseUrl` 漂移）。已作为“修复方案明确/可排队修复”关闭。
- [已关闭] [#140393](https://github.com/openclaw/openclaw/issues/140393)：2026.9.2 的上手流程安装了 Codex，但首次 dashboard 聊天因缺少已准备运行时而失败。
- [已关闭] [#106920](https://github.com/openclaw/openclaw/issues/106920)：`openclaw update` 导致 2026.7.1 中的 gateway 无法重启（回归；5 个 👍）。
- [已关闭] [#96203](https://github.com/openclaw/openclaw/issues/96203)：Gateway 在大型工作区、默认 Node 堆（约 4GB）下约每 10–12 秒崩溃循环一次。

**P1**
- [开放] [#97616](https://github.com/openclaw/openclaw/issues/97616)：hook/工具子进程未被回收，导致僵尸进程累积和运行时性能下降（回归）。尚无修复 PR。
- [开放] [#140010](https://github.com/openclaw/openclaw/issues/140010)：Windows 睡眠/恢复后，gateway 在 30–60 秒以上无法从 UI/WebChat 访问。尚无修复 PR。
- [开放] [#118018](https://github.com/openclaw/openclaw/issues/118018)：过期的子 Agent 完成结果可能被投递到已替换的请求方生命周期中，并在无错误的情况下完成结算。相关修复 PR [#140137](https://github.com/openclaw/openclaw/pull/140137) 正在开放中。
- [开放] [#121232](https://github.com/openclaw/openclaw/issues/121232)：memory-core dreaming 一直报告 “Ranked N, Promoted 0 forever”——ranker 与 applier 在结构上相互矛盾，但没有任何机制暴露这一冲突。
- [已关闭，含修复] [#140466](https://github.com/openclaw/openclaw/issues/140466)：xAI 的 `auto` 别名被规范化成 canonical 模型后，在运行时认证重新物化时失败。
- [已关闭，含修复] [#137690](https://github.com/openclaw/openclaw/issues/137690)：2026.8.2 上，Telegram 来源会话执行 `sessions_spawn` 时返回 “unknown parent session” 错误。
- [已关闭] [#140535](https://github.com/openclaw/openclaw/issues/140535)：Discord `/new` 不重置会话，并返回误导性的回退错误。

**P2 值得注意项**
- [已关闭，含修复] [#140214](https://github.com/openclaw/openclaw/issues/140214)：`memory.search.extraPaths` 静默忽略已配置的符号链接根目录（QMD 迁移后 Obsidian vault 缺失）。
- [已关闭，含修复] [#140416](https://github.com/openclaw/openclaw/issues/140416)：在 package-root cwd 之外单独使用 `--import tsx` 会破坏 worker 启动。
- [开放] [#140129](https://github.com/openclaw/openclaw/issues/140129)：Anthropic prompt 缓存停留在约 46k 的 tools+system 前缀；每轮都会重写整个对话。
- [开放] [#102078](https://github.com/openclaw/openclaw/issues/102078)：本地 MLX 上上下文压缩失败，报错 “Thread group size (1024) exceeds maximum (896)”，导致会话阻塞。

总体来看，今天的 bug 流入偏向**会话状态生命周期正确性**、**认证/目录状态漂移**以及**静默配置错误**；其中多个“修复方案明确”的项目已被关闭，说明对可复现 bug 已经形成了快速分诊管线。

## 6. 功能请求与路线图信号

需求最高的用户请求：

- **Telegram 访客 bot 与 bot-to-bot**（[#79077](https://github.com/openclaw/openclaw/issues/79077)）：8 个 👍，已作为 stale 关闭——需要产品决策。鉴于平台功能对齐的历史优先级，尽管被标记为 stale，这仍可能是近期的路线图项目。
- **WhatsApp 仅监听/仅 hooks 模式**（[#78963](https://github.com/openclaw/openclaw/issues/78963)）：反复出现的 ETL/归档使用场景；已作为 stale 关闭，等待产品决策。
- **Control UI 模型选择器中的本地 vs. 云端来源标识**（[#122403](https://github.com/openclaw/openclaw/issues/122403)）：这是一个很小的 UX 改动，且数据 OpenClaw 已经具备——很可能是未来版本的候选。
- **飞书/Microsoft Teams/Mattermost 的原生审批按钮**（[#104521](https://github.com/openclaw/openclaw/issues/104521)）：在文本输入式审批重构后，恢复可点击的 `/approve` UX；注意，跨渠道共享结算重构 PR [#140645](https://github.com/openclaw/openclaw/pull/140645) 已经针对其他渠道开放。
- **手动清除工具结果的上下文**（[#45503](https://github.com/openclaw/openclaw/issues/45503)）：P3，自 3 月以来一直 stale；针对大型临时工具输出的效率诉求。
- **中文输入错别字/语法检测**（[#82011](https://github.com/openclaw/openclaw/issues/82011)）：P3，仍开放，维护者未处理。
- **MCP `notifications/tools/list_changed` + HTTP 重新加载端点**（[#91556](https://github.com/openclaw/openclaw/issues/91556)）：由一家规模化运维客户（大规模 Composio MCP）推动。

PR 信号表明近期方向偏向：**跨渠道审批 UX 规范化**（[#140645](https://github.com/openclaw/openclaw/pull/140645)）、**Discord 管理员/信任控制**（[#140798](https://github.com/openclaw/openclaw/pull/140798)）、**会话数据保留控制**（[#140793](https://github.com/openclaw/openclaw/pull/140793)），以及 **provider 行为修复**（xAI/Grok 订阅路由，[#140653](https://github.com/openclaw/openclaw/pull/140653)）。

## 7. 用户反馈摘要

- **长会话的成本敏感性**：Anthropic 缓存问题（[#140129](https://github.com/openclaw/openclaw/issues/140129)）描述了长会话中约 46k 的固定缓存前缀，以及每轮 377k–386k 的 `cache_create` 重写——用户实际上是在反复为完整历史记录的重新上传付费。这是一个很强的成本/延迟痛点。
- **静默失败模式正在侵蚀信任**：Discord 设置卡住而不报错（[#140497](https://github.com/openclaw/openclaw/issues/140497)）、memory-core dreaming 一直显示 “0 promoted forever” 且不暴露矛盾（[#121232](https://github.com/openclaw/openclaw/issues/121232)）、僵尸进程不断累积（[#97616](https://github.com/openclaw/openclaw/issues/97616)）都有一个共同主题：失败没有大声地暴露出来。
- **发布节奏带来的挫败感**：[#102311](https://github.com/openclaw/openclaw/issues/102311) 记录了一个合并到 `main` 的修复（Telegram 出站文件名 UUID 后缀，#96538）从未进入稳定版发布；两次重新打开请求均未获回应。用户已经开始注意到 `main` 与 release 构建之间的差距。
- **平台功能对齐与管理员 UX 需求**：Telegram 功能获得 8 个 👍（[#79077](https://github.com/openclaw/openclaw/issues/79077)）；运维人员希望把 Discord 作为完整控制面（PR [#140798](https://github.com/openclaw/openclaw/pull/140798) 是回应）；自托管用户希望在模型选择器中区分本地与云端来源（[#122403](https://github.com/openclaw/openclaw/issues/122403)）。
- **可观测性缺口**：vLLM 用量/成本页面不显示数据（[#87110](https://github.com/openclaw/openclaw/issues/87110)）；`/context detail` 无法解释新会话中约 62k 个未跟踪 token（[#86819](https://github.com/openclaw/openclaw/issues/86819)）。用户需要可预测、可解释的 token/成本核算。

## 8. 积压观察

这些事项存在时间较长、重要性高，且仍在等待维护者处理或产品决策：

- **[#45503](https://github.com/openclaw/openclaw/issues/45503)**（2026-03-13，P3）：手动清除工具结果上下文——约 6 个月没有回应；需要产品决策。
- **[#68264](https://github.com/openclaw/openclaw/issues/68264)**（2026-04-17，P2 回归）：Canvas/Browser UI 可视化在聊天中无法渲染——已开放约 5 个月。
- **[#77378](https://github.com/openclaw/openclaw/issues/77378)**（2026-05-04，P2）：会话轮换创建重复会话，并带有损坏的投递上下文（Mattermost）——仍开放；需要更多信息。
- **[#97616](https://github.com/openclaw/openclaw/issues/97616)**（2026-06-29，P1）：僵尸子进程泄漏——尚无修复 PR；等待更多信息。
- **[#118018](https://github.com/openclaw/openclaw/issues/118018)**（2026-08-02，P1）：过期子 Agent 完成结果被投递到已替换的请求方生命周期——关联修复 PR 已开放（[#140137](https://github.com/openclaw/openclaw/pull/140137)）。
- **[#121232](https://github.com/openclaw/openclaw/issues/121232)**（2026-08-09，P1）：memory-core dreaming 的 ranker/applier 结论不一致——尚无新的修复 PR。
- **[#118482](https://github.com/openclaw/openclaw/issues/118482)**（2026-08-03，P2）：codex-supervisor 的 WebSocket 握手在 unix socket 上失败（permessage-deflate 协商）。
- **[#122403](https://github.com/openclaw/openclaw/issues/122403)**（2026-08-12，P2）：UI 模型选择器中的本地/云端来源标识——需要产品决策。
- **[#104521](https://github.com/openclaw/openclaw/issues/104521)**（stale，P2）：飞书/Teams/Mattermost 原生审批按钮——存在被 stale 关闭的风险；但与渠道功能对齐仍相关。

长期未合并、值得维护者关注的 PR： [#117504](https://github.com/openclaw/openclaw/pull/117504)（Bedrock 自定义 embedding 端点，自 8 月 1 日）、[#117605](https://github.com/openclaw/openclaw/pull/117605)（fail-closed 任务取消，自 8 月 1 日）、[#119773](https://github.com/openclaw/openclaw/pull/119773)（媒体理解提取，自 8 月 5 日）、[#127240](https://github.com/openclaw/openclaw/pull/127240)（非 canonical 运行的 CI broker；与安全扫描相关，自 8 月 21 日）、[#130877](https://github.com/openclaw/openclaw/pull/130877)（SQLite 轨迹导出的 OOM 边界，自 8 月 27 日）。

最后，几个高反应数的功能 issue 今天仅因 stale 自动化被关闭（[#79077](https://github.com/openclaw/openclaw/issues/79077)、[#78963](https://github.com/openclaw/openclaw/issues/78963)、[#86986](https://github.com/openclaw/openclaw/issues/86986)、[#86946](https://github.com/openclaw/openclaw/issues/86946)、[#96203](https://github.com/openclaw/openclaw/issues/96203)）。如果它们代表真实路线图需求，维护者应将其转化为明确的产品决策，而不是让它们静默过期。

---

## 横向生态对比

# 跨项目生态系统对比 — 2026-09-07

---

## 1. 生态系统概览

开源个人 AI 助手/智能体（agent）领域正在向一个共享架构收敛：一个长期运行的网关/守护进程持有会话状态、记忆与工具执行；渠道适配器（Discord、Telegram、WhatsApp、电子邮件等）作为控制面；桌面/移动/Web 客户端日益成为无头（headless）运行时之上可选的前端。在今日解析的五个项目——OpenClaw、Hermes Agent、IronClaw、QwenPaw 与 ZeroClaw——中，共涉及约 639 个 PR，其中约 244 个被合并/关闭；约 192 个 issue 有动态，约 96 个被关闭；**没有任何项目发布版本**，表明当前处于稳定期而非功能发布周期。主导性的工程主题不再是新功能亮点，而是*会话生命周期正确性、崩溃安全的回合持久化、守护进程可靠性、供应商/成本控制以及渠道 UX 对齐*——这些是将智能体作为常驻服务运行时的运维关切。另一个次要但明确的信号是：Anthropic 提示缓存调优、上下文窗口回退以及 token/成本核算已成为一等平台问题，而非供应商侧的细节。

---

## 2. 活动对比

数据来自各项目 digest 的汇报；部分计数包含自动化生命周期/CI 活动（如 OpenClaw 的 `clawsweeper` 机器人）。

| 项目 | Issue 更新（关闭） | PR 更新（合并/关闭） | 发布状态 | 健康评分* |
|---|---|---|---|---|
| **OpenClaw** | 144（89 关闭 / 55 开放） | 500（225 合并/关闭） | 今日无；稳定版 **2026.9.2**，**2026.9.3** 处于发布验证中 | **4.0 / 5** |
| **Hermes Agent** | 9（0 关闭） | 50（5 合并/关闭） | 无 | **4.0 / 5** |
| **IronClaw** | 0（0 关闭） | 13（3 合并/关闭，均为依赖/CI） | 无 | **3.5 / 5** |
| **QwenPaw** | 25（约 7 关闭） | 26（6 合并/关闭） | 无；v2.2.0-beta.7 发布任务项已关闭 | **3.0 / 5** |
| **ZeroClaw** | 14（0 关闭） | 50（5 合并/关闭） | 无 | **2.5 / 5** |

*\*健康评分为分析师的综合评分（1–5），考量 24 小时吞吐量、issue 关闭率、最高严重级 bug 的修复 PR 可用性以及开放严重级负载。*

- **OpenClaw** —— 拥有出色的分类吞吐量（24 小时内关闭 225 个 PR 与 89 个 issue），但仍有一个开放的 P0（Discord 设置接受将 application ID 当作 bot token）以及多个无修复 PR 的开放 P1；大量陈旧自动化关闭正在推迟产品决策。
- **Hermes Agent** —— issue 量少，且当日即可完成 issue→修复 PR 配对；因一个未解决的 170 条评论的技能索引基础设施 issue 以及需要整合的重叠开放 PR（#76774/#77359）而扣分。
- **IronClaw** —— 健康状况良好：零 issue 流入，无崩溃级报告，依赖卫生保持一致；仅有日常维护合并，因此势头较低。
- **QwenPaw** —— 活跃的 beta 稳定化阶段，首次贡献者参与健康（贡献者既提交了 bug 也提交了修复 PR），但多项高严重级 issue（#7579、#7589、#7363、#7576）缺乏修复，威胁 v2.2.0 GA。
- **ZeroClaw** —— 贡献者与维护者协作循环最健康，但 24 小时内 issue 关闭为零，存在一个开放的 **S0 数据丢失 bug**（#10121），且 PR 队列增长速度超过合并吞吐量。

---

## 3. OpenClaw 的定位

**对同行的优势**

- **数量级规模的领先优势。** OpenClaw 单日涉及 500 个 PR、合并/关闭 225 个——PR 涉及量约为最接近同行的 10 倍，合并量为 37–75 倍。它在 24 小时内关闭的 issue（89 个）超过了其他四个项目*涉及*的 issue 总和（48 个）。
- **最成熟的发布流程。** 它是唯一运行命名稳定版发布列车（2026.9.2）并带有候选版（2026.9.3）通过发布验证 CI 的项目——同行均处于预发布或发布静默期。
- **最广的平台覆盖面。** OpenClaw 是唯一同时覆盖 Discord、Telegram、WhatsApp、iMessage/Signal、飞书/Teams/Mattermost、Canvas/浏览器 UI *以及*专用 Android/iOS/macOS 客户端的项目。其托管运行时方案（llama.cpp bump #140278、Codex 接入、本地 MLX 支持）使其成为该队列中最具"平台形态"的项目。
- **流程成熟度。** 自动化生命周期工具（`clawsweeper`、陈旧标签）以及针对可复现 bug 的快速分类管线，其规模是任何同行都尚未达到的。

**技术方案差异**

- OpenClaw 将智能体活动建模为**带根会话固定（root-session pinning）的会话树**，并具备显式的子智能体分派语义——yield-batch 唤醒（#140137）、回复权限回退（#140158）以及 Code Mode 中仅限顺序工具执行（#140767）。同行仍在向更简单的"一会话 = 一会话"模型收敛。
- 它运行一个独立的**记忆子系统**（memory-core，具备 dream cycle、ranker/applier 行为）和一个**插件层**（Team Reports，#139850）——其他 digest 未展示出同等深度的组件。
- 其渠道审批逻辑正在*跨渠道*归一化（#140645 WhatsApp/iMessage/Signal），表明其已构建了同行尚未建立的抽象层。

**社区规模对比**

- OpenClaw 的贡献者/流程流量明显最大。然而，每线程的终端用户参与度并未同比例更高——Hermes 的群聊 issue（#97681）吸引了 25 条评论，而 OpenClaw 讨论度最高的条目仅 14 条。OpenClaw 的主导地位很可能反映了*更大的贡献者基础加上更重的自动化*，而非同比例更大的用户社区。
- 其最高反应数的 issue 仅获得 8 个 👍（#79077，Telegram guest bot），表明相对于 PR 体量，终端用户深度参与仍有空间。

**风险提示**

- 陈旧自动化关闭了多个高需求功能请求（Telegram bot-to-bot #79077、WhatsApp 仅监听 #78963），且无产品决策——在其规模下构成社区信任风险。
- OpenClaw 尽管有吞吐量优势，仍与队列共享核心稳定性缺口（僵尸子进程 #97616、Windows 睡眠/恢复 #140010、静默 Discord 错误配置 P0 #140497）。

---

## 4. 共同技术关注领域

多个项目正在独立收敛到同一组问题上——这强有力地证明了生态系统的真实需求所在。

### 4.1 持久、有序、无竞争的会话执行（全部五个项目；在 ZeroClaw、QwenPaw、OpenClaw 中最为突出）
- **ZeroClaw:** 部分 Code/ACP 回合在进程退出时消失（**S0， #10121**）；失败的回合在会话切换后消失（#9333）；超出预算的回合丢弃已流式传输的进度（#10659）；第二条消息可启动重复的并行运行（#10408）。进行中的修复：checkpoint/恢复 PR #10197、transcript 持久化 #9378。
- **QwenPaw:** 模型回复在持久化后从上下文中消失（#7579）；重复消息堆积以及心跳 cron 中约 2 小时无响应（#7589）。
- **OpenClaw:** 陈旧的子智能体完成可被结算到已替换的请求方生命周期中（#118018）；已退役操作上的回复权限回退（#139847）。
- **Hermes Agent:** ACP 客户端铸造永久空探测会话（#104724）；常驻会话租约必须从已死亡的 Desktop lane 回收（#104737）。

**共同需求:** 崩溃安全、类 WAL 的回合转录；精确一次（exactly-once）分派；服务端强制每会话单智能体运行。

### 4.2 常驻、守护进程优先的可靠性（OpenClaw、ZeroClaw、QwenPaw、Hermes）
- **OpenClaw:** 僵尸子进程累积（#97616）；Windows 睡眠/恢复重连停滞（#140010）。
- **ZeroClaw:** 守护进程启动/重载栈溢出（#10230）；重载时 RPC 连接未关闭（#10262）。
- **QwenPaw:** 同步调用冻结 Windows 事件循环 118–135 秒（#7363）；LAN LLM 服务器反复断开（#7505）。
- **Hermes:** 用户明确希望 Desktop 关闭后群聊继续运行（#97681），并提议网关侧回合驱动（#95163）。

**共同需求:** 将客户端 UI 与权威后端会话分离；将重连/恢复视为核心 SLA，而非边缘情况。

### 4.3 供应商成本、缓存与配置完整性（OpenClaw、ZeroClaw、QwenPaw）
- **Anthropic 缓存经济学:** OpenClaw 报告每次回合都有约 46k 前缀被卡住、377k–386k 次 `cache_create` 重写（#140129）；ZeroClaw 正在添加第三个缓存断点（#10660/#10666），修复低于最小值的 OAuth 标记（#10662），并请求可配置的缓存 TTL（#10663）。
- **上下文窗口正确性:** QwenPaw 发布了一个硬编码的 32768 token 回退，影响*所有* v2.1–v2.2 版本（#7576）；DeepSeek 压缩因 `role=user` 而损坏（#6541）。
- **供应商状态漂移:** OpenClaw xAI OAuth 覆盖 Grok 目录（#140482，已关闭）；ZeroClaw 希望在用量事件上获得实时供应商身份（PR #8966，仍开放）。

**共同需求:** 供应商抽象必须显式处理目录状态、缓存标记、TTL 与上下文大小——绝不可静默处理。

### 4.4 聊天渠道作为智能体控制面（OpenClaw、QwenPaw、ZeroClaw、Hermes、IronClaw）
- **审批与管理信任:** OpenClaw 正在跨 WhatsApp/iMessage/Signal 归一化审批反应结算（#140645），并添加受信任的 Discord 管理员（#140798）；用户仍希望飞书/Teams/Mattermost 获得原生审批按钮（#104521）。ZeroClaw 有一个被阻塞的受监督 shell 审批路由 PR（#10241）。
- **IM 中的输出质量:** QwenPaw 正在修复 Telegram 上的原始 Markdown 表格（#7590）、添加中间消息清理（#7592）并折叠飞书长"思考"卡片（#7570）。
- **渠道实例寻址:** ZeroClaw cron 投递（#9940）与心跳目标（#10670）错误处理 `<type>.<alias>` 组合键；IronClaw 正在区分"已配对但断开"与"未配对"的助手渠道（#8076）。

**共同需求:** 一个渠道能力矩阵（审批、管理员角色、实例寻址、消息编辑/清理），在各适配器间保持一致维护。

### 4.5 工具输出/上下文卫生（IronClaw、OpenClaw、QwenPaw、ZeroClaw、Hermes）
- IronClaw 正在将临时命令结果卡片提升为一等 UI：可关闭（#8069）、稳定尺寸（#8071）、菜单导航（#8068）。
- OpenClaw 用户仍希望手动清除大型工具结果（#45503，自三月起已陈旧）。ZeroClaw 报告工具结果截断在模型上下文之外不可见（#10115）。
- QwenPaw 贡献者正在添加工具调用可见性开关（#7357）和流式滚动锁定（#7356）。
- Hermes 有一个 composer z-index bug 隐藏了待办事项（#104723）。

**共同需求:** 工具输出的生命周期管理——关闭、缩小、截断、摘要，并让截断对用户可见。

---

## 5. 差异化分析

| 项目 | 功能重点 | 目标用户 | 技术架构 |
|---|---|---|---|
| **OpenClaw** | 全栈个人 AI 网关；最广 IM/客户端覆盖；记忆子系统；插件；多云提供商路由（含本地运行时） | 在聊天、桌面与移动端运行助手的自托管者与组织 | 网关背后的多运行时编排（托管 llama.cpp、Codex、MLX）；会话树模型；带回复权限的子智能体生命周期；每渠道审批抽象 |
| **Hermes Agent** | 桌面为中心的智能体工作区：bot pinboard、群聊、电子邮件会话、cron/看板工作流、ACP/IDE 集成 | 使用 Nous/Hermes 模型、通过桌面配合 IDE/Code relay 扩展运行智能体工作流的用户 | 桌面客户端 + 网关拆分，群聊状态迁移至网关侧（建议中）；强大的 ACP/OpenCode relay；会话可见性接缝仍在统一中 |
| **IronClaw** | 精简、高性能智能体网关；WebUI 命令/斜杠命令打磨；MCP 诊断；渠道状态清晰 | 想要原生、轻依赖智能体运行时的技术型自托管者 | Rust 原生（tokio、wasmtime/WASM 组件面、WebUI）；保守、维护者驱动的变更节奏 |
| **QwenPaw** | AgentScope 生态中的多提供商智能体枢纽；控制台/插件/技能工作流；v2.2 beta 稳定化 | 基于 Qwen/AgentScope 开发并混用多家提供商（DeepSeek、LM Studio LAN、OpenAI 兼容）的开发者 | Python/AgentScope 运行时（RuntimeWebUI 集成）；多种模式（如 Advisor Mode 的 advisor/worker 配对循环）；插件与技能商店；对首次贡献者非常开放 |
| **ZeroClaw** | 可靠性优先的编码/ACP 智能体：崩溃安全的回合持久化、守护进程/RPC 稳定性、预算/cron 执行、受监督的 shell 审批 | 运行长生命周期、多会话助手的重度 IDE/编码智能体用户，配合 ZeroCode 快速上手 | 守护进程/RPC 架构，带重载语义；ACP/Code 回合检查点；严重性分类（S0–S3）；强大的可信贡献者维护模式 |

实际分野为：**OpenClaw = 广泛平台**，Hermes = 精致的桌面/工作流客户端，IronClaw = 精简原生核心，QwenPaw = 模型生态/开发者工具打法，ZeroClaw = 可靠性与持久性专家。

---

## 6. 社区势头与成熟度

**第一梯队 — 工业级规模迭代：OpenClaw**
每日合并 225 个 PR、关闭 89 个 issue，为命名稳定版/候选版组合运行发布验证 CI，并执行自动化 backlog 卫生。它是唯一瓶颈在于*产品决策吞吐量*而非工程能力的项目。

**第二梯队 — 积极稳定化并并行推进功能工作：QwenPaw 与 ZeroClaw**
- **QwenPaw** 是典型的 pre-GA 形态：高 issue 流入，首次贡献者提交 bug 并修复 bug，有意义的重构仍在落地（记忆生命周期 #7561、主题 token #7487），且今日关闭了一个 beta 发布任务项。v2.2.0 显然临近但尚不可发布。
- **ZeroClaw** 贡献丰富（涉及 50 个 PR），但受限于分类能力：24 小时内 0 个 issue 关闭、仅 5 个 PR 合并，且有一个开放的 S0。维护者明显在投资可信贡献者（引导 #9739、批准 #10623），但队列增长速度超过合并速率。

**第三梯队 — 响应迅速但体量较低：Hermes Agent**
中等活跃度（50 个 PR、5 个合并、9 个 issue），队列中拥有最佳的 issue→修复响应时间（ACP 污染与 vision-relay 422 均当日提交修复 PR）。仍在解决架构方向问题（Desktop 本地 vs. 网关侧群组会话）与 PR 重叠（#76774/#77359）。

**第四梯队 — 稳定维护：IronClaw**
零 issue 流入、三个依赖合并、一批等待审查的精细 WebUI/MCP 打磨 PR。队列中最健康的*代码库*信号，但社区势头最弱。其 wasm 依赖 PR（#7834）自 8 月 23 日以来一直搁置，需要维护者做出决定。

---

## 7. 趋势信号

**1. 崩溃安全的智能体回合正成为入场门槛。**
证据：ZeroClaw S0 #10121（回中进程退出丢失工作）、QwenPaw #7579（回复在持久化后消失）、OpenClaw #118018（陈旧子智能体完成）、Hermes #104724（空 ACP 会话）。*对开发者的价值：* 在继续执行前持久化每个助手/工具片段；将会话设计为追加式日志并支持幂等恢复，而非内存中的聊天缓冲区。

**2. 桌面客户端正成为视图，而非运行时。**
证据：Hermes #97681/#95163（bot 必须比 Desktop 活得更久）、OpenClaw #140010（睡眠/恢复不能杀死可达性）、QwenPaw #7363（UI 事件循环阻塞拖垮整个智能体）、ZeroClaw #10230（守护进程重载崩溃）。*价值：* 以无头优先的方式构建——权威状态放在服务端，客户端作为可重连的表面，并显式测试重载/唤醒/恢复路径。

**3. 提供商成本工程现已成为核心平台功能。**
证据：OpenClaw（#140129）与 ZeroClaw（#10660–#10663）中的 Anthropic 缓存断点/TTL 工作；OpenAI-completions 传输上的缓存标记（#140781）；QwenPaw 中硬编码上下文窗口回归（#7576）；OpenClaw 中缺失的用量/成本页面（#87110）。*价值：* 暴露缓存命中率与每会话成本；使 `cache_control`、TTL 与上下文窗口大小可配置且动态推导——绝不硬编码。

**4. 聊天平台正成为智能体的运维控制台。**
证据：OpenClaw 中的审批/管理员控制（#140645、#140798、#104521），Telegram guest-bot/bot-to-bot 需求（#79077），QwenPaw Telegram/飞书输出清理（#7590/#7592/#7570），ZeroClaw 心跳/cron 渠道实例 bug（#9940/#10670）。*价值：* 将渠道建模为能力矩阵（审批、管理员、编辑、删除、实例寻址）；为 bot-to-bot 通信设计，而非仅限人机对话。

**5. 静默失败是最具腐蚀性的故障模式。**
证据：OpenClaw P0 Discord 设置在挂起时报告"已配置/已停止"（#140497）；ZeroClaw 未经认证的 `/health` 泄漏组件错误（#10606）；QwenPaw 记忆"dreaming"报告"Promoted 0 forever"且不表面化分歧（OpenClaw #121232 是同一模式）；QwenPaw FTS 损坏静默破坏保留策略（#7596）。*价值：* 在设置时通过调用真实 API 来验证配置；以人类可读的错误大声失败；清理但绝不隐藏健康端点中的组件状态。

**6. 多模型编排正将规划者、执行者与编码者角色分离。**
证据：QwenPaw Advisor Mode（#7569）配对 advisor/worker 模型；Hermes 为 `/plan` 添加专用模型路由（#104735）；OpenClaw 在 Code Mode 中强制仅限顺序工具（#140767）并分派嵌套子智能体（#140137）；ZeroClaw 路由受监督的 shell 工作（#10241）。*价值：* 按*任务角色*架构模型路由，为每个角色配置提供商、预算与工具限制——单一的"最佳模型"设置在真实生产环境中无法存活。

**7. 工具输出需要显式的生命周期。**
证据：IronClaw 可关闭/稳定尺寸的命令结果卡片（#8069/#8071）、OpenClaw 陈旧的手动上下文清理请求（#45503）、ZeroClaw 不可见的截断（#10115）、QwenPaw 工具调用可见性开关（#7357）。*价值：* 为用户提供对工具结果的关闭、折叠、截断与"清除上下文"控制，且截断始终在 UI 中表面化——否则长期运行的智能体会被自己的输出淹没。

---

**给决策者的底线：** 生态系统的重心已从模型能力转向*运维可靠性*——持久会话、无头操作、成本可见性与渠道级 UX。OpenClaw 在广度和吞吐量上领先，但同样背负着队列中开放的可可靠性债务；Hermes 与 QwenPaw 提供每 issue 最响应的社区；IronClaw 是低风险、低势头的选项；ZeroClaw 是最值得关注的持久化模式，这些模式很可能成为行业默认。

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目摘要 — 2026-09-07

## 今日概览
Hermes Agent 在过去 24 小时内十分活跃：9 个 issue 被更新（均仍处于开启状态），50 个 PR 被更新，其中 45 个仍开放，5 个已合并或关闭。没有发布任何新版本，因此没有需要向用户同步的版本变更。最活跃的领域包括会话生命周期与状态持久化、ACP/邮件/网关处理、Desktop UI 打磨、特定提供商的模型路由，以及 cron/看板修复。本窗口内一个值得注意的现象是：bug 报告与修复 PR 在当天迅速配对，尤其是 ACP 会话污染和 Console Go 视觉中继失败。

## 版本发布
过去 24 小时内没有发布任何新版本。

## 项目进展
本窗口内有 5 个 PR 转为已合并/已关闭状态。可见的已关闭项包括：

- [#104715 — `feat(cron): support atomic disabled job creation`](https://github.com/NousResearch/hermes-agent/pull/104715)  
  新增 `hermes cron create ... --disabled`，直接持久化一条惰性的暂停记录，而不是先创建一个启用的任务、之后再将其暂停。
- [#104730 — `feat(desktop): add bot pinboard above sessions`](https://github.com/NousResearch/hermes-agent/pull/104730)  
  在 Desktop 会话区域上方新增了一个紧凑的 bot 固定栏，复用现有的 bots 名册与路由，未改动完整的 Bots 管理窗格。

其他仍在推进功能或修复但尚未合并的显著 PR：

- [#104739 — `fix(acp): never persist empty probe sessions; prune can sweep legacy ACP shells`](https://github.com/NousResearch/hermes-agent/pull/104739)  
  针对 ACP 空会话污染问题。
- [#104735 — `feat: add a dedicated model route for /plan`](https://github.com/NousResearch/hermes-agent/pull/104735)  
  为单轮 `/plan` 添加可选的 `planning` 提供商/模型配置。
- [#104148 — `fix(email): isolate sessions by normalized subject when opted in`](https://github.com/NousResearch/hermes-agent/pull/104148)  
  实现基于邮件主题的会话隔离。
- [#104734](https://github.com/NousResearch/hermes-agent/pull/104734) 和 [#104736](https://github.com/NousResearch/hermes-agent/pull/104736)  
  两个针对 Console Go/OpenCode 中继 422 视觉错误的候选修复。

## 社区热门话题
按评论量排序，讨论最多的 issue 如下：

- [#66616 — `[skills-index-watchdog] Skills index is stale or degraded`](https://github.com/NousResearch/hermes-agent/issues/66616) — **170 条评论**  
  这一自动化新鲜度失败问题引发了持续讨论。根本问题出在运维层面：Skills Hub 索引按 cron 计划重建，而当前索引已存在 29.8 小时，超过了 26 小时的限制。高评论数更说明这是持续嘈杂的自动化 watchdog 活动，而非一个有争议的设计问题。
- [#97681 — `Bot Group Chats should keep working after Desktop closes`](https://github.com/NousResearch/hermes-agent/issues/97681) — **25 条评论**  
  用户希望群聊驻留在后端，使 bot 能够在笔记本电脑、家庭服务器或 VPS 上继续运行，并能从另一台设备恢复对话。
- [#95163 — `Opt-in backend-hosted group rooms — gateway-side round driver + authoritative room log`](https://github.com/NousResearch/hermes-agent/issues/95163) — **14 条评论，1 👍**  
  这是一项技术提案，主张将群组房间的编排从 Desktop 渲染器移到网关中。与 #97681 合在一起看，这释放出一个围绕持久化、多设备 bot 协作的清晰路线图信号。
- [#26277 — `Feature request: optional email session isolation by normalized subject`](https://github.com/NousResearch/hermes-agent/issues/26277) — **10 条评论，2 👍**  
  用户希望不同的邮件主题使用不同的 Hermes 会话，而不是每个发件人只有一个连续会话。目前已有对应的实现 PR。

本次快照未提供 PR 评论数量，但最活跃的 PR 线程很可能集中在会话可见性、网关投递和 ACP 状态修复相关的话题上。

## Bug 与稳定性
过去 24 小时内没有关闭任何 issue。以下为已报告且仍在持续的 bug，按预期用户影响排序：

1. [#104731 — `HTTP 422 from Console Go relay: vision_analyze embeds image parts in tool messages upstream rejects`](https://github.com/NousResearch/hermes-agent/issues/104731)  
   通过 OpenCode Go 中继的视觉调用会以不可重试的 422 中断当前轮次。已有两个同日提交的修复 PR：[#104734](https://github.com/NousResearch/hermes-agent/pull/104734) 和 [#104736](https://github.com/NousResearch/hermes-agent/pull/104736)。

2. [#104724 — `ACP mints a permanent empty session row for every session/new that never receives a prompt`](https://github.com/NousResearch/hermes-agent/issues/104724)  
   会话发现探测会让 `state.db` 里充满 `message_count=0` 的行，这些行看起来就像真实对话。修复 PR：[#104739](https://github.com/NousResearch/hermes-agent/pull/104739)。

3. [#66616 — `Skills index is stale or degraded`](https://github.com/NousResearch/hermes-agent/issues/66616)  
   持续的文档基础设施不稳定问题：当前对外提供的 Skills Hub 索引过于陈旧。本窗口内没有直接的修复 PR。

PR 队列中还有一些针对上述可见 issue 集合之外问题的稳定性修复：

- [#104738](https://github.com/NousResearch/hermes-agent/pull/104738) — 在 OAuth 恢复时清除账单 `failure_reason`，并防止产生永不过期的账单闩锁。
- [#104737](https://github.com/NousResearch/hermes-agent/pull/104737) — 从已失效的 Desktop 通道回收驻留会话租约。
- [#104723](https://github.com/NousResearch/hermes-agent/pull/104723) — 修复 Desktop Composer 中 z-index 堆叠溢出导致溢出待办事项被隐藏的问题。
- [#104721](https://github.com/NousResearch/hermes-agent/pull/104721) — 修复看板停止提醒未与发起方 worker 运行关联的问题。

## 功能请求与路线图信号
最强的路线图信号是**持久化 bot 群聊**。把 [#97681](https://github.com/NousResearch/hermes-agent/issues/97681) 与 [#95163](https://github.com/NousResearch/hermes-agent/issues/95163) 结合起来看，方向就是把群组房间状态和轮次编排从 Desktop 本地存储迁往网关侧的权威会话。考虑到已有的会话状态相关工作流，这很可能是近期的一个功能目标。

其他功能信号：

- [#26277 — Email session isolation by normalized subject](https://github.com/NousResearch/hermes-agent/issues/26277) 已有开放的实现 PR，可能很快落地。
- [#104735 — Dedicated model route for /plan](https://github.com/NousResearch/hermes-agent/pull/104735) 规模小、可选，并且由配置驱动，因此是很有可能的近期新增功能。
- [#104729 — Desktop slash autocomplete full-description hover](https://github.com/NousResearch/hermes-agent/issues/104729) 属于低风险的 UX 增强。
- [#104720 — Dashboard light/cream theme](https://github.com/NousResearch/hermes-agent/issues/104720) 是对 UI 可访问性反馈的回应。
- [#100655 — Opt-in pre-lifecycle boundary for external applications](https://github.com/NousResearch/hermes-agent/issues/100655) 明确在征求维护者的方向性意见，而非寻求实现批准。

## 用户反馈摘要
本窗口内用户的痛点主要集中在**状态清洁**、**跨设备连续性**和**UI 打磨**上：

- Dashboard 默认主题引发了异常强烈的负面反馈：一位用户称默认主题“简直糟透了”、“黑得吓人且只有单色”、“看得人眼睛疼”（[#104720](https://github.com/NousResearch/hermes-agent/issues/104720)）。
- Desktop 用户发现斜杠命令的描述被截断，无法阅读完整内容（[#104729](https://github.com/NousResearch/hermes-agent/issues/104729)）。
- ACP 客户端和编辑器的模型发现探测正在制造令人困惑的空会话，看起来像真实聊天记录（[#104724](https://github.com/NousResearch/hermes-agent/issues/104724)）。
- 用户希望群聊 bot 能独立于正在运行的 Desktop 客户端继续工作，并使用自己的模型、工具和凭据（[#97681](https://github.com/NousResearch/hermes-agent/issues/97681)）。
- 邮件用户希望与同一发件人的多主题对话不再合并进同一个会话（[#26277](https://github.com/NousResearch/hermes-agent/issues/26277)）。

## 积压事项观察
有几项值得关注的事项似乎需要维护者审查或做出决定：

- [#66616 — Skills index stale/degraded](https://github.com/NousResearch/hermes-agent/issues/66616) 自 7 月起一直处于开放状态，已累积 170 条评论，但仍未解决。
- [#26277 — Email session isolation by subject](https://github.com/NousResearch/hermes-agent/issues/26277) 自 5 月起开放；目前已有一个实现 PR（[#104148](https://github.com/NousResearch/hermes-agent/pull/104148)），需要评估。
- [#53544 — Recover undelivered tool-call content](https://github.com/NousResearch/hermes-agent/pull/53544) 是自 6 月底起开放的 P2 级 core-agent 投递 bug 修复 PR。
- [#76774 — Unify session-source visibility across all resume surfaces](https://github.com/NousResearch/hermes-agent/pull/76774) 与 [#77359 — Unify cross-source session visibility for `/sessions` and `/resume`](https://github.com/NousResearch/hermes-agent/pull/77359) 是两个相互重叠的开放 PR，很可能应当合并。
- [#85744 — Show persisted usage without live agent](https://github.com/NousResearch/hermes-agent/pull/85744) 自 8 月起一直开放，需要做出决定。
- [#100655 — Pre-lifecycle boundary for external applications](https://github.com/NousResearch/hermes-agent/issues/100655) 明确在等待维护者输入，而非实现。

---

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# Ironclaw 项目摘要 — 2026-09-07

## 今日概览

截至 2026-09-07 的 24 小时内，Ironclaw 保持着稳定的维护者驱动开发节奏：0 个 Issue 更新，13 个 PR 更新（10 个开放，3 个关闭/合并）。未发布任何新版本。活跃工作集中在 WebUI 命令结果/斜杠命令打磨（[#8071](https://github.com/nearai/ironclaw/pull/8071)、[#8070](https://github.com/nearai/ironclaw/pull/8070)、[#8069](https://github.com/nearai/ironclaw/pull/8069)、[#8068](https://github.com/nearai/ironclaw/pull/8068)）、助手频道状态消息（[#8076](https://github.com/nearai/ironclaw/pull/8076)）、MCP 诊断（[#8077](https://github.com/nearai/ironclaw/pull/8077)）以及自动化依赖升级。三个关闭/合并的 PR 均为依赖/CI 维护更新，因此该窗口期内没有面向用户的功能落地。整体来看项目健康状况稳定：新 Issue 数量少，核心贡献者的打磨完善工作仍在继续。

## 版本发布

该时段内未发布任何新版本。

## 项目进展

该窗口期内有三个 PR 关闭/合并，均为依赖/CI 更新：

- [PR #8049](https://github.com/nearai/ironclaw/pull/8049) — `chore(deps)`：将 “everything-else” Rust 依赖组升级 19 项。
- [PR #7835](https://github.com/nearai/ironclaw/pull/7835) — `chore(deps)`：将 GitHub Actions 依赖组升级 5 项。
- [PR #7020](https://github.com/nearai/ironclaw/pull/7020) — `chore(deps)`：在 tokio-ecosystem 依赖组中将 `tokio-tungstenite` 从 0.29.0 升级到 0.30.0。

这些合并以维护为主，而非功能或缺陷修复。更偏功能性的工作仍在进行中：来自核心贡献者的四个 WebUI 斜杠命令/命令结果修复（#8071、#8070、#8069、#8068），外加助手共享频道处理（#8076）与 MCP 响应泄漏诊断（#8077）。

## 社区热点

该窗口期内没有 Issue 更新，数据集中的 PR 也未见有实质意义的评论/回应活动，因此没有真正值得关注的社区讨论串。最活跃的工作线如下：

- **WebUI 斜杠命令与命令结果打磨** — 来自 `italic-jinxin` 的四个相关 PR：保持命令结果卡片高度（[#8071](https://github.com/nearai/ironclaw/pull/8071)）、对齐斜杠命令元数据（[#8070](https://github.com/nearai/ironclaw/pull/8070)）、为命令结果卡片增加关闭操作（[#8069](https://github.com/nearai/ironclaw/pull/8069)）以及保持当前激活的斜杠命令可见（[#8068](https://github.com/nearai/ironclaw/pull/8068)）。
- **助手频道状态更明确** — [PR #8076](https://github.com/nearai/ironclaw/pull/8076) 区分了“已配对用户的共享频道已断开”与“从未配对的账号”这两种情形。
- **依赖自动化压力** — 大量 Dependabot 批量更新仍处于开放状态（[#8080](https://github.com/nearai/ironclaw/pull/8080)、[#8078](https://github.com/nearai/ironclaw/pull/8078)、[#8079](https://github.com/nearai/ironclaw/pull/8079)、[#7834](https://github.com/nearai/ironclaw/pull/7834)）。

潜在信号表明，项目正处于打磨完善模式：改善 WebUI 对临时命令结果的可用性、让助手错误状态更易于理解，并持续保持依赖的新鲜度。

## 缺陷与稳定性

该窗口期内未记录到崩溃级缺陷或新的 Issue 报告。若干开放的修复 PR 针对现有缺陷或体验粗糙之处，按可能的严重程度排序如下：

1. **MCP 响应泄漏诊断误分类** — [PR #8077](https://github.com/nearai/ironclaw/pull/8077) 将关闭 issue #8009；它修复了 MCP 出站诊断，使泄漏阻断保持安全的同时，保留一个 MCP 可见的独立原因。这是这批修复中最贴近安全性的一个。
2. **共享频道断开状态引发的混淆** — [PR #8076](https://github.com/nearai/ironclaw/pull/8076) 修复了“已配对用户的共享频道断开连接”与“从未配对”两种情况下用户体验有误的问题。
3. **命令结果卡片高度塌陷** — [PR #8071](https://github.com/nearai/ironclaw/pull/8071) 防止结构化命令结果卡片在聊天记录视口内缩小/折叠。
4. **命令结果卡片缺少关闭操作** — [PR #8069](https://github.com/nearai/ironclaw/pull/8069) 为临时命令结果增加易用的关闭操作，同时保留持久消息。
5. **当前斜杠命令项可能被隐藏** — [PR #8068](https://github.com/nearai/ironclaw/pull/8068) 修复命令菜单中键盘与指针导航时的可见性问题。
6. **斜杠命令元数据错位** — [PR #8070](https://github.com/nearai/ironclaw/pull/8070) 修复可变宽度布局问题；主要是外观层面。

## 功能请求与路线图信号

该窗口期内 Issue 跟踪器未出现用户新提交的功能请求。不过，开放的 PR 仍传递出一些路线图信号：

- **临时命令结果生命周期** 正在成为 UI 层的一等公民行为：可关闭（[#8069](https://github.com/nearai/ironclaw/pull/8069)）、稳定尺寸（[#8071](https://github.com/nearai/ironclaw/pull/8071)）以及更好的菜单导航（[#8068](https://github.com/nearai/ironclaw/pull/8068)）。
- **助手频道诊断** 正在改进，让用户获得针对具体频道的指引（[#8076](https://github.com/nearai/ironclaw/pull/8076)）。
- **MCP 诊断** 正在产品、适配器及 OpenAI 兼容接口等层面走向标准化（[#8077](https://github.com/nearai/ironclaw/pull/8077)）。

如果这些 PR 均能顺利合并，下一个版本很可能会包含有实际意义的 WebUI 命令菜单/结果改进，以及更准确的助手/MCP 错误提示。

## 用户反馈摘要

该窗口期内的 Issue/评论数据不足以直接衡量满意度。间接来看，开放的修复反映了可能的用户痛点：过长或不断累积的命令结果难以查看、临时命令卡片无法关闭、斜杠命令选项滚出屏幕时行为令人困惑、共享频道或 MCP 拒绝消息含义不清。在这 24 小时内，GitHub Issues 中未出现任何明确的负面反馈。

## 积压观察

- **[PR #7834](https://github.com/nearai/ironclaw/pull/7834)** 仍是最值得注意的长期未合并 PR：这是 Dependabot 对 `wasm` 依赖组（wasmtime、wasmtime-wasi、wit-component、wit-parser）的批量升级，创建于 2026-08-23，风险标记为“中”，在 2026-09-06 更新后仍处于开放状态。它需要维护者做出决定：合并、更新还是关闭。
- 数据集中没有长期未答复的 Issue。较早的依赖类 PR 积压正在被清理，这可以从 [#7020](https://github.com/nearai/ironclaw/pull/7020)、[#7835](https://github.com/nearai/ironclaw/pull/7835) 和 [#8049](https://github.com/nearai/ironclaw/pull/8049) 的关闭看出。

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

## QwenPaw 项目简报 — 2026-09-07

### 1. 今日概览

QwenPaw 在 2026-09-07 呈现出活跃度高但发布面平静的态势：过去 24 小时内有 25 个 issue 被更新（18 个仍打开），26 个 PR 被更新（20 个仍打开）。该时间窗口内没有发布新版本；最近的公开信号仍是 v2.2.0-beta.7 的发布职责事项。更新流以 v2.2.x 缺陷报告和首次贡献者修复为主，反复出现的主题包括任务队列行为、上下文持久化、流式/渠道渲染以及工具调用错误可见性。总体来看，项目正处于积极的稳定化阶段，同时仍有可观的特性开发在并行推进。

---

### 2. 发布

**本周期内无发布。**

过去 24 小时内未发布任何新的版本产物。当前追踪到的最新事项仍是早前 v2.2.0-beta.7 的安装验证职责 issue，该 issue 已于 2026-09-07 更新/关闭：

- [QwenPaw v2.2.0-beta.7 Release Duty #7503](https://github.com/agentscope-ai/QwenPaw/issues/7503)

---

### 3. 项目进展

过去 24 小时内有 26 个 PR 被更新，其中 6 个显示为已合入/已关闭。本次样本中可见的已关闭 PR：

- [#7595 fix(console): unify language selector options](https://github.com/agentscope-ai/QwenPaw/pull/7595) — 已关闭；统一了顶部下拉框与侧边栏设置中的语言选项。
- [#7086 fix(console): unify language options between settings gear and dropdown](https://github.com/agentscope-ai/QwenPaw/pull/7086) — 也已关闭，很可能已被上述 PR 或等效变更取代。

本周期内有明显进展的开放 PR：

- [#7561 refactor(memory): unify automatic memory lifecycle and actions](https://github.com/agentscope-ai/QwenPaw/pull/7561) — 内存管理器契约发生重大变更。
- [#7487 feat/theme token unification](https://github.com/agentscope-ai/QwenPaw/pull/7487) — 将多个 UI 界面迁移至集中式语义主题令牌。
- [#7502 feat(console): redesign sidebar and settings experience](https://github.com/agentscope-ai/QwenPaw/pull/7502)
- [#7486 feat(creator) 1.1.2](https://github.com/agentscope-ai/QwenPaw/pull/7486) — 大型插件更新：异步委派、A/B 对比、媒体排期、Docker 部署。
- [#7509 feat(skill): Update make-skill to v2](https://github.com/agentscope-ai/QwenPaw/pull/7509) — 审批驱动的先草稿后发布工作流。
- [#7569 feat(modes): add Advisor Mode](https://github.com/agentscope-ai/QwenPaw/pull/7569) — 新增配对式顾问/执行者模型循环模式。
- [#7382 feat(chat): adapt AgentScopeRuntimeWebUI 1.2.0 APIs](https://github.com/agentscope-ai/QwenPaw/pull/7382)

另有几个首次贡献者提交的修复 PR 正在等待审核，对应本周报告的紧急缺陷：

- [#7577 fix(console): enqueue follow-up messages when chat task is running](https://github.com/agentscope-ai/QwenPaw/pull/7577)
- [#7578 fix(tool_calls): log exceptions in coordinator _drain()](https://github.com/agentscope-ai/QwenPaw/pull/7578)
- [#7593 feat(console): restore session direct path input alongside picker](https://github.com/agentscope-ai/QwenPaw/pull/7593)
- [#7592 feat(telegram): optional cleanup of intermediate messages after final answer](https://github.com/agentscope-ai/QwenPaw/pull/7592)
- [#7590 fix(telegram): render Markdown tables as `<pre>` instead of raw pipes](https://github.com/agentscope-ai/QwenPaw/pull/7590)

---

### 4. 社区热点

按评论量计算最活跃的 issue：

- [#7505 qwenpaw accessing LAN LLM server frequently disconnects and times out](https://github.com/agentscope-ai/QwenPaw/issues/7505) — 12 条评论。已关闭。运行本地 LM Studio 服务器的用户会遇到反复的 `client disconnect` 重试并最终超时。这表明面向 OpenAI 兼容的局域网端点需要更好的连接复用/重试处理。

- [#7559 Sending a new message while a task is running triggers 409](https://github.com/agentscope-ai/QwenPaw/issues/7559) — 5 条评论。用户期望后续消息进入队列，而不是被拒绝。关联到开放的修复 PR [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577)。

- [#6820 UI does not show model output / tool calls until everything is finished](https://github.com/agentscope-ai/QwenPaw/issues/6820) — 5 条评论。较早的已关闭 issue，但仍反映了用户对流式可见性的期望。

- [#7587 OpenAI-compatible provider gets Cloudflare 403 with WUSRouter](https://github.com/agentscope-ai/QwenPaw/issues/7587) — 4 条评论。提供商连接与模型列表获取方面存在集成缺口。

- [#7513 DeepSeek conversation mixes model responses with QwenPaw tool calls](https://github.com/agentscope-ai/QwenPaw/issues/7513) — 4 条评论。用户报告其他代理工具不受影响；问题指向工具调用边界的解析/处理缺陷。

- [#7363 Synchronous calls freeze the event loop and timeout never fires](https://github.com/agentscope-ai/QwenPaw/issues/7363) — 4 条评论。Windows 桌面端在启动和发送消息期间存在严重性能问题。

潜在需求：用户越来越依赖 QwenPaw 作为多提供商代理中枢，他们需要更流畅的流式输出、可靠的任务排队，以及更稳健的 HTTP/提供商行为。

---

### 5. 缺陷与稳定性

**严重 / 高严重度**

- [#7579 Model replies unexpectedly disappear from context after persistence](https://github.com/agentscope-ai/QwenPaw/issues/7579) — 打开中。模型“看不到自己之前的回复”，导致空回复和重复工具循环。相关严重报告 [#7584](https://github.com/agentscope-ai/QwenPaw/issues/7584) 被作为重复问题关闭。目前尚无明确的修复 PR。

- [#7589 Heartbeat cron session feedback loop / duplicate message pile-up](https://github.com/agentscope-ai/QwenPaw/issues/7589) — 高严重度；代理约 2 小时无响应。已在最新 `main` 分支验证。未见修复 PR。

- [#7363 Synchronous calls freeze the event loop and timeout never fires](https://github.com/agentscope-ai/QwenPaw/issues/7363) — Windows 桌面端启动时冻结 118–135 秒，发送消息时冻结约 126 秒。

- [#7567 Stop button reports task stopped, but the task keeps running](https://github.com/agentscope-ai/QwenPaw/issues/7567) — 导致后续 409 错误，并使被误发的命令得到错误执行。

- [#7576 RetryChatModel hardcoded 32768 context_size fallback causes CONTEXT_UNFIT](https://github.com/agentscope-ai/QwenPaw/issues/7576) — 影响 v2.1.0 至 v2.2.0 的全部已发布版本；模型上下文窗口未被正确推断。

- [#7596 Scroll history.db FTS corruption undetected by integrity check](https://github.com/agentscope-ai/QwenPaw/issues/7596) — 导致每次启动时保留清理静默失败。

**中等严重度 / 需要关注**

- [#7597 Tool-returned image/PDF binary sent as bare base64 triggers 400](https://github.com/agentscope-ai/QwenPaw/issues/7597) — 新 issue；工具返回二进制内容时会导致对话失败。

- [#7559 409 error when user sends message during active task](https://github.com/agentscope-ai/QwenPaw/issues/7559) — 修复 PR [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577) 已打开。

- [#7572 Tool dispatch layer swallows exception stacks in `_coordinator.py`](https://github.com/agentscope-ai/QwenPaw/issues/7572) — 影响故障诊断；修复 PR [#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578) 已打开。

- [#7587 Cloudflare 403 / “Just a moment” challenge with WUSRouter](https://github.com/agentscope-ai/QwenPaw/issues/7587)

- [#6541 Scroll context compression uses `role=user` on DeepSeek, causing MODEL_EXECUTION_ERROR](https://github.com/agentscope-ai/QwenPaw/issues/6541) — 自 7 月起一直打开。

- [#7585 Telegram Markdown tables appear as raw `|` and `---`](https://github.com/agentscope-ai/QwenPaw/issues/7585) — 修复 PR [#7590](https://github.com/agentscope-ai/QwenPaw/pull/7590) 已打开。

- [#7505 LAN LLM server disconnects/client retries](https://github.com/agentscope-ai/QwenPaw/issues/7505) — 已关闭，但代表了长期存在的本地服务器可靠性关切。

---

### 6. 功能请求与路线图信号

本周值得关注的用户驱动功能信号：

- **恢复工作目录的直接路径输入**
  [#7588 [Feature] Restore v2.1.0 main working-directory switching](https://github.com/agentscope-ai/QwenPaw/issues/7588) — 用户强烈偏好旧的键入加回车工作流，而非当前的目录选择器。开放修复：[#7593](https://github.com/agentscope-ai/QwenPaw/pull/7593)。

- **更干净的渠道输出**
  - Telegram：在最终回答后自动清理中间推理/工具消息——[#7586](https://github.com/agentscope-ai/QwenPaw/issues/7586)，另有 PR [#7592](https://github.com/agentscope-ai/QwenPaw/pull/7592)。
  - 飞书：流式结束后自动折叠长“思考”卡片——[#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570)。用户甚至提供了一种本地实现方案。

- **插件商店可用性**
  [#7582 Plugin store needs one-click updates and update notifications](https://github.com/agentscope-ai/QwenPaw/issues/7582) — 管理多个 QwenPaw 安装实例的用户认为当前商店流程点击过多，且容易丢失上下文。

- **聊天可读性控制**
  社区贡献者提交的开放 PR 已在为下一版本做准备：
  - 工具调用可见性切换：[#7357](https://github.com/agentscope-ai/QwenPaw/pull/7357)
  - 流式期间聊天滚动锁定：[#7356](https://github.com/agentscope-ai/QwenPaw/pull/7356)
  - 富输入框光标可见性修复：[#7347](https://github.com/agentscope-ai/QwenPaw/pull/7347)

- **较早的请求**
  [#4077 UI font scaling & clickable file-path links](https://github.com/agentscope-ai/QwenPaw/issues/4077) — 已关闭，但随着桌面端使用量增长，该需求很可能会再次出现。

下一版本可能的候选：恢复直接路径输入、Telegram 输出清理、Markdown 表格处理、插件商店改进，以及一个或多个控制台/聊天 UX PR。

---

### 7. 用户反馈摘要

本周用户的真实痛点同时涉及技术和工作流两个层面的不满：

- 任务控制尚不可信赖：用户排队新消息时收到 409，停止按钮不被执行，重复的任务输出在异常的时间间隔出现。
  [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)、[#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567)、[#7594](https://github.com/agentscope-ai/QwenPaw/issues/7594)

- 长期指令/记忆遵从是明显的痛点，尤其是在插件开发场景中，QwenPaw 几天后就会忘记期望的工作路径。
  [#7571](https://github.com/agentscope-ai/QwenPaw/issues/7571)

- 高级用户认为 v2.2.0 在 v2.1.0 中有用的桌面/控制台交互上出现回退，尤其是直接输入工作目录路径这一交互。
  [#7588](https://github.com/agentscope-ai/QwenPaw/issues/7588)

- 渠道用户认为 Telegram/飞书上的输出噪声较大且渲染不足，尤其是推理文本、工具调用和 Markdown 表格。
  [#7585](https://github.com/agentscope-ai/QwenPaw/issues/7585)、[#7586](https://github.com/agentscope-ai/QwenPaw/issues/7586)、[#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570)

- 多份报告包含高质量的诊断细节，新贡献者也在主动为自己报告的 issue 提交修复 PR。这是非常积极的项目健康信号。

---

### 8. 积压事项观察

以下 issue/PR 看起来很重要，仍需维护者关注：

- [#6541 Scroll context compression triggers MODEL_EXECUTION_ERROR on DeepSeek](https://github.com/agentscope-ai/QwenPaw/issues/6541) — 自 2026-07-29 起一直打开，未见关联修复 PR。对 DeepSeek API 用户影响较大。

- [#7363 Synchronous calls freeze the event loop and timeout never fires](https://github.com/agentscope-ai/QwenPaw/issues/7363) — 自 2026-08-27 起一直打开；Windows 桌面性能问题，暂无可见修复。

- [#7576 RetryChatModel hardcoded context window fallback](https://github.com/agentscope-ai/QwenPaw/issues/7576) — 据报告存在于所有已发布的 v2.1–v2.2 构建中；尚无修复 PR。

- [#7589 Heartbeat cron duplicate-message feedback loop](https://github.com/agentscope-ai/QwenPaw/issues/7589) — 高严重度、近期报告，尚未见维护者回应或修复 PR。

- [#7596 Scroll `history.db` FTS corruption](https://github.com/agentscope-ai/QwenPaw/issues/7596) — 今日报告的静默数据损坏问题；需要一套持久可靠的修复策略。

以下开放 PR 修复了近期报告的高信号缺陷，值得维护者及时审核：

- [#7577 Enqueue follow-up messages instead of returning HTTP 409](https://github.com/agentscope-ai/QwenPaw/pull/7577)
- [#7578 Log exceptions in tool-coordinator `_drain()`](https://github.com/agentscope-ai/QwenPaw/pull/7578)
- [#7590 Telegram Markdown table fallback rendering](https://github.com/agentscope-ai/QwenPaw/pull/7590)
- [#7592 Telegram intermediate-message cleanup](https://github.com/agentscope-ai/QwenPaw/pull/7592)
- [#7593 Restore direct path input for working-directory switching](https://github.com/agentscope-ai/QwenPaw/pull/7593)

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目摘要 — 2026-09-07

## 1. 今日概览
ZeroClaw 今日活跃度极高：过去 24 小时内有 14 个 issue 被更新（全部仍为开启，无一关闭），50 个 PR 被更新——其中 45 个仍开启，5 个已合并/关闭。没有发布新版本。主要议题集中在 ACP/Code 回合持久性与数据丢失防护（[#9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333)、[#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121)、[#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659)）、守护进程/RPC 在重载时的稳定性（[#10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230)）以及 Anthropic 提示词缓存调优（[#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660)、[#10662](https://github.com/zeroclaw-labs/zeroclaw/issues/10662)、[#10663](https://github.com/zeroclaw-labs/zeroclaw/issues/10663)）。维护团队正在积极引导大型社区 PR，但 issue 全部开启且零关闭的现状表明 triage 吞吐量仍赶不上上报量。整体来看，项目在贡献量上表现稳健，主要风险集中在回合生命周期可靠性以及不断增长的 P1 缺陷队列上。

## 2. 版本发布
过去 24 小时没有发布新的 ZeroClaw 版本。没有变更日志、破坏性变更或迁移说明需要报告。

## 3. 项目进展
- 窗口期内有 5 个 PR 被合并或关闭；这些条目未包含在可见的前 20 条数据中，因此无法提供具体情况。没有 issue 被关闭。
- 有明显推进的功能和修复（均仍为开启状态）：
  - [PR #10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197) — fix(acp)：对中断的 Code/ACP 回合进度进行检查点保存和恢复；直接针对 S0/S1 持久性问题族（[#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121)、[#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659)）。
  - [PR #9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378) — fix(acp)：持久化失败及已取消回合的对话记录；是 #10197 的姊妹 PR，目前标记为 `stale-candidate`/`needs-author-action`。
  - [PR #10654](https://github.com/zeroclaw-labs/zeroclaw/pull/10654) — fix(runtime)：限制 RPC 分发时的栈使用量，并附带结构性回归测试；缓解了 [#10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230) 背后的栈溢出风险。
  - [PR #10262](https://github.com/zeroclaw-labs/zeroclaw/pull/10262) — fix(rpc)：在守护进程重载时关闭 RPC 连接，并解决 ZeroCode quickstart 卡住的问题。
  - [PR #10623](https://github.com/zeroclaw-labs/zeroclaw/pull/10623)（已于 09-05 获批）及其堆叠的 [PR #10666](https://github.com/zeroclaw-labs/zeroclaw/pull/10666) — Anthropic 提示词缓存透传，外加第三个缓存断点（#10666 关闭 [#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660)）。
  - [PR #9739](https://github.com/zeroclaw-labs/zeroclaw/pull/9739) — 带智能体侧边栏的多会话面板；维护者已完成重连与生命周期修复。
- 其他仍在推进中的大型功能：可点击的对话记录 URL（[PR #10386](https://github.com/zeroclaw-labs/zeroclaw/pull/10386)）、会话提示词附件持久化（[PR #10407](https://github.com/zeroclaw-labs/zeroclaw/pull/10407)）以及用量事件中的实时提供商身份（[PR #8966](https://github.com/zeroclaw-labs/zeroclaw/pull/8966)）。

## 4. 社区热门话题
按评论数排序的热门 issue（均无 reaction）：
- [Issue #10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230)（6 条评论）— 应用 Quickstart 配置时，智能体初始化期间守护进程启动/重载发生栈溢出；S1、needs-repro、进行中。
- [Issue #9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333)（4 条评论）— 失败的 ACP 回合在切换会话后消失；S1，进行中，修复候选为 [#9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378)/[#10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197)。
- [Issue #10408](https://github.com/zeroclaw-labs/zeroclaw/issues/10408)（3 条评论）— 回合进行中发送第二条消息会触发并行运行，造成重复工作和重复回复；S2。
- [Issue #10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121)（3 条评论）— 进程在回合中途退出时，部分 Code/ACP 回合会消失；S0 数据丢失/安全风险。
- [Issue #9191](https://github.com/zeroclaw-labs/zeroclaw/issues/9191)（3 条评论）— Cron 智能体任务缺少 wall-clock（墙钟）超时；进行中的锁只在进程启动时被清除；S1。

潜在需求：用户把 ZeroClaw 当作长期运行的多会话助手使用，结果不断遇到生命周期竞态条件——回合进行中发送消息、切换会话、预算耗尽以及进程退出，都可能导致工作丢失或重复执行。共同诉求是：持久、有序、崩溃安全的会话执行。

## 5. 缺陷与稳定性
按严重程度排序；如已有修复 PR 会注明。

- **S0 — 数据丢失/安全风险**
  - [#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121) — 进程在完成前退出时，部分 Code/ACP 回合会丢失。修复审查中：[PR #10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197)。
- **S1 — 工作流受阻**
  - [#10230](https://github.com/zeroclaw-labs/zeroclaw/issues/10230) — 应用 Quickstart 配置时，智能体初始化期间守护进程启动/重载发生栈溢出；相关加固见 [PR #10654](https://github.com/zeroclaw-labs/zeroclaw/pull/10654) 和 [PR #10262](https://github.com/zeroclaw-labs/zeroclaw/pull/10262)。
  - [#9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333) — 切换会话后失败的 ACP 回合消失。修复候选（已停滞）：[PR #9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378)。
  - [#9191](https://github.com/zeroclaw-labs/zeroclaw/issues/9191) — Cron 任务从不超时；过期的进行中锁会一直残留到重启。尚无修复 PR 附上。
  - [#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659) — 预算超限的 Code 回合在会话恢复后会丢弃已经流式输出的进度（09-06 新增）。
  - [#10670](https://github.com/zeroclaw-labs/zeroclaw/issues/10670) — `heartbeat.target` 拒绝频道实例复合键 `<type>.<alias>`，导致非默认频道实例无法工作（09-07 新增）。
- **S2 — 行为降级**
  - [#10408](https://github.com/zeroclaw-labs/zeroclaw/issues/10408) — 同一会话中第二条消息触发并行智能体运行。
  - [#9940](https://github.com/zeroclaw-labs/zeroclaw/issues/9940) — 回合上下文指示智能体使用无法解析的 cron 投递频道。
  - [#10115](https://github.com/zeroclaw-labs/zeroclaw/issues/10115) — 工具结果截断在模型的上下文之外不可见。
- **S3 — 次要**
  - [#10104](https://github.com/zeroclaw-labs/zeroclaw/issues/10104) — `zeroclaw-hardware` 特性门控的 lib 测试从未在 CI 中执行。
- **提供商/安全事项**
  - [#10662](https://github.com/zeroclaw-labs/zeroclaw/issues/10662) — Anthropic OAuth 系统前缀缓存标记低于 Anthropic 的缓存最小值，浪费一个断点槽位（P2）。
  - [#10606](https://github.com/zeroclaw-labs/zeroclaw/issues/10606) — 未认证的 `/health` 响应会泄露组件的 `last_error`；已接受为 P1，尚无修复 PR。

## 6. 功能请求与路线图信号
- **Anthropic 缓存效率是最清晰的近期路线图方向**：第三个缓存断点（[#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660)）已有堆叠的修复 PR（[#10666](https://github.com/zeroclaw-labs/zeroclaw/pull/10666)），其基础是已获批的 [PR #10623](https://github.com/zeroclaw-labs/zeroclaw/pull/10623)；社区还请求可配置的 1 小时提示词缓存 TTL（[#10663](https://github.com/zeroclaw-labs/zeroclaw/issues/10663)）。这些缓存改动最有可能进入下个版本。
- **安全加固**：[Issue #10606](https://github.com/zeroclaw-labs/zeroclaw/issues/10606)（在未认证的健康检查响应中清理组件错误）已被接受为 P1，且有望作为发布候选。
- **回合持久化路线图**：考虑到 [#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121) 的 S0 严重性，PR #10197/#9378 正在向“崩溃安全的 ACP 对话记录”收敛，很可能成为下一个小版本的主打修复。
- **更长远的用户可见功能**：可点击的对话记录 URL（[PR #10386](https://github.com/zeroclaw-labs/zeroclaw/pull/10386)）、会话提示词附件持久化（[PR #10407](https://github.com/zeroclaw-labs/zeroclaw/pull/10407)）以及多会话面板（[PR #9739](https://github.com/zeroclaw-labs/zeroclaw/pull/9739)）。

## 7. 用户反馈摘要
- **数据丢失是最大的痛点**：用户反映，已经显示的助手文本、工具调用和工具结果会在回合失败、切换会话、预算耗尽或进程退出后消失（[#9333](https://github.com/zeroclaw-labs/zeroclaw/issues/9333)、[#10121](https://github.com/zeroclaw-labs/zeroclaw/issues/10121)、[#10659](https://github.com/zeroclaw-labs/zeroclaw/issues/10659)）。
- **用户对并发的预期很明确**：每个会话一次只运行一个智能体；[#10408](https://github.com/zeroclaw-labs/zeroclaw/issues/10408) 表明这一预期目前被打破，带来了重复工作和重复回复。
- **频道与 cron 的配置令人困惑**：cron 投递默认值和 heartbeat.target 对频道实例寻址（`<type>.<alias>`）的处理方式不一致（[#9940](https://github.com/zeroclaw-labs/zeroclaw/issues/9940)、[#10670](https://github.com/zeroclaw-labs/zeroclaw/issues/10670)）。
- **重度 Anthropic 用户很在意成本控制**：缓存断点/TTL 问题（[#10660](https://github.com/zeroclaw-labs/zeroclaw/issues/10660)、[#10662](https://github.com/zeroclaw-labs/zeroclaw/issues/10662)、[#10663](https://github.com/zeroclaw-labs/zeroclaw/issues/10663)）反映出真实的费用压力，以及对减少缓存未命中的诉求。
- **满意度信号**：维护团队正在与资深/受信任的贡献者展开明显协作——在 [PR #9739](https://github.com/zeroclaw-labs/zeroclaw/pull/9739) 上完成范围受限的重连工作、保留原作者署名并批准 [PR #10623](https://github.com/zeroclaw-labs/zeroclaw/pull/10623)——这表明尽管缺陷积压，开源贡献者循环依然健康。

## 8. 积压观察
以下条目开启最久或已停滞，需要维护者关注：
- [PR #8966](https://github.com/zeroclaw-labs/zeroclaw/pull/8966)（07-11 创建）— 用量事件中的实时提供商身份，以及正确的上下文窗口上限；`needs-author-action`、XL，涉及大量频道/运行时路径。
- [PR #9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109)（07-17 创建）— 原生 Hailo-Ollama 提供商；`do-not-merge`。
- [Issue #9191](https://github.com/zeroclaw-labs/zeroclaw/issues/9191)（07-20 创建）— P1/S1：cron 任务缺少 wall-clock 超时；状态为 accepted/no-stale，但约 7 周过去仍无修复 PR 附上。
- [PR #9420](https://github.com/zeroclaw-labs/zeroclaw/pull/9420)（07-26 创建）— Anthropic OAuth 配置文件存储；`do-not-merge` + blocked。
- [PR #9378](https://github.com/zeroclaw-labs/zeroclaw/pull/9378)（07-26 创建）— ACP 失败/已取消回合的持久化；标记为 `stale-candidate` 和 `needs-author-action`，且功能重叠的较新 PR #10197 进展更快——存在重复风险，或需要进行合并。
- [PR #9447](https://github.com/zeroclaw-labs/zeroclaw/pull/9447)（07-27 创建）— 对不完整的 Anthropic 终止响应进行分类；`needs-author-action`。
- [PR #10241](https://github.com/zeroclaw-labs/zeroclaw/pull/10241)（08-22 创建）— 恢复受监督的 shell 审批路由；状态 `blocked`。
- [PR #10356](https://github.com/zeroclaw-labs/zeroclaw/pull/10356)（08-25 创建）— AnySearch 网络搜索提供商；`blocked`、`do-not-merge`、`needs-maintainer-review`。
- [Issue #10104](https://github.com/zeroclaw-labs/zeroclaw/issues/10104)（08-18 创建）— CI 从不执行硬件门控的 lib 测试；已接受为 P2，附有后续安排，但没有明显进展。

---

</details>