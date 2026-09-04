# Claude API 定价（Claude API Pricing）— 中文版

> 原文：`claude-api-pricing.md`（英文版，2026-09-04 核验）
> 本文为正文的完整中文翻译，价格数字、表格、链接与英文版一致。涉及模型名、API ID、Batch API、Prompt Caching 等术语保留英文以便对照官方文档。

---

# Claude API 定价：模型成本、Token 费率、缓存与批处理折扣

截至 **2026 年 9 月 4 日**，Anthropic 官方 Claude API 以每百万 Token（**MTok**）计价（美元），标准输入费率从 **Claude Haiku 4.5 的每百万输入 Token $1、每百万输出 Token $5**，到 **Claude Fable 5.1 的每百万输入 Token $10、每百万输出 Token $50**。批处理（Batch）可将异步工作负载的标准 Token 价格降低 50%，提示词缓存（Prompt Caching）则单独按写入和读取计费。价格为时效性数据——本快照核验于 2026 年 9 月 4 日；在制定预算前请复核 Anthropic 的实时定价。

> **速览（At a glance）**
>
> - Claude API 按用量计费，不是固定月度订阅。
> - 输入与输出 Token 费率不同；输出更贵。
> - Haiku 4.5 的标准费率最低（每 MTok $1 / $5）。
> - Sonnet 5 是通用型候选，其 $2 / $10 费率现已成为标准价。
> - Batch API 可将异步工作的标准 Token 价格降低 50%。
> - 提示词缓存仅在稳定前缀在缓存 TTL 内被复用时才划算。
> - 本文中关于 ApiFlux 的价格与能力均为**厂商发布口径**，非独立基准测试结果。

## 快速回答：Claude API 多少钱？

Anthropic 官方 Claude API 当前的标准模型费率为：

| 模型 | 输入（每 MTok） | 输出（每 MTok） | 定位 |
|---|---:|---:|---|
| Claude Fable 5.1 | $10 | $50 | 高难度推理与长周期 Agent 任务，视可用性而定 |
| Claude Opus 5 | $5 | $25 | 复杂编码、Agent 与企业级工作负载 |
| Claude Sonnet 5 | $2 | $10 | 通用型速度与能力平衡（现为标准定价） |
| Claude Haiku 4.5 | $1 | $5 | 快速、低成本任务与高吞吐工作负载 |

**快速决策——你该从哪个模型入手？**

| 如果你的需求是… | 建议先评估… | 主要权衡 |
|---|---|---|
| 最低的列示 Token 价格 | Haiku 4.5 | 可能需要更多验证 |
| 通用生产的性价比平衡 | Sonnet 5 | 用你自己的任务集测试 |
| 复杂编码或长周期 Agent | Opus 5 | Token 费率更高 |
| 不计成本的高难度推理 | Fable 5.1 | 价格更高且有可用性风险 |
| 离线批量处理 | Batch API | 无实时响应 |
| 单一 Key、统一余额、路由与日志 | 类似 ApiFlux 的网关 | 多一个待验证的依赖 |

Anthropic 的模型总览将 Fable 5.1 定位为高难度推理、Opus 5 为复杂 Agent 编码与企业工作、Sonnet 5 为速度与智能的平衡、Haiku 4.5 为最低延迟与价格。这些是**厂商定位表述**，并非独立基准结果。生产决策请用你自己的数据、代表性提示词进行测试。

以上价格覆盖标准第一方 Token 用量。你的实际成本还可能包含：

- 输入 Token，包括相关工具定义与工具结果。
- 模型生成的输出 Token。
- 提示词缓存写入与缓存读取。
- 异步处理时的 Batch API 价格。
- 服务端工具费用，例如 Web Search。
- 可选服务修饰项，如仅限美国推理（`inference_geo: "us"`，1.1× 系数）或 fast mode。
- Amazon Bedrock、Google Cloud、Microsoft Foundry 或其他平台不同的定价、计费单位、可用性与生命周期策略。

## Claude API 成本计算器

Anthropic 在其[当前定价文档](https://platform.claude.com/docs/en/about-claude/pricing)中列出 Sonnet 5 为每百万输入 Token $2、每百万输出 Token $10。基本 Token 成本公式为：

```text
基础输入成本  = 每月请求数 × 平均输入 Token  ÷ 1,000,000 × 输入费率
基础输出成本  = 每月请求数 × 平均输出 Token  ÷ 1,000,000 × 输出费率
含重试成本    = 基础成本 × (1 + 重试率)
每个有效结果成本 = 总成本 ÷ 有效生产结果数
```

不想手算的话，可使用我们的**交互式 [Claude API 成本计算器](claude-api-cost-calculator.html)**。它接受月请求数、平均输入与输出 Token、模型、Batch 与提示词缓存开关、重试率、工具调用、以及 ApiFlux 路由开关，返回月度输入/输出成本、缓存写入与读取成本、Batch 估算、直接 Anthropic 成本、ApiFlux 厂商标价估算、以及每个有效结果的成本。

以 Claude Sonnet 5、上述费率为例做一次简短试算（一次请求 20,000 输入 Token、4,000 输出 Token）：

```text
输入： 20,000 ÷ 1,000,000 × $2  = $0.04
输出：  4,000 ÷ 1,000,000 × $10 = $0.04
单次请求估算成本                     = $0.08
```

这是对所示模型 Token 的简单估算，不包含缓存操作、服务端工具费用、税费、商务合同条款、失败请求处理或各平台的计费差异。

## Claude API 计费方式

### 输入与输出 Token 定价不同

在当前模型阵容中，输出 Token 普遍比输入 Token 更贵。一个生成不必要长回答的应用，可能比一个发送大量稳定上下文、并要求简洁回复的应用贵得多。

在通过降低模型档位来压成本之前，先检查你是否可以：

1. 设置合理的最大输出长度。
2. 要求结构化、简洁的回答。
3. 从每次请求中去掉无关上下文。
4. 用提示词缓存复用稳定指令。
5. 把简单、可预测的任务路由到更低成本的模型。
6. 同时衡量重试与人工复核成本，而不只看原始 Token 用量。

### 工具、重试与失败循环同样计入成本

带工具的 Claude API 请求并非只按可见的用户提问计费。Token 用量可包含工具定义与工具结果（它们会进入模型上下文），外加一条自动的 tool-use 系统提示词。Anthropic 定价文档目前将 Web Search 列为**每 1,000 次搜索 $10**，外加搜索生成内容的 Token 成本。Web Fetch **不额外收费**，只按进入对话上下文的 Token 计费。较新的服务端工具（代码执行、文本编辑器、Computer Use、Browser Use）会增加各自的输入 Token 开销，某些情况下按执行时间计费。

估算工具型应用时，至少记录：用户与系统输入 Token、工具定义 Token、工具结果 Token、搜索或其他服务端工具调用、输出 Token、重试与失败工具循环、缓存写入与读取、以及人工复核或下游处理。一个在纯聊天测试中显得便宜、但一旦接上搜索、代码执行、检索或多步 Agent 循环的模型，其成本画像可能完全不同。

## 当前 Claude API 模型价格

下表汇总了 Anthropic [模型总览](https://platform.claude.com/docs/en/models/overview)列出的主要当前模型。价格为**第一方 Claude API 基础费率**，核验于 2026 年 9 月 4 日。

| 模型 | API ID | 输入/输出（每 MTok） | 上下文窗口 | 最大输出 |
|---|---|---:|---:|---:|
| Claude Fable 5.1 | `claude-fable-5-1` | $10 / $50 | 1M tokens | 128K tokens |
| Claude Opus 5 | `claude-opus-5` | $5 / $25 | 1M tokens | 128K tokens |
| Claude Sonnet 5 | `claude-sonnet-5` | $2 / $10 | 1M tokens | 128K tokens |
| Claude Haiku 4.5 | `claude-haiku-4-5-20251001` | $1 / $5 | 200K tokens | 64K tokens |

模型 ID 与可用性因平台而异。Anthropic 自 4.6 代起使用不带日期的模型 ID 格式（例如 `claude-opus-5`），每个 ID 是固定快照而非常青指针——参见[模型 ID 与版本文档](https://platform.claude.com/docs/en/about-claude/models/model-ids-and-versions)。即使旧模型已退役，也可能出现在定价或生命周期文档中供参考。不要从旧文章直接复制模型 ID，应先查当前[模型 API](https://platform.claude.com/docs/en/models/overview)与生命周期文档。

可复现的应用应记录：精确模型 ID、提示词版本、工具配置、日期与评估结果。模型别名或合作云部署名可能不等同于第一方 Claude API 标识符。

## 应该选哪个 Claude 模型？

**决策树：**

```text
你需要立即得到响应吗？
├── 是 → 使用同步 Messages API
└── 否
    ├── 大批量 → 评估 Batch API
    └── 反复长上下文 → 评估 Prompt Caching

任务是否可预测且易于验证？
├── 是 → 从 Haiku 4.5 开始
└── 否
    ├── 通用生产工作 → 评估 Sonnet 5
    └── 复杂编码 / 推理 → 基准测试 Opus 5 或 Fable 5.1
```

### Claude Haiku 4.5

**价格：** 每 MTok 输入 $1 / 输出 $5。

**适合：** 分类、抽取、路由、短文本转换，以及其他输出可自动校验的高吞吐工作负载。

**不适合：** 需要深度多步推理的工作，或"便宜响应会反复失败、需不断重试"的场景。

**建议衡量：** 正确分类/抽取率、平均输出长度、重试率、目标流量下的延迟、人工复核负担，以及每个有效结果的综合成本。一个便宜但需反复重试或人工修正的响应，可能比一个首次即通过校验、价格更高的响应更贵。

### Claude Sonnet 5

**价格：** 每 MTok 输入 $2 / 输出 $10，现为标准定价。

**适合：** 通用助手功能、编码工作流、结构化生成，以及中等复杂度的 Agent 任务。

**不适合：** 模型无法通过严格校验、或需要更强模型才能完成的长多步推理。

**建议衡量：** 首轮成功率、重试率、延迟、输出长度、每个有效结果成本——尤其是应用涉及工具调用、长上下文、严格 JSON、多语言输出或可靠边缘用例处理时。关于编码型 LLM 的更全面对比，参见我们的 [2026 最佳编码 LLM](https://github.com/tom61-bl/best-llm-for-coding-2026) 指南。

### Claude Opus 5

**价格：** 每 MTok 输入 $5 / 输出 $25。

**适合：** 复杂代码修改、更深推理、Agent 编码，以及"更少失败尝试可抵消更高 Token 费率"的企业工作负载。

**不适合：** 简单、高吞吐、易验证、Haiku 或 Sonnet 一次就能通过的任务。

**建议衡量：** 在固定任务集上的成功结果、重试负担、延迟与复核工作量。不要因为 Anthropic 的定位就把"Opus 5 对每个项目都最具性价比"当成普遍结论。

### Claude Fable 5.1

**价格：** 每 MTok 输入 $10 / 输出 $50。

**适合：** 高难度推理与长周期 Agent 工作，即低成本模型在评估中表现不足的场景。

**不适合：** 大多数常规工作负载——其价格显著高于 Sonnet 5 或 Haiku 4.5。

**建议衡量：** 在低成本模型失败的具体工作负载上的任务级成功率，以及可用性与接入。不应把"有限可用"或"处于预览期变化中的产品"当作人人可用的生产依赖。

## 提示词缓存成本

提示词缓存可降低反复发送同一提示词前缀的成本，例如大段系统指令、参考文档、工具定义集或对话历史。当稳定内容较大、且在缓存 TTL 内被复用时最划算。Anthropic 的[提示词缓存文档](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)列出以下相对模型基础输入价格的标准缓存系数：

| 缓存操作 | 定价系数 | TTL 或含义 |
|---|---:|---|
| 5 分钟缓存写入 | 基础输入的 1.25× | 缓存有效期为 5 分钟 |
| 1 小时缓存写入 | 基础输入的 2× | 缓存有效期为 1 小时 |
| 缓存读取（命中） | 基础输入的 0.1× | 应用于缓存命中 |

Anthropic 当前定价表将 Claude Fable 5.1 的缓存命中与刷新列为**每 MTok $0.25，即其 $10 基础输入费率的 0.025×**。由于缓存定价因模型而异且时效性强，发布前请复核各模型的定价表。

按标准系数计算：5 分钟缓存写入在**一次**成功缓存读取后回本（1.25× 写入 vs. 第二次完整 1× 输入）；1 小时缓存写入在**两次**成功读取后回本（2× 写入 vs. 三次 1× 输入）。例如，对输入费率 $2/MTok 的模型，同一合格前缀发送三次、不用缓存需支付三次基础输入费用；用 1 小时缓存则为首写 2× 加两次读取各 0.1×，合计 2.2×。

> **关于叠加的提示：** Batch、缓存与数据驻留修饰项可以互相作用，但这些修饰项并不会让账单总额变得可自动预测。请分别计算每一类 Token，并确认所选模型与平台的计费规则。

以上盈亏平衡分析假设：前缀足够长、可被缓存；断点之前的内容保持不变；后续请求在 TTL 内到达；且确实发生缓存命中。输出 Token 与未缓存的尾部 Token 不在这个狭窄对比范围内。

### 如何设计对缓存友好的提示词

把稳定内容放在可变内容之前：

```text
稳定的系统指令
稳定的工具与模式
稳定的参考文档
缓存断点
可变的用户提问
```

断点之前的内容一旦变化，可能使缓存段失效。因此缓存策略应用与生产一致的请求形态来测试，而不是只用一个小演示提示词。

## Batch API 定价

Anthropic 的 [Message Batches API](https://platform.claude.com/docs/en/build-with-claude/batch-processing) 面向不需要立即响应的大量请求。官方文档称其提供**相比标准 API 定价 50% 的折扣**，并以异步方式处理请求。

| 模型 | 标准输入/输出（每 MTok） | Batch 输入/输出 |
|---|---:|---:|
| Claude Fable 5.1 | $10 / $50 | $5 / $25 |
| Claude Opus 5 | $5 / $25 | $2.50 / $12.50 |
| Claude Sonnet 5 | $2 / $10 | $1 / $5 |
| Claude Haiku 4.5 | $1 / $5 | $0.50 / $2.50 |

**同步 vs 异步——你需要哪个？**

| | 同步 Messages API | Message Batches API |
|---|---|---|
| 延迟 | 实时响应 | 大多数批处理一小时内完成 |
| 价格 | 标准费率 | 标准费率五折 |
| 适合 | 用户正在等待回答 | 评估、离线分析、批量分类、定时任务 |
| 限制 | 模型速率限制 | 每批 100,000 请求或 256 MB；须在 24 小时内完成 |
| 结果处理 | 内联响应 | JSONL 文件，结果可下载 29 天 |

请把批处理数字视为基于官方公布的 50% 折扣计算出的示例，而非所有平台或计费安排的通用报价。提示词缓存与其他定价修饰项会与批处理定价相互作用。

Batch API 还有操作约束。请求独立处理，**不保证保持输入顺序**，且不支持 streaming、fast mode 与 `max_tokens: 0`。请使用唯一 `custom_id` 值，并围绕这些 ID 建立结果匹配策略。

## Claude API 与 Claude 订阅版的区别

Claude **API** 是"用多少付多少"：按你的应用实际使用的 Token 与功能计费。它不是消费者或团队订阅的预付费包，订阅也不应被当作预付费的 API 额度。

单独的 Claude 消费者或团队套餐（例如聊天/桌面订阅档）购买的是产品使用权，不是 API 容量。如果你在构建应用，请把 API Token 成本、消费上限与速率限制，与你持有的任何 Claude 订阅分开规划。

## Claude API 与 Bedrock、Vertex AI 的对比

除特别说明外，本文价格均指**第一方 Claude API**。不应将其自动套用到 Amazon Bedrock、Google Cloud Vertex AI、Microsoft Foundry、Claude Platform on AWS、经销商或网关等其他提供方。

| 平台 | 计费 | 说明 |
|---|---|---|
| Claude API（第一方） | 按每 MTok Token 费率 | 即本文中的价格与系数 |
| Amazon Bedrock | 云厂商开票；区域/全球端点 | 区域端点相对全球端点含 10% 溢价 |
| Google Cloud Vertex AI | 云厂商开票；区域/全球/多区域端点 | 区域端点相对全球端点含 10% 溢价 |
| Claude Platform on AWS | 通过 AWS Marketplace 按 Claude Consumption Units（CCU） | Token 用量按每 CCU $0.01 换算 |

合作伙伴平台可能使用不同的计费单位、区域定价、模型 ID、可用性规则与退役节奏。对比提供方时，先统一口径：

1. 尽量使用同一模型代际与行为。
2. 确认价格分别覆盖输入、输出、缓存与工具的哪部分。
3. 检查是否适用税费、云费用或区域系数。
4. 核实精确的模型 ID 与可用性。
5. 用同一固定工作负载、同一成功标准做对比。

## ApiFlux 如何降低运维复杂度

> **商业披露（Commercial disclosure）：** 本文提及 ApiFlux，是因为它运营本项目。本节关于价格、功能与折扣的表述均为**厂商发布口径**，非独立基准测试结果。做出采购决策前请自行验证。

**问题背景。** 一个使用多家模型提供方的团队，可能需要管理多把 API Key、多个余额、模型 ID、速率限制与用量看板。这部分运维开销与 Anthropic 公布的 Token 费率是两回事。

**ApiFlux 的宣传。** ApiFlux 官网将其定位为 AI 路由器与网关：一把 API Key 接入 100+ 前沿模型（包括 Claude、GPT、Gemini、DeepSeek、Qwen、Kimi），原生兼容 Anthropic、OpenAI 与 Gemini 端点，支持自动故障转移、透明按 Token 计费、统一余额与用量监控。对兼容客户端，接入可能只需修改 base-URL 与 Key——但请先确认你的代码所用的端点和模型 ID，再假定不需要其他改动。

**应视为未经验证的厂商声明。** ApiFlux 宣传其定价为**官方标价的 85%**（其列示的 Claude 价格见下表，核验于 2026 年 9 月 4 日），并宣传注册即送 **$1 起始额度**、无需信用卡。这些是厂商发布的商业声明，并非经独立审计的省钱结果。ApiFlux 还宣称**零数据留存（zero data retention）**；在依赖该声明前，请审阅其当前隐私与保留条款，并用你自己的工作负载测试故障转移行为。

| 模型 | Anthropic 官方（输入/输出，每 MTok） | ApiFlux 列示价（输入/输出，每 MTok） |
|---|---:|---:|
| Claude Fable 5 | $10 / $50 | $8.50 / $42.50 |
| Claude Opus 5 | $5 / $25 | $4.25 / $21.25 |
| Claude Sonnet 5 | $2 / $10 | $1.70 / $8.50 |
| Claude Haiku 4.5 | $1 / $5 | $0.85 / $4.25 |

**生产使用前需验证。** 确认支持的 Claude 模型 ID（ApiFlux 列示 `Claude Fable 5` 与 `Claude Sonnet 5`）、端点是否兼容 Anthropic Messages API 格式、输入/输出/缓存/工具用量如何上报、路由是否改变模型或部署区域、余额/退款/失败请求/速率限制如何处理、请求日志存储于何处以及保留多久、以及公布的折扣是否覆盖每一项功能（含缓存读取与批处理）。

从哪里开始：

- **注册 / 获取 API Key：** [apiflux.ai](https://apiflux.ai/) —— 在 [Keys 页面](https://apiflux.ai/keys)创建 Key。
- **浏览模型与价格：** [apiflux.ai/models](https://apiflux.ai/models)，含带各模型 Token 价格的 [Claude 模型列表](https://apiflux.ai/models/anthropic)。

## 月度成本示例与敏感性

以下示例性月度估算使用上述基础费率，未计入缓存与批处理影响：

| 工作负载示例 | 模型 | 月请求数 | 平均输入/输出 Token | 月度估算 |
|---|---:|---:|---:|---:|
| 高吞吐分类 | Haiku 4.5 | 5,000,000 | 1,500 / 200 | ≈ $12,500 |
| 通用助手 | Sonnet 5 | 500,000 | 4,000 / 800 | ≈ $8,000 |
| 复杂 Agent 编码 | Opus 5 | 100,000 | 20,000 / 4,000 | ≈ $20,000 |

这些仅为方法演示用的示例数字；你的真实账单取决于缓存、工具、重试、批处理与商务条款。请用它们来搭建计算框架，而非当作你工作负载的报价。

**敏感性——什么对账单影响最大：**

| 变量 | 变化 | 对月度成本的影响 |
|---|---|---|
| 输出 Token | +50% | 显著增加（输出费率是输入的 5 倍） |
| 重试率 | 0% → 10% | 大约增加 10% 的 Token 成本 |
| 缓存命中率 | 0% → 80% | 显著降低重复输入成本 |
| 批处理使用率 | 0% → 100% | 标准 Token 费用降低 50% |
| 人工复核 | 增加 | 可能抵消 API 省下的钱 |

如果通过 ApiFlux 之类的网关路由请求，其列示的每 Token 费率比官方标价低 15%（例如 Claude Opus 5 为 $4.25 / $21.25，而非 $5 / $25），会降低 Token 成本部分。在假设省钱之前，请把网关自身费用、故障转移行为与缓存记账一并考虑进去。

## Claude API 定价 FAQ

### Claude API 是按月计费还是按用量计费？

第一方 Claude API 按用量计费。费用取决于 Token 用量与适用功能，而非单一固定的 API 订阅价。单独的 Claude 消费者或团队订阅不应被当作预付费的 API 额度。

### Claude API 定价中的 MTok 是什么意思？

MTok 指一百万 Token。输入与输出 Token 分别定价，费率取决于所选模型。

### 哪个 Claude API 模型最便宜？

在当前第一方阵容中，Claude Haiku 4.5 的标准费率最低：每百万输入 Token $1、每百万输出 Token $5。它是否是生产中的最便宜选择，取决于任务成功率、重试、延迟与复核成本。

### 为什么 Claude Sonnet 5 定价是 $2 / $10？

Sonnet 5 的 $2 输入 / $10 输出费率在发布时被宣布为截至 2026 年 8 月 31 日的首发价。原定 2026 年 9 月 1 日上调至 $3 / $15 的计划已取消，因此 $2 / $10 现已成为标准价格，见 Anthropic 的[定价文档](https://platform.claude.com/docs/en/about-claude/pricing)。

### Claude API 有免费档吗？

在本指南审阅的资料来源中，Anthropic 的 API 文档并未确立永久的免费 API 档。请查看当前 Claude Console 的计费文档，了解是否有账户专属额度或促销，不要假定消费者版 Claude 套餐结构可映射到 API 用量。（另外，ApiFlux 官网目前宣传 $1 起始额度；资格与条款可能变化，注册前请确认。）

### 提示词缓存能降低总账单吗？

在所选 TTL 内发生缓存命中时，它可以降低反复发送合格稳定前缀的成本。它不会自动降低输出 Token、可变输入、工具调用或应用中每一次请求的成本。

### Claude Message Batches API 能便宜多少？

Anthropic 文档写明 Message Batches API 相比标准 API 定价提供 50% 折扣。批处理请求是异步的，因此该折扣适合可以等待处理与取回的工作负载。

### Claude Code 相对 Claude API 如何计费？

Claude Code 运行在相同的底层 Claude 模型上，因此其 Token 用量按本文中的模型费率计费。若通过 Anthropic 套餐或 API Key 使用 Claude Code，请确认适用哪条计费路径、提示词缓存是否开启，以及你账户生效的速率限制或订阅上限。

### Claude API 网关会改变 Anthropic 的官方价格吗？

网关可能附加自身费用、加价、货币换算或运营条款。请单独确认网关当前的定价与用量记账。本文把 ApiFlux 描述为一种路由与管理选项，而非 Anthropic 底层费率更低的证明。

### Claude API 价格在 Bedrock 与 Vertex AI 上一样吗？

不一定。云市场可能有不同的定价、计费单位、模型 ID、区域规则、可用性与生命周期策略。请按各平台当前的官方文档逐一对比。

## 来源与更新历史

本文为基于 Anthropic 官方文档编写的编辑指南，核验于 **2026 年 9 月 4 日**：

- [Anthropic Claude API 定价](https://platform.claude.com/docs/en/about-claude/pricing)
- [Anthropic 模型总览](https://platform.claude.com/docs/en/models/overview)
- [Anthropic 提示词缓存](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [Anthropic Message Batches API](https://platform.claude.com/docs/en/build-with-claude/batch-processing)
- [Anthropic API 速率限制](https://platform.claude.com/docs/en/api/rate-limits)
- [Anthropic 模型 ID 与版本](https://platform.claude.com/docs/en/about-claude/models/model-ids-and-versions)
- [Anthropic 模型退役](https://platform.claude.com/docs/en/about-claude/model-deprecations)
- [Claude 定价页](https://claude.com/pricing)
- [ApiFlux 首页](https://apiflux.ai/) —— 厂商声明：比官方标价低 15%、$1 起始额度、零数据留存
- [ApiFlux 模型目录](https://apiflux.ai/models)
- [ApiFlux Claude 模型价格](https://apiflux.ai/models/anthropic)

**更新触发条件。** 当 Anthropic 变更模型费率、模型 ID、缓存系数、Batch API 条款、工具价格、消费上限或生命周期日期时，以及当 ApiFlux 变更价格、支持的模型、隐私或保留条款、页面 URL 时，立即复核。每月检查模型 ID 与生命周期；每季度复核 FAQ 与结构。

**变更日志（Changelog）**

- **2026-09-04** — 确认 Sonnet 5 维持 $2 / $10（标准定价）。将 Fable 5.1 缓存命中费率更新为 $0.25 / MTok（0.025×）。删除未经核实的"新用户免费额度"表述。新增 ApiFlux 商业披露、厂商声明标注、模型决策树、交互式成本计算器、"vs 订阅"与"vs Bedrock/Vertex AI"小节、敏感性分析、就地来源引用。建立 FACTS.md 事实账本。

本文不声称拥有独立基准排名、保证省钱、保证可用性或保证搜索排名。模型建议是基于已公布定价与厂商描述定位的条件性编辑指导，请针对你自己的工作负载进行验证。

---

*中文版由英文正文完整翻译，核验日期与数据同英文版（2026-09-04）。术语差异处保留英文便于对照官方文档。*
