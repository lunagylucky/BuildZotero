---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: 项目-BuildZotero-13专业知识文档-AI应用与产品经理
DateCreated: 2026-04-19 22:55
DateModified: 2026-04-19 22:55
Type:
- doc
Status:
- doing
Version:
- v1.0
CardStatus: true
CardType:
- card-project
tags:
- Pattern/Memo
- Topic/工具技能/工作笔记
- Topic/专业知识
- Topic/AI应用
- Topic/产品经理
RelatedNote:
- '[[项目-BuildZotero-01简历撰写]]'
- '[[项目-BuildZotero-02-任务主线与业务面试问答]]'
- '[[项目-BuildZotero-06项目介绍梳理]]'
- '[[项目-BuildZotero-08简历项目介绍-一分钟与三分钟版]]'
- '[[项目-BuildZotero-09宝洁行为面八大问题]]'
- '[[项目-BuildZotero-10产品经理岗面试问答]]'
- '[[项目-BuildZotero-11商业数据分析岗面试问答]]'
- '[[项目-BuildZotero-12投行咨询研究岗面试问答]]'
RelatedProjects:
CardRecord: 专业知识文档，覆盖 AI 应用（LLM/RAG/Agent/Prompt/评估）与产品经理（框架/指标/商业化）两大维度。与 BuildZotero 各面试答案相互引用。
---

**系列导航**（BuildZotero 面试材料）

| 上一页 | 下一页 |
| --- | --- |
| [[项目-BuildZotero-12投行咨询研究岗面试问答]] | （系列终点） |

---

# 专业知识文档｜AI 应用 + 产品经理

> **用途**：面试前系统扫一遍——涵盖被问到几乎所有"专业名词"时的**准确定义、工程含义、与 BuildZotero 的关联、可引用的外部资料**。
>
> **阅读建议**：按面试岗位侧重——
> - **产品经理岗**：重点读 §二（PM 框架）+ §三（AI 产品方法论）
> - **AI/Agent 岗**：重点读 §四（LLM/RAG/Agent 技术栈）+ §五（Prompt 工程）
> - **数据分析岗**：重点读 §三（指标体系）+ §六（评估与 A/B）
> - **投行/咨询/研究岗**：重点读 §一（AI 产业版图）+ §七（商业化与竞争格局）

---

## 一、AI 产业版图（2026）

### 1.1 分层理解

AI 产业自底向上四层：

```
应用层（Application Layer）
  ├── 垂直行业：Harvey（法律）、Hippocratic（医学）、BloombergGPT（金融）、BuildZotero（科研）…
  └── 通用场景：ChatGPT、Copilot、Jasper、Perplexity…
─────────────────────────────────────
平台层（Platform Layer）
  ├── Agent 框架：LangChain、LlamaIndex、AutoGen、CrewAI…
  ├── 向量数据库：Pinecone、Weaviate、Chroma、Milvus…
  └── 企业级 AI：Databricks、Palantir、Notion AI…
─────────────────────────────────────
模型层（Model Layer）
  ├── 闭源：GPT、Claude、Gemini…
  └── 开源：Llama、DeepSeek、Qwen、Mistral…
─────────────────────────────────────
基础层（Infrastructure Layer）
  ├── 芯片：NVIDIA、Google TPU、AMD…
  ├── 云：AWS、Azure、GCP、阿里云、华为云…
  └── 训练基础设施：Ray、Megatron…
```

**投资与就业洞察**：
- 基础层 + 模型层：资本密集、巨头格局已定
- **平台层 + 应用层：机会最大，护城河在"垂直深度 + 工作流嵌入"**
- BuildZotero 是**应用层 + 垂直行业（科研）** 的典型

### 1.2 2026 年关键趋势

| 趋势 | 本质 | 影响 |
|---|---|---|
| **从 Prompt Engineering → Context Engineering** | 设计完整信息环境，不只是提示词 | PM 需理解系统全栈 |
| **Agentic Workflow** | 多 Agent 协作完成复杂任务 | 应用形态从"对话" → "工作流" |
| **GraphRAG** | 知识图谱 + 向量检索 | 精度可达 99% |
| **Self-Correcting RAG** | 自评估 + 多轮迭代检索 | 降低幻觉 60–80% |
| **Multimodal Agents** | 图文音视频统一处理 | 应用边界扩展 |
| **On-device AI** | 模型部署到端侧 | 隐私 + 低延迟 |
| **AI 编程** | Cursor、Copilot 融入开发 | 工程门槛下降 |

参考：[RAG in 2026: Bridging Knowledge and Generative AI — Squirro](https://squirro.com/squirro-blog/state-of-rag-genai) / [Retrieval-Augmented Generation (RAG): Complete Guide 2026 — Data Expertise](https://www.dataexpertise.in/retrieval-augmented-generation-rag-guide/) / [Prompt engineering techniques: Top 6 for 2026 — K2view](https://www.k2view.com/blog/prompt-engineering-techniques/) / [Agentic Engineering Roadmap 2026 — Towards Agentic AI](https://towardsagenticai.com/agentic-engineering-roadmap-skills-tools-resources-2026/)

---

## 二、产品经理核心框架

### 2.1 四大方法论速查

| 框架 | 用途 | 口诀 |
|---|---|---|
| **CIRCLES** | 产品设计题（"设计一个 X 产品"） | Comprehend / Identify / Report / Cut / List / Evaluate / Summarize |
| **STAR-R** | 经历题 | Situation / Task / Action / Result / Reflection |
| **AARRR** | 增长漏斗 | Acquisition / Activation / Retention / Revenue / Referral |
| **HEART** | 体验指标 | Happiness / Engagement / Adoption / Retention / Task Success |

参考：[Product Manager Interview Questions and Answers — Enhancv](https://enhancv.com/blog/product-manager-interview-questions-and-answers/) / [What is the HEART framework — LogRocket](https://blog.logrocket.com/product-management/what-is-the-heart-framework/) / [HEART & AARRR frameworks — PM Playbook](https://simran-pm.medium.com/heart-aarrr-frameworks-in-product-management-7596b6c717de) / [Product Metrics: AARRR vs HEART vs North Star — Hyperact](https://www.hyperact.co.uk/blog/product-metrics-frameworks)

### 2.2 CIRCLES 详解

Lewis C. Lin 《Decode and Conquer》提出，用于产品设计题的 7 步方法：

1. **Comprehend the Situation**：澄清问题上下文（是什么产品、在什么市场、什么规模）
2. **Identify the Customer**：定义目标用户（画像、使用场景）
3. **Report the Customer's Needs**：列出用户需求（用 Jobs-to-be-Done 思路）
4. **Cut through Prioritization**：排优先级（按影响/可行/频次）
5. **List Solutions**：头脑风暴 3+ 方案
6. **Evaluate Trade-offs**：对比方案，选 1 个推荐
7. **Summarize**：总结结论与下一步

**BuildZotero 示例**：在 [[项目-BuildZotero-10产品经理岗面试问答#Q11｜假如你是 Notion AI 的 PM，你会怎么做产品？|Q11 Notion AI 题]] 里就是套用这套框架。

### 2.3 AARRR 详解

**Acquisition（获客）**：用户怎么找到你
- 渠道：SEO / SEM / 口碑 / 内容 / 合作
- 指标：访客数、获客成本 CAC

**Activation（激活）**：用户首次积极体验
- 关键：Aha Moment（用户第一次感受到产品价值的瞬间）
- 指标：激活率（注册→关键动作完成率）

**Retention（留存）**：用户持续回访
- 次留 / 7留 / 30留 / 90留
- Stickiness = DAU/MAU（>0.2 健康）

**Revenue（收入）**：用户付费
- ARPU、LTV、付费转化率

**Referral（推荐）**：用户带新
- NPS（净推荐值）、K-Factor（病毒系数）

**BuildZotero 映射**：
- Acquisition：课题组口碑 + 小红书 demo
- Activation：私有化部署 + Onboarding 培训 → 3 周内首次综述产出
- Retention：3 个月留存课题组占比
- Revenue：（未商业化）潜在路径 — 私有化年费 + Pro 版
- Referral：课题组内 5 人扩展 → 实验室间扩展

### 2.4 HEART 详解

Google 发明的用户体验评估框架：

| 维度 | 定义 | 典型指标 |
|---|---|---|
| **Happiness** | 用户主观满意度 | NPS、CSAT、应用商店评分 |
| **Engagement** | 用户与产品的互动深度 | 单次使用时长、频次、深度 |
| **Adoption** | 新用户采纳关键功能 | 关键功能首次使用率 |
| **Retention** | 用户持续回访 | 留存曲线、活跃用户数 |
| **Task Success** | 用户任务完成 | 完成率、错误率、任务耗时 |

**GAM 拆解**（每个维度都要定义）：
- **Goals（目标）**：想让用户做什么
- **Actions（动作）**：用户的具体行为
- **Metrics（指标）**：怎么度量

### 2.5 北极星指标（North Star Metric, NSM）

**定义**：最能反映产品价值交付的单一核心指标。

**选择原则**：
1. 反映用户价值（不是单纯流量）
2. 反映业务健康（可预测收入）
3. 可量化、可追踪、可影响

**案例**：
| 公司 | NSM |
|---|---|
| Facebook | 日活跃用户（DAU） |
| Airbnb | 预订夜数 |
| Amazon | 每月购买用户数 |
| Netflix | 每月观看时长 |
| Spotify | 每月听歌时长 |
| BuildZotero | **每月完成综述的课题组数** |

### 2.6 需求优先级方法

| 方法 | 评分维度 | 适用场景 |
|---|---|---|
| **ICE** | Impact / Confidence / Ease | 快速排序 |
| **RICE** | Reach / Impact / Confidence / Effort | 有用户规模数据时 |
| **Kano Model** | 必备/期望/兴奋/无差异/反向 | 功能定位 |
| **MoSCoW** | Must / Should / Could / Won't | 版本规划 |
| **价值成本矩阵** | 用户价值 × 开发成本 | 可视化排序 |

**BuildZotero P0–P6 排序**用的就是"依赖关系 + 价值 + 不确定性"多轴打分，本质是 **RICE 变体**。

### 2.7 PRD（产品需求文档）标准结构

BuildZotero 的 PRD 结构可作参考：

```markdown
1. 产品概述与定位
2. 目标与非目标
3. 用户画像
4. 使用场景（5 个以上典型场景）
5. 功能需求
   - 5.1 功能列表（优先级）
   - 5.2 详细需求（每个功能的输入/处理/输出）
6. 非功能需求
   - 性能、可用性、安全、合规
7. 技术架构
8. 数据模型
9. 指标与成功标准
10. 风险与依赖
11. 路线图
12. Open Questions
```

---

## 三、AI 产品方法论

### 3.1 AI 产品 vs 传统产品的 5 大差异

| 维度 | 传统产品 | AI 产品 |
|---|---|---|
| **输出确定性** | 确定（按规则） | 概率（随机性内生） |
| **迭代方式** | 代码改动 | 代码 + Prompt + 数据三向迭代 |
| **评估方式** | 功能测试 | 评估集 + 人工评审 |
| **用户预期管理** | 按功能说明 | 要明说"草案生成" |
| **成本模型** | 固定成本 | 按 Token/API 调用变动 |

### 3.2 AI 产品四象限（上/下线决策）

|  | 错误成本低 | 错误成本高 |
|---|---|---|
| **准确率高** | ✅ 直接上线 | ⚠️ 加人工核对层 |
| **准确率低** | ⚠️ "AI 辅助" 标识 | ❌ 不上线 |

**BuildZotero 实例**：
- 标签抽取（错误成本低 + 准确率 80–90%）→ 上线，允许用户手改
- 理论识别（错误成本高 + 准确率不稳）→ 加 `Theory/new-` 前缀强制核对
- 自动引用（错误成本高 + 可追溯）→ 上线，强制带锚点
- 自动综述（错误成本中高）→ 明说"草案"

### 3.3 防幻觉五层防御

1. **预防**：输入侧治理（结构化 Schema，不是裸全文）
2. **检测**：与知识库对比标记异常（如 `Theory/new-`）
3. **兜底**：输出带证据锚点（`[number]` + 一键跳转）
4. **修复**：白盒可改（Zotero 条目可手改）
5. **沟通**：文档 + 培训明确"AI 是草案生成器"

### 3.4 AI 产品 PRD 额外章节

除标准 PRD 外，AI 产品还需：
- **输入输出 Schema**（结构化定义）
- **评估集（≥30 条样本）**
- **可接受错误率上限**
- **模型选型与 Tradeoff**
- **Prompt 版本管理策略**
- **故障降级策略**
- **成本预估（Token/月）**

### 3.5 Human-in-the-Loop（HITL）

**定义**：将人工审核纳入 AI 系统的闭环，关键决策点由人确认。

**BuildZotero HITL 设计**：
- 理论标签加 `new-` 前缀 → 用户核对入库
- 综述生成为"草案"→ 用户修改定稿
- 引用匹配提供候选列表 → 用户确认

**价值**：
- 承认 AI 不完美的同时仍能提供效率
- 把每次人工确认变成训练数据（如 `new-` 入库后成为新的理论库条目）
- 建立用户信任（他们知道自己仍有控制权）

---

## 四、LLM / RAG / Agent 技术栈

### 4.1 LLM 核心概念

| 概念 | 含义 |
|---|---|
| **Token** | 模型处理的最小单元（约等于 0.75 英文单词或 0.5 中文字符） |
| **Context Window** | 模型一次能处理的 Token 总量（如 GPT-4 是 128K，Gemini 2.5 Pro 是 2M） |
| **Temperature** | 输出随机性（0=确定，1=随机） |
| **Top-P / Top-K** | 采样策略，控制输出多样性 |
| **Embedding** | 把文本映射为向量（通常 768/1024/1536 维） |
| **Fine-tuning** | 用领域数据二次训练 |
| **RLHF** | 人类反馈强化学习（ChatGPT 核心技术） |
| **MoE** | Mixture of Experts（DeepSeek V3 架构） |

### 4.2 RAG（检索增强生成）

**RAG 定义**（AWS 官方）："RAG is the process of optimizing the output of a large language model, so it references an authoritative knowledge base outside of its training data sources before generating a response."

**标准 RAG 流程**：
```
用户提问
  ↓
Query 向量化 (Embedding)
  ↓
向量库相似度检索 (Top-K 匹配)
  ↓
检索结果 + 原始问题作为 Context
  ↓
LLM 生成回答
  ↓
（可选）引用标注 + 证据展示
```

**2026 年主流 RAG 变体**：

| 变体 | 核心思想 | 应用场景 |
|---|---|---|
| **Naive RAG** | 单轮检索 + 生成 | 基础问答 |
| **Advanced RAG** | 查询重写 + 多路检索 + 重排 | 复杂问答 |
| **Self-RAG** | LLM 自评估，决定要不要检索 | 动态判断 |
| **Agentic RAG** | 多 Agent 协作，多轮检索 | 复杂工作流 |
| **GraphRAG** | 知识图谱辅助检索 | 关系密集领域 |
| **Hybrid RAG** | 向量 + 关键词 + 图谱 | 企业知识库 |

**BuildZotero 的 RAG 设计特点**：
- **二层检索**：AI 结构化字段（精准） + 用户原标签（保留）
- **先治理再生产**：输入侧是结构化标签，不是裸全文——**本质是一种 Light Agentic RAG**
- **证据锚点**：`[number]` 标注 + 一键跳转
- **理论库循环**：新理论标记 → 人验 → 入库的持续更新

**RAG 核心挑战（通用）**：
1. **Chunking**：文档怎么切——太粗信息散，太细语义失
2. **Retrieval Quality**：Top-K 能不能召回真正相关的内容
3. **Context Window 约束**：检索结果 + 问题要能装进 LLM
4. **Hallucination**：即使给了 Context，LLM 仍可能瞎编

参考：[What is RAG? — AWS](https://aws.amazon.com/what-is/retrieval-augmented-generation/) / [What is Agentic RAG? — IBM](https://www.ibm.com/think/topics/agentic-rag) / [RAG Architecture: RAG Patterns for Enterprise AI — Calmops](https://calmops.com/architecture/rag-architecture-retrieval-augmented-generation/) / [Retrieval Augmented Generation (RAG) in Azure AI Search — Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

### 4.3 AI Agent 架构

**Agent 定义**：具备**感知 - 规划 - 记忆 - 行动**能力的 AI 系统，能自主完成多步骤任务。

**核心组件**：
```
Agent
├── 感知（Perception）：接收输入
├── 规划（Planning）：任务拆解
│   ├── Chain-of-Thought
│   ├── Tree-of-Thought
│   └── ReAct (Reason + Act)
├── 记忆（Memory）
│   ├── Short-term（单次会话）
│   └── Long-term（持久化）
├── 工具（Tools）
│   ├── API 调用
│   ├── 代码执行
│   └── 搜索/数据库
└── 反思（Reflection）：自我评估与修正
```

**2026 Agent 三代**：
- **第一代（单 Agent）**：ChatGPT 式单智能体
- **第二代（工具调用）**：Function Calling + RAG
- **第三代（多 Agent 协作）**：AutoGen / CrewAI / HiveMind 架构

**BuildZotero 的 Agent 设计**：
- 7 模块异步代理框架 = 7 个子 Agent
- 每个 Agent 有明确输入输出接口
- 记忆放在 Zotero 条目（持久化）
- 工具 = 多模型 API + Zotero API + Obsidian Link
- 属于**第 2.5 代**（有结构化记忆 + 工具调用，但未做 multi-agent 协作）

参考：[Agentic Retrieval-Augmented Generation: A Survey — arXiv 2501.09136](https://arxiv.org/abs/2501.09136) / [LLM Agents — Prompt Engineering Guide](https://www.promptingguide.ai/research/llm-agents)

### 4.4 多模型路由（Model Routing）

**动机**：同一系统里不同任务对模型能力要求不同；一刀切用最强模型成本不可控。

**路由策略**：

| 策略 | 原理 | 适用场景 |
|---|---|---|
| **按任务类型路由** | 如标签抽取用小模型，综述用大模型 | 任务结构明确时 |
| **按输入复杂度路由** | 小 Query 小模型，大 Query 大模型 | 输入差异大 |
| **按成本敏感度路由** | 用户选"经济/平衡/质量" | 有付费分层 |
| **按 Fallback** | 主模型失败降级备模型 | 高可用要求 |
| **按 ensemble** | 多模型投票/融合 | 质量敏感场景 |

**BuildZotero 实际路由表**（简化示意）：

| 任务 | 主模型 | 备用 | 降本幅度 |
|---|---|---|---|
| 标签抽取 | DeepSeek V3 | DeepSeek R1 | ~87% |
| 理论识别 | Gemini 2.5 Pro | Gemini 2.5 Flash | 0% |
| 综述生成 | Gemini 2.5 Pro | DeepSeek R1 | 0% |
| 简单对话 | DeepSeek V3 | Gemini 2.5 Flash | ~75% |
| 深度问答 | Gemini 2.5 Pro | DeepSeek R1 | 0% |

---

## 五、Prompt 工程

### 5.1 Prompt 三要素

一个好 Prompt 包含：
1. **角色（Role）**：让模型扮演什么身份（"你是学术文献分析师"）
2. **任务（Task）**：明确要做什么（"从下面文献抽取 8 维标签"）
3. **约束（Constraint）**：输出格式、长度、规范（"以 JSON 输出，每个字段 ≤50 字"）

### 5.2 常见 Prompt 技术

| 技术 | 核心思想 | 案例 |
|---|---|---|
| **Zero-Shot** | 直接问 | "什么是 RAG？" |
| **Few-Shot** | 给几个示例 | "像下面这样回答：<示例>" |
| **Chain-of-Thought (CoT)** | "let's think step by step" | 数学/推理题 |
| **Tree-of-Thought (ToT)** | 多分支推理 | 复杂规划 |
| **ReAct** | 推理 + 行动交替 | Agent 任务 |
| **Self-Consistency** | 多次采样投票 | 质量敏感 |
| **Self-Refine** | 生成 + 自我修改 | 长文本生成 |
| **Role-Playing** | 扮演角色 | 特定领域任务 |
| **Structured Output** | JSON/表格输出 | 需机器可解析 |

### 5.3 BuildZotero 的 Prompt 三层设计

1. **第一层：角色与规范注入**
   - 「你是学术文献分析助手，熟悉管理学/心理学/社会学的标准理论与方法论术语。」
   - 「请严格按照 8 维标签体系抽取信息。」

2. **第二层：任务与格式约束**
   - 「对以下论文提取：主题（Item）、方法（sMeth）、样本（Sample）、理论（Theory）、结论（Result）、变量（A1-DV/A2-IV/…）、条目细节（V1-V5）。」
   - 「输出格式：每维一行，格式为 `<维度>: <值>`。」

3. **第三层：质量控制**
   - 「理论标签必须优先与以下标准理论库匹配：<理论库>。」
   - 「若识别出库中没有的新理论，加 `Theory/new-` 前缀。」
   - 「所有结论必须带出自段落编号 `[<section>]`。」

### 5.4 Prompt 版本管理

对应 BuildZotero 的 **V / V-R / V-Rnote**：
- **V**：标准版 Prompt
- **V-R**：重处理版（处理 AI 输出错误时的修正 Prompt）
- **V-Rnote**：笔记优先版（从 Obsidian 笔记而非 PDF 读取时的变体）

**版本策略价值**：
- Prompt 变更可追溯，不是"一句话改到底"
- 不同场景用不同版本，不是一个 Prompt 打天下
- 上线新版 Prompt 时可快速回滚

### 5.5 Prompt 评估

好 Prompt 需要经过：
1. **评估集测试**：准备 30+ 条样本跑一遍
2. **对比 A/B**：新旧 Prompt 同一批样本对比
3. **人工评审**：盲评
4. **边界测试**：极端输入下会不会崩

参考：[Chain-of-Thought Prompting — Prompt Engineering Guide](https://www.promptingguide.ai/techniques/cot) / [Chain-of-Thought Prompting: A Guide for LLM Applications — Comet](https://www.comet.com/site/blog/chain-of-thought-prompting/) / [The Ultimate Prompt Engineering Guide for 2026 — Sariful Islam](https://sarifulislam.com/blog/prompt-engineering-2026/) / [Beyond the Text Box: The Future of LLM Prompt Engineering in 2026](https://blog.eif.am/llm-prompt-engineering)

---

## 六、AI 系统评估与 A/B

### 6.1 AI 系统评估分类

| 评估类型 | 方法 | 场景 |
|---|---|---|
| **离线评估（Offline）** | 评估集跑分、指标计算 | 上线前 |
| **在线评估（Online）** | A/B 测试、用户反馈 | 上线后 |
| **自动评估（Auto-eval）** | LLM-as-Judge | 大规模低成本 |
| **人工评估（Human-eval）** | 专家打分、盲评 | 高精度验证 |

### 6.2 常见指标

**通用 NLP 指标**：
- **BLEU**：机器翻译质量
- **ROUGE**：摘要召回率
- **BERTScore**：语义相似度
- **Perplexity**：语言模型流畅度

**任务特化指标**：
- **Precision / Recall / F1**：分类任务
- **MRR / NDCG**：检索排序
- **Exact Match / F1**：问答
- **Faithfulness / Relevance / Coherence**：生成质量

**BuildZotero 指标**：
- 标签抽取 Precision/Recall：80–90%
- 理论库匹配 Accuracy：>95%
- 引用可追溯率：100%
- 综述 Faithfulness：抽检

### 6.3 LLM-as-Judge

**原理**：用一个强模型（如 GPT-4）评估另一个模型的输出质量。

**优点**：大规模、低成本、可自动化
**缺点**：评估者本身有偏、可能漏掉细节错误

**BuildZotero 应用**：在多模型路由压测时，用 Gemini Pro 作为 Judge 评估 DeepSeek V3 的输出——省了人工评估时间。

### 6.4 A/B 测试（重复回顾）

参考 [[项目-BuildZotero-11商业数据分析岗面试问答#Q10｜如果让你为一个 1000 万 DAU 的产品做 A/B 测试，设计思路？|11 Q10]]。

AI 产品 A/B 特殊点：
- **Prompt A/B**：同一个模型，不同 Prompt
- **Model A/B**：同一个 Prompt，不同模型
- **Pipeline A/B**：不同 RAG 策略
- **UI A/B**：同一 AI 输出，不同展示方式

---

## 七、AI 商业化与竞争格局

### 7.1 AI 应用商业模式

| 模式 | 定义 | 案例 |
|---|---|---|
| **SaaS 订阅** | 月/年费 | ChatGPT Plus、Cursor |
| **按量付费** | Per-token / Per-call | OpenAI API、Claude API |
| **License + 部署** | 一次性 + 年维保 | Palantir、企业 Copilot |
| **广告变现** | 免费 + 广告 | Perplexity |
| **增值 Prosumer** | Freemium 分层 | Notion AI、Canva |
| **方法论咨询** | 卖服务 | 部分 AI 咨询公司 |
| **数据交易** | 卖数据/模型 | HuggingFace |

### 7.2 AI 应用护城河

**Moat 七问**：
1. **数据护城河**：我有独家数据吗？
2. **用户习惯护城河**：用户换我成本高吗？
3. **生态护城河**：我在什么生态里？迁移难度如何？
4. **规模护城河**：越用越便宜/越聪明吗？
5. **品牌护城河**：用户认我这个品牌吗？
6. **法规/合规护城河**：我有合规资质吗？
7. **技术护城河**：Schema/模型/工程是否独特？

**BuildZotero 护城河**：
- 数据（弱）：用户数据在用户本地
- 用户习惯（强）：集成进 Zotero 工作流，迁移成本高
- 生态（中）：绑定 Zotero + Obsidian
- 规模（弱）：单用户独立，无规模效应
- 品牌（弱）：小众领域
- 合规（中）：私有化部署符合学术数据安全要求
- 技术（强）：**8 维/11 维 Schema + Prompt 工程迭代 + 多模型路由**

**核心判断**：BuildZotero 的护城河是 **技术（Schema 深度）+ 用户习惯（生态集成）**。

### 7.3 竞品分析：AI 文献管理赛道

| 产品 | 形态 | 核心能力 | 差异 |
|---|---|---|---|
| **ChatPDF** | SaaS | 单 PDF 问答 | 点状会话，无沉淀 |
| **Kimi Chat** | SaaS（中国） | 长文档处理 | 通用工具，非学术 |
| **NotebookLM** | SaaS（Google） | 多源笔记本 | 无写回文库能力 |
| **Elicit** | SaaS | 文献搜索 + 抽取 | 在线，数据主权弱 |
| **Scite** | SaaS | 引用分析 | 无综述生成 |
| **Undermind** | SaaS | 深度文献搜索 | 无本地集成 |
| **Scholarcy** | SaaS | 文献摘要 | 无 Schema 化 |
| **BuildZotero** | 脚本 + 私有化 | 结构化 + 白盒 + 工作流 | **唯一写回文库的方案** |

### 7.4 投资判断框架

**四问法**：
1. **市场够大吗？**（TAM > 10 亿 人民币）
2. **护城河够深吗？**（Moat 七问命中 3+）
3. **团队够好吗？**（懂模型 + 懂行业 + 懂工作流 "三懂"）
4. **时机对吗？**（模型能力 vs 用户接受度曲线）

**BuildZotero 作为投资标的自评**：
- 市场：单垂直 1.5 亿人民币，跨垂直 10–100 亿（中）
- 护城河：Schema + 生态（强）
- 团队：单人 → 需组队（弱）
- 时机：AI 应用元年（强）

---

## 八、领域方法论的可迁移（跨行业复用）

BuildZotero 验证了一个**通用方法论**：

```
非结构化领域知识
      ↓ (Schema 设计)
结构化字段 + 标注规范
      ↓ (Prompt 工程 + 多模型路由)
自动化抽取
      ↓ (二层检索 + 多维筛选)
可检索资产
      ↓ (RAG + 证据锚点)
可追溯生成
      ↓ (工作流嵌入)
生产力提升
```

**可迁移行业清单**：

| 行业 | 类比"文献" | 类比"Schema" | 类比"工作流" |
|---|---|---|---|
| **法律** | 合同/判例 | 条款类型、适用法、涉及主体 | 尽调/合规审查 |
| **医学** | 临床指南/病历 | 疾病、症状、治疗、结果 | 诊疗路径 |
| **投研** | 研报/财报 | 财务指标、估值方法、驱动因子 | 投资决策 |
| **制药** | 临床试验报告/FDA 文件 | 试验设计、终点、样本 | 药物研发 |
| **咨询** | 案例库/方法库 | 行业、问题类型、方法、结果 | 项目交付 |
| **工程** | 技术文档/Bug 报告 | 模块、问题类型、根因、方案 | 知识管理 |
| **教育** | 教案/课程 | 知识点、难度、先验 | 教学设计 |

**核心判断**：**"Schema × Prompt × 工作流"三件套，是未来 5–10 年 AI 应用层的通用方法论**。BuildZotero 是这个方法论在科研垂直的证明。

---

## 九、关键术语速查（面试前扫描）

### AI 技术
- **LLM**：大语言模型
- **Token / Context Window / Temperature**：模型参数
- **Embedding**：文本向量化
- **Fine-tuning / RLHF**：模型训练方法
- **RAG / Agentic RAG / GraphRAG**：检索增强生成
- **Agent / Multi-Agent / ReAct**：智能体
- **CoT / ToT / Few-Shot**：提示技术
- **LLM-as-Judge**：大模型评估
- **MoE**：混合专家架构
- **Prompt Engineering / Context Engineering**：提示/上下文工程
- **Function Calling / Tool Use**：工具调用

### 产品
- **PRD / MRD / BRD**：需求文档三类
- **MVP / PMF**：最小可用产品 / 产品市场契合
- **NSM / DAU / MAU / Stickiness**：核心指标
- **CIRCLES / AARRR / HEART / STAR**：框架
- **ICE / RICE / Kano / MoSCoW**：优先级方法
- **Jobs-to-be-Done**：用户任务论
- **Aha Moment**：激活时刻
- **North Star Metric**：北极星指标

### 数据
- **OLAP / OLTP / ETL / ELT**：数据处理架构
- **Star Schema / Snowflake Schema**：维度建模
- **Cohort / Funnel / Retention**：分析方法
- **A/B Test / Power Analysis / p-value**：实验设计
- **Issue Tree / MECE / Pyramid Principle**：结构化分析

### 商业与战略
- **TAM / SAM / SOM**：市场规模三层
- **LTV / CAC / ARPU / ARR / MRR**：财务指标
- **Rule of 40**：SaaS 健康指标
- **Moat**：护城河
- **Porter's Five Forces / SWOT / 3C / 4P**：战略框架
- **Ansoff Matrix / BCG Matrix**：扩张/组合矩阵

---

## 十、面试前 1 小时扫描清单

面试开始前最后扫描——确保以下**15 个问题**都能 20 秒内答上：

**项目基础（5 个）**
- [ ] 1 句话 BuildZotero 定位
- [ ] 12 瓶颈 → 4 壁垒 → 7 模块的递进
- [ ] 8 维标签是哪 8 个
- [ ] 二层检索解决什么
- [ ] 多模型路由怎么分

**方法论（5 个）**
- [ ] CIRCLES 是 7 步
- [ ] AARRR 是 5 步
- [ ] HEART 是 5 维
- [ ] STAR-R 是 5 段
- [ ] MECE 是什么含义

**AI 技术（3 个）**
- [ ] RAG 是什么、Agentic RAG 是什么
- [ ] Prompt 三要素（角色/任务/约束）
- [ ] 防幻觉五层防御

**商业（2 个）**
- [ ] TAM/SAM/SOM 定义
- [ ] LTV/CAC 健康阈值

---

## 十一、外部参考（按主题归类）

### AI 应用（RAG / Agent / Prompt）

- [RAG in 2026: Bridging Knowledge and Generative AI — Squirro](https://squirro.com/squirro-blog/state-of-rag-genai)
- [Retrieval Augmented Generation (RAG) in Azure AI Search — Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)
- [What is Agentic RAG? — IBM](https://www.ibm.com/think/topics/agentic-rag)
- [Agentic Retrieval-Augmented Generation: A Survey — arXiv 2501.09136](https://arxiv.org/abs/2501.09136)
- [RAG Explained: Complete Retrieval Augmented Generation Guide — AI Agents Plus](https://www.ai-agentsplus.com/blog/rag-complete-guide-retrieval-augmented-generation-2026)
- [RAGFlow: Open-source RAG Engine — GitHub](https://github.com/infiniflow/ragflow)
- [RAG in 2026: How RAG Works for Enterprise AI — Techment](https://www.techment.com/blogs/rag-in-2026/)
- [RAG Architecture: Retrieval-Augmented Generation Patterns — Calmops](https://calmops.com/architecture/rag-architecture-retrieval-augmented-generation/)
- [Retrieval-Augmented Generation (RAG): Complete Guide 2026 — Data Expertise](https://www.dataexpertise.in/retrieval-augmented-generation-rag-guide/)
- [What is RAG? — AWS](https://aws.amazon.com/what-is/retrieval-augmented-generation/)
- [The Ultimate Prompt Engineering Guide for 2026 — Sariful Islam](https://sarifulislam.com/blog/prompt-engineering-2026/)
- [Beyond the Text Box: The Future of LLM Prompt Engineering in 2026 — EIF Blog](https://blog.eif.am/llm-prompt-engineering)
- [Agentic Engineering Roadmap: Skills, Tools & Resources 2026 — Towards Agentic AI](https://towardsagenticai.com/agentic-engineering-roadmap-skills-tools-resources-2026/)
- [Prompt engineering techniques: Top 6 for 2026 — K2view](https://www.k2view.com/blog/prompt-engineering-techniques/)
- [Chain-of-Thought Prompting — Prompt Engineering Guide](https://www.promptingguide.ai/techniques/cot)
- [Chain-of-Thought Prompting: A Guide for LLM Applications — Comet](https://www.comet.com/site/blog/chain-of-thought-prompting/)
- [LLM Agents — Prompt Engineering Guide](https://www.promptingguide.ai/research/llm-agents)

### 产品经理框架

- [Product Manager Interview Questions and Answers — Enhancv](https://enhancv.com/blog/product-manager-interview-questions-and-answers/)
- [Top 16 Product Management Frameworks — David Olszewski](https://davidolszewski.com/top-16-frameworks-every-product-manager-wants-to-know)
- [Ace Your Product Manager Interview: Behavioral & Case Study Prep — AceJobi](https://acejobi.com/interviews/product-manager-interview-prep-behavioral-case/)
- [25 Product Management Frameworks — SaaS Funnel Lab](https://www.saasfunnellab.com/essay/product-management-frameworks/)
- [What is the HEART framework — LogRocket](https://blog.logrocket.com/product-management/what-is-the-heart-framework/)
- [Product Managers' Guide for Selecting the Right Product Metrics Framework — Userpilot](https://userpilot.com/blog/product-metrics-framework/)
- [Product Manager Interview Questions & Answers — IgmGuru](https://www.igmguru.com/blog/product-manager-interview-questions)
- [10 Popular Frameworks Every Product Manager Should Know — Fueler](https://fueler.io/blog/10-popular-frameworks-every-product-manager-should-know-a-comprehensive-guide-with-use-cases-and-examples)
- [HEART & AARRR frameworks in Product Management — PM Playbook](https://simran-pm.medium.com/heart-aarrr-frameworks-in-product-management-7596b6c717de)
- [Product Metrics: AARRR vs HEART vs North Star — Hyperact](https://www.hyperact.co.uk/blog/product-metrics-frameworks)

### 数据分析

- [SQL Interview Questions for Data Analysts — InterviewQuery](https://www.interviewquery.com/p/sql-questions-data-analyst)
- [The SQL Metrics Interviewers Really Care About — LearnSQL](https://learnsql.com/blog/sql-metrics-real-interviews/)
- [Data Analyst Interview Questions & Prep Guide 2026 — Coursera](https://www.coursera.org/resources/data-analysis-interview-prep-guide)
- [50 A/B Testing Interview Questions & Answers — DataLemur](https://datalemur.com/blog/ab-testing-interview-questions-and-answers)
- [Ultimate SQL Interview Guide — DataLemur](https://datalemur.com/blog/sql-interview-guide)
- [Data Analyst Interview Questions: 75+ Q&As for 2026 — Simplilearn](https://www.simplilearn.com/tutorials/data-analytics-tutorial/data-analyst-interview-questions)
- [SQL Interview Questions You Must Prepare — StrataScratch](https://www.stratascratch.com/blog/sql-interview-questions-you-must-prepare-the-ultimate-guide/)
- [Data Analyst Interview Prep (2026 Guide) — Exponent](https://www.tryexponent.com/blog/data-analyst-interview-guide)

### 投行/咨询框架

- [Five Powerful Consulting Frameworks — StrategyU](https://strategyu.co/consulting-frameworks/)
- [Mastering Case Interview Frameworks (2025) — MECE Academy](https://mece.academy/resources/mastering-case-interview-frameworks-a-complete-guide-2025)
- [Case Interview Frameworks: The Ultimate Guide (2026) — Hacking the Case Interview](https://www.hackingthecaseinterview.com/pages/case-interview-frameworks)
- [Consulting Case Study 101: An Introduction to Frameworks — Street Of Walls](https://www.streetofwalls.com/finance-training-courses/consulting-case-study-training/consulting-case-study-101-frameworks/)
- [Common Case Interview Frameworks — IGotAnOffer](https://igotanoffer.com/blogs/mckinsey-case-interview-blog/118288068-case-interviews-frameworks-comprehensive-guide)
- [Case interview frameworks: the complete guide — StrategyU](https://strategyu.co/case-interview-frameworks/)
- [How to Use Porter's Five Forces — PrepLounge](https://www.preplounge.com/en/case-interview-basics/porters-five-forces)
- [Case Interview Frameworks & When to Use Them — Leland](https://www.joinleland.com/library/a/case-interview-frameworks)
- [Case Interview Frameworks Guide — CaseBasix](https://www.casebasix.com/pages/case-interview-frameworks-guide)

### 行为面（宝洁八大问）

- [宝洁八大问｜STAR 回答｜面试经验 — CSDN](https://blog.csdn.net/qq_43777616/article/details/141867149)
- [宝洁八大问：绝大多数面试都用得到的 8 道题 — 知乎](https://zhuanlan.zhihu.com/p/150474976)
- [如何回答「宝洁八大问」？— 知乎](https://www.zhihu.com/question/19889186)
- [2022 宝洁 FA 面试总结（八大问 + 情景模拟）— 知乎](https://zhuanlan.zhihu.com/p/445553205)
- [宝洁八大面试经典问题答案和解析 — HighMarkCareer](https://www.highmarkcareer.com/InterviewSkill/363.html)
- [掌握宝洁八大问，其实就是掌握了半个求职季 — LinkedIn](https://cn.linkedin.com/pulse/%E6%8E%8C%E6%8F%A1%E5%AE%9D%E6%B4%81%E5%85%AB%E5%A4%A7%E9%97%AE%E5%85%B6%E5%AE%9E%E5%B0%B1%E6%98%AF%E6%8E%8C%E6%8F%A1%E4%BA%86%E5%8D%8A%E4%B8%AA%E6%B1%82%E8%81%8C%E5%AD%A3-%E7%A5%8E%E5%90%8D-%E9%83%AD)

---

## 十二、关联笔记

- [[项目-BuildZotero-01简历撰写]]
- [[项目-BuildZotero-02-任务主线与业务面试问答]]
- [[项目-BuildZotero-06项目介绍梳理]]
- [[项目-BuildZotero-07产品介绍文档]]
- [[项目-BuildZotero-08简历项目介绍-一分钟与三分钟版]]
- [[项目-BuildZotero-09宝洁行为面八大问题]]
- [[项目-BuildZotero-10产品经理岗面试问答]]
- [[项目-BuildZotero-11商业数据分析岗面试问答]]
- [[项目-BuildZotero-12投行咨询研究岗面试问答]]

---

*本文件为 AI 应用与产品经理专业知识速查稿；技术细节请以权威文档为准，项目相关数字请以 [[项目-BuildZotero-02-任务主线与业务面试问答#数值、指标与材料口径速查|02 文数字速查]] 为准。*
