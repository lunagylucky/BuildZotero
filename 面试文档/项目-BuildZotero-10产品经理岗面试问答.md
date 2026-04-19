---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: 项目-BuildZotero-10产品经理岗面试问答
DateCreated: 2026-04-19 22:40
DateModified: 2026-04-19 22:40
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
- Topic/面试
- Topic/面试/产品经理
RelatedNote:
- '[[项目-BuildZotero-01简历撰写]]'
- '[[项目-BuildZotero-02-任务主线与业务面试问答]]'
- '[[项目-BuildZotero-06项目介绍梳理]]'
- '[[项目-BuildZotero-07产品介绍文档]]'
- '[[项目-BuildZotero-08简历项目介绍-一分钟与三分钟版]]'
- '[[项目-BuildZotero-09宝洁行为面八大问题]]'
- '[[项目-BuildZotero-11商业数据分析岗面试问答]]'
- '[[项目-BuildZotero-12投行咨询研究岗面试问答]]'
- '[[项目-BuildZotero-13专业知识文档-AI应用与产品经理]]'
RelatedProjects:
CardRecord: 产品经理岗专业面准备稿，覆盖需求/架构/优先级/指标/竞品/AI Agent 产品化等；可经受压力面追问。
---

**系列导航**（BuildZotero 面试材料）

| 上一页 | 下一页 |
| --- | --- |
| [[项目-BuildZotero-09宝洁行为面八大问题]] | [[项目-BuildZotero-11商业数据分析岗面试问答]] |

---

# 产品经理岗面试问答｜用 BuildZotero 作答

> **适用岗位**：互联网/SaaS 产品经理、AI 产品经理、AI Agent 产品经理、平台产品经理、B 端产品经理。
>
> **面试套路三层**：①项目讲述 → ②结构化追问（需求/设计/指标/权衡）→ ③开放题或假设题（假如你是 X 公司 PM）。
>
> **核心方法论**：**CIRCLES**（Comprehend / Identify / Report / Cut / List / Evaluate / Summarize）用于设计题；**AARRR**（Acquisition / Activation / Retention / Revenue / Referral）用于增长；**HEART**（Happiness / Engagement / Adoption / Retention / Task Success）用于体验；**STAR-R** 用于经历题。
>
> 参考：[Product Manager Interview Questions and Answers — Enhancv](https://enhancv.com/blog/product-manager-interview-questions-and-answers/) / [Top 16 Product Management Frameworks — David Olszewski](https://davidolszewski.com/top-16-frameworks-every-product-manager-wants-to-know) / [What is the HEART framework — LogRocket](https://blog.logrocket.com/product-management/what-is-the-heart-framework/) / [HEART & AARRR frameworks — PM Playbook](https://simran-pm.medium.com/heart-aarrr-frameworks-in-product-management-7596b6c717de)

---

## 一、面试官评估维度（先理解被看的是什么）

| 维度 | 被考察点 | BuildZotero 映射 |
|---|---|---|
| **用户同理心** | 能否从用户视角定义问题 | 自身痛点 → 归类访谈 → 12 瓶颈 |
| **问题结构化** | 能否把模糊问题拆清 | 12 瓶颈 → 4 壁垒 → 7 模块 |
| **产品直觉** | 能否判断"这事该不该做" | 选择集成而非独立产品 |
| **技术理解** | 能否和工程师"对齐颗粒" | JS + API 编排 + Prompt 三层 |
| **数据敏感** | 能否定义和读懂指标 | 8 维标签 + 11 维 Schema + 准确率指标 |
| **优先级判断** | 能否排 P0 vs P1 | 7 模块 P0–P6 排序 |
| **沟通表达** | 能否 2 分钟讲清楚 | [[项目-BuildZotero-08简历项目介绍-一分钟与三分钟版]] |
| **商业 sense** | 能否理解成本与商业化 | 多模型路由降本 73% |

---

## 二、高频问题：项目讲述与追问

### Q1｜2–3 分钟讲一下你做过最 proud 的项目

参考 [[项目-BuildZotero-08简历项目介绍-一分钟与三分钟版#二、三分钟版（≈ 750 字，口播 180 秒，STAR-R 展开）|3 分钟版]]。要点——定位一句话、12 瓶颈、7 模块、核心 3 件事、三个关键数字、一句反思。

### Q2｜你是怎么定义这个产品的需求的？

**STAR-R**：

**S**：2024 年底我读研时遇到科研文献处理"质量不可控、检索单一、难积累、工作流割裂"四重痛点；同期市面 Kimi、ChatPDF、NotebookLM、Elicit 等 AI 读 PDF 工具停留在"点状会话"。

**T**：判断这是**个人困扰**还是**群体刚需**；如果是刚需，怎么量化？

**A**：三层需求调研方法：
1. **自身体验复盘**：1 周写痛点日志，标记每次"为什么这次读论文效率低"。
2. **结构化访谈**：找 15 位来自清华、中科大、北大等 6 个课题组的研究生 + PI，1v1 访谈 30 分钟，用半结构化问卷（5 个维度、20 个问题）。
3. **竞品边界分析**：把 Kimi、ChatPDF、NotebookLM、Elicit、Scholarcy 等 6 个工具按"交互形态 / 检索能力 / 处理规模 / 可维护性 / 工作流嵌入度"5 个维度打分，识别共同盲区。
4. **优先级矩阵**：把 30+ 条抱怨归类到 **6 个维度、12 类瓶颈**，再按"用户频次 × 痛感强度 × 技术可行性"三轴排序。
5. **抽象壁垒**：归纳为 **4 大技术壁垒**——上下文长度、幻觉、输出不稳定、工作流断裂。

**R**：形成 PRD 一稿；4 大壁垒直接对应 7 模块的设计（每模块解决 1–2 个壁垒）。

**Reflection**：后悔没有更早做**可量化的用户实验**（如让用户对同一篇论文用新旧工作流各跑一次计时）。

### Q3｜为什么这么拆模块？为什么是 7 个而不是 3 个或 15 个？

**考点**：模块划分的**可论证性**（任务驱动 vs 技术驱动）。

**答**：按**用户真实工作流阶段**切分，不按技术栈切。

- **P0 基础操作**：标签展示/清理/删除——数据卫生层
- **P1 文献标签解构**：7 维标签提取——**先治理**
- **P2 文献标签统计**：条目/标签/自定义统计——治理后看全貌
- **P3 文献自定义筛选**：多维逻辑检索——找得到
- **P4 文献综述**：矩阵生成/要点综述——**再生产**
- **P5 文献引用**：自动引用匹配——产出链路
- **P6 交互系统**：8 种问答 + 文件上传——长尾交互

「**先治理、后生产**」是顺序依赖，不是口号。如果合并到 3 个模块，会丢掉"是否已治理"这个关键状态位；如果拆到 15 个，会把"同一链路节点"切散，增加模块间依赖。

### Q4｜你用了哪些框架？

**答**：我不是先背框架再套项目的，但事后能映射到几个经典框架上：

| 我的做法 | 对应框架 | 说明 |
|---|---|---|
| 自身痛点→访谈→归类 | **Design Thinking**（共情→定义→构思→原型→测试） | 前半段 |
| 12 瓶颈→4 壁垒 | **MECE** | 归纳与降维 |
| 多模型路由 | **Value Engineering** | 按功能价值分配成本 |
| AARRR | **Pirate Metrics** | 种子用户→口碑传播→私有化→培训 的推广漏斗 |
| 8 维 + 11 维 Schema | **ER Modeling + Taxonomy Design** | 数据建模方法论 |
| P0–P6 模块顺序 | **价值阶梯 / Now-Next-Later** | 价值闭环先行 |

### Q5｜如果只给你 1 个季度，你会先做哪个模块？

**考点**：优先级判断；是否理解 MVP 逻辑。

**答**：**先做 P1（文献标签解构）+ P3（自定义筛选）**，不做综述与引用。理由：
- **价值闭环**：用户把 100 篇文献扔进去、10 分钟后能用"变量 + 方法 + 样本"检索到——这就是 MVP，足以让用户愿意留下来。
- **杠杆点**：P1 是所有后续模块的**输入**——没有 P1 产生的结构化标签，P2/P4/P5 都是空中楼阁。
- **成本可控**：P1+P3 不依赖多模型路由的成熟度，单模型就能跑。
- **学习闭环**：1 个季度内能看到真实用户的标签错误分布，反向优化 Prompt。

P4/P5（综述/引用）放 Q2 做；P2（统计）和 P6（交互）放 Q3 做。

### Q6｜你的优先级是怎么排的？

**答**：多轴排序：
1. **用户价值**：解决了哪个壁垒？这个壁垒影响多少用户？
2. **依赖关系**：是不是别的模块的前置？（P1 是前置 → 最高优先）
3. **不确定性**：技术风险高还是低？高风险的先做 spike。
4. **沉没成本**：已经投入的模块不要轻易放弃。

对应到 P0–P6：
- **P0/P1 = P0 级**（必须做，做不好别的都白搭）
- **P2/P3 = P1 级**（让 P1 产生的标签变成用户价值）
- **P4/P5 = P2 级**（扩大使用场景）
- **P6 = P3 级**（长尾但高频）

### Q7｜你的关键指标是什么？怎么量化"成功"？

**答**：分三层指标体系：

**北极星指标**：**每月完成综述的课题组数量**——既反映产品价值，也反映粘性。

**一级指标**（HEART 框架对应）：
- **Happiness**：课题组 NPS（目前是课题组面谈反馈，后续可标准化调研）
- **Engagement**：单用户月活天数、单次使用时长
- **Adoption**：新课题组首次部署→3 周内至少一次综述产出 的比率
- **Retention**：3 个月后仍在使用的课题组比率
- **Task Success**：
  - 标签抽取准确率 80–90%
  - 理论库匹配 >95%
  - 引用来源可追溯率 100%
  - 单篇处理耗时 ≤1 分钟
  - 综述周期 ≤3 天

**二级指标（健康度）**：
- API 成本 / 课题组 / 月
- 错误标签人工修正率（越低越好）
- 功能使用分布（哪些模块冷门）

### Q8｜面对 AI 幻觉问题，你作为 PM 怎么做产品决策？

**考点**：AI 产品经理的核心能力——**在模型不完美时如何设计产品**。

**答**：**幻觉不是要"消灭"，是要"设计成可管理的风险"**。

| 层级 | 策略 | BuildZotero 实例 |
|---|---|---|
| **预防** | 控制输入质量（先治理） | P1 结构化标签做综述输入，不是裸全文 |
| **检测** | 让幻觉暴露出来 | 理论库对比 + `Theory/new-` 前缀 |
| **兜底** | 输出可核对 | `[number]` 锚点 + 一键跳转 |
| **修复** | 用户可改 | 白盒标签，Zotero 条目可手改 |
| **沟通** | 设置用户预期 | 文档与培训里明说"AI 是草案生成器" |

这五层在业内叫**防幻觉五层防御**，不只靠模型本身。

### Q9｜你的产品是 B 端还是 C 端？怎么定位？

**答**：**介于 B 和 C 之间**，更准确说是 **Prosumer（专业消费者）**。

- **C 端特征**：单个研究者直接使用；自主安装、自主决定。
- **B 端特征**：以课题组为采纳单位；涉及私有化部署、培训；有组织内口碑传播。
- **定位选择**：走 **Prosumer + 自下而上渗透**——先打动个体研究者，再通过他们渗透到课题组、再到学校。

### Q10｜你的商业模式是什么？

**答**：诚实作答——当前处于**非商业化阶段**，主要交付价值是**方法论 + 开源脚本 + 私有化部署服务**。

**潜在路径**：
1. **私有化部署 + 年费**：针对实验室级客户（清华、中科大等规模）按年收取。
2. **Pro 版插件**：对高频用户做增值版（更高配额、更强推理模型）。
3. **方法论咨询**：帮助企业/机构把他们的知识库按同样思路做结构化。
4. **B 端知识工程方案**：迁移到法律、投研、制药等垂直领域。

**我对商业化的判断**：BuildZotero 的**方法论**（结构化 + 白盒 + 工作流嵌入）比**工具本身**更值钱——这个方法论可以跨垂直复用。

---

## 三、开放题与假设题

### Q11｜假如你是 Notion AI 的 PM，你会怎么做产品？

**CIRCLES 方法**：

- **C（Comprehend Situation）**：Notion 是笔记 + 文档 + 数据库一体的生产力工具；Notion AI 当前是"写作助手"。
- **I（Identify Customer）**：个人用户（写作、笔记）、团队（项目管理、文档库）、企业（内部 Wiki）。
- **R（Report Needs）**：个人——写得更快；团队——找得到知识；企业——知识资产沉淀。
- **C（Cut Priority）**：**企业知识沉淀**是最大机会点——Notion 的 C 端粘性已到瓶颈，B 端企业知识库是增量。
- **L（List Solutions）**：
  1. **企业知识图谱**：基于页面关系自动建图，回答"XX 项目最新进展是什么"
  2. **自动摘要与每日简报**：按订阅推送部门/项目动态
  3. **智能搜索**：语义检索 + 证据溯源（对标 Glean）
  4. **AI 编辑官**：自动标准化内部文档格式
- **E（Evaluate）**：企业知识图谱价值最高但技术门槛最高；智能搜索价值次之但交付最快——**MVP 选智能搜索**。
- **S（Summarize）**：Quarter 1 智能搜索 + 证据溯源；Quarter 2 每日简报；Quarter 3 企业知识图谱。

**可迁移**：我在 BuildZotero 里做的「结构化 Schema + 二层检索 + 证据溯源」是这套方案的原型。

### Q12｜假如 ChatGPT 推出了文献管理功能，你会怎么应对？

**答**：这是 AI PM 经典场景。我的思考——**不应对，而是加固护城河**。

- **分析威胁**：ChatGPT 做文献管理有三个软肋——①不懂 Zotero 已有的数据沉淀；②幻觉更严重（无学术标准库）；③无法私有化部署。
- **加固策略**：
  1. **垂直深化**：把 8 维/11 维做得比通用工具深 10 倍。
  2. **生态绑定**：在 Zotero + Obsidian + Cursor 上建立自动化链路——ChatGPT 做不到这种集成。
  3. **数据主权**：强调本地优先 + 私有化——学术用户对未发表论文极度敏感。
  4. **社区资产**：种子用户课题组成为口碑壁垒。

**不做的**：不硬抗对话体验——ChatGPT 的对话引擎不是我能追的。

### Q13｜你怎么看 AI Agent 这波趋势？

**答**：我把 AI Agent 分成三代——

- **第一代（ChatGPT 式）**：单次对话 + 无记忆；强感知、弱规划。
- **第二代（工具调用式）**：Function Calling、ReAct、RAG；有工具但链路断、状态易丢。
- **第三代（架构化式）**：多 Agent 协作、有持久记忆、有工作流——这是 2026 趋势。

BuildZotero 属于第 2.5 代——有结构化记忆（Zotero 条目），有工具调用（多模型路由），但尚未做到 multi-agent 协作。

**PM 视角的判断**：
1. AI Agent 的壁垒不再是模型，而是**上下文工程**（Context Engineering）和**数据治理**。
2. 谁能把"领域知识 + 工作流 + 用户习惯"建模好，谁就赢。
3. Agent 应用 PMF 的核心是**"AI 输出能沉淀成资产"**而不是**"AI 输出能让人惊叹"**。

参考：[The Ultimate Prompt Engineering Guide for 2026 — Sariful Islam](https://sarifulislam.com/blog/prompt-engineering-2026/) / [Agentic Retrieval-Augmented Generation: A Survey — arXiv](https://arxiv.org/abs/2501.09136)

### Q14｜如果 BuildZotero 的用户数要从 100 涨到 10000，你会怎么做？

**AARRR** 分析：

| 阶段 | 当前 | 目标做法 |
|---|---|---|
| **Acquisition** | 课题组口碑 + 小红书 demo | 开源发布 + 学术会议 demo + B 站/YouTube 视频 + Reddit r/academia 露出 |
| **Activation** | 私有化部署 + 培训 | 一键安装包 + 开箱即用示例库 + 15 分钟上手视频 |
| **Retention** | 课题组面谈反馈 | 使用数据埋点 + 每月邮件摘要 + 社区论坛 |
| **Revenue** | 非商业化 | 免费核心功能 + Pro 版（高配额/高级模型） |
| **Referral** | 同实验室传播 | 内建邀请机制 + 课题组案例白皮书 |

**关键风险**：用户从"课题组"变成"个体"时，**培训环节会缺失**——需要把培训做成自助化内容。

### Q15｜你怎么做 A/B 测试？

**答**：BuildZotero 当前规模不适合严格 A/B（样本太小），但设计了**准实验**：
- 对同一课题组，2 周用旧版 Prompt，2 周用新版 Prompt。
- 记录标签抽取准确率（抽检 50 条）、用户修改次数、单次会话时长。
- 由课题组博士生盲评（不告诉他们哪周用了哪版）。

如果是大规模产品（10k+ 用户），会做：
1. **randomization**：用户 hash 到 A/B 桶。
2. **样本量**：用 power analysis 预估 n（通常 min 1000/arm）。
3. **guardrail 指标**：除核心指标外，必须监控"Token 成本/用户"不能涨过 20%。
4. **分段分析**：按用户阶段（新/老）分段看结果。

### Q16｜给我讲一个你做错了的产品决策

**答**：**早期我把"做独立 Web App"当作备选方向考虑了太久**，耽误了约 1 个月。

**S**：早期我在"独立 Web App" vs "Zotero 集成"之间摇摆，团队（其实就是我自己 + 导师）对独立 App 的品牌感受更好。

**T**：定一个方向并推进。

**A（错误）**：我花了 3 周做了一个 Web 原型，拿给目标用户看。

**R（结果）**：用户反馈"好看但不会用"——他们的文献都在 Zotero 里，切换成本太高。我才下定决心走集成路线。

**Reflection**：**"产品经理最容易犯的错误之一，是把'自己觉得酷'当作'用户会买单'"**。应该一开始就做 5 分钟访谈而不是 3 周 demo。

---

## 四、AI 产品经理专项（高频压力题）

### Q17｜怎么评估一个 AI 功能该不该上线？

**答**：**四象限评估法**：

|  | 准确率可接受 | 准确率差 |
|---|---|---|
| **错误成本低** | ✅ 上线，快速迭代 | ⚠️ 可上线但需"AI 辅助"标签 |
| **错误成本高** | ⚠️ 加人工核对层 | ❌ 不上线 |

BuildZotero 的例子：
- **标签抽取（错误成本低，准确率 80–90%）**：上线，允许用户手改。
- **理论识别（错误成本高，准确率不稳定）**：上线但加 `Theory/new-` 前缀标记，强制人工核对。
- **自动引用生成（错误成本高，但锚点可追溯）**：上线，强制带锚点。
- **自动综述（错误成本中高）**：上线但明说"草案"，不直接用。

### Q18｜怎么设计 AI 产品的容错与降级机制？

**答**：**三层容错**：

1. **模型层**：路由——主模型失败自动降级到备用模型；超时重试。
2. **流程层**：批处理失败只回滚该条，不影响已处理条目；关键写入前落可校验中间态。
3. **用户层**：最坏情况允许退到"人工修订 + 白盒字段"，不把半成功结果 silent 当成可信标签。

### Q19｜你怎么和工程师对齐"AI 产品"的技术方案？

**答**：AI 产品 PM 和传统 PM 最大区别——**需要懂 Prompt/模型/上下文工程的边界**。

我的做法：
1. **产出"AI-PRD"**：除常规 PRD 外，列出**预期输入输出的结构化 Schema**、**评估集（至少 30 条样本）**、**可接受错误率**。
2. **对齐模型选型**：和工程师一起讨论 DeepSeek V3 vs Gemini Pro 的 tradeoff——不是甩给工程师。
3. **设计评估流程**：上线前跑一批评估，不合格就回炉。
4. **共同写 Prompt**：Prompt 属于 PM 和工程师共同的产出物，不是工程师单方面。

### Q20｜如果要把 BuildZotero 卖给大厂，你的 pitch 是什么？

**答**：

> BuildZotero 证明了**"AI 只要不做知识工程就永远停留在玩具层"**。我们把非结构化文献变成可检索、可追溯、可复用的资产，这套方法论可以在任何非标知识密集领域复用——法律尽调、医学文献、金融研报、企业案例库。核心资产不是代码（33 个脚本 119 版本），而是我们沉淀下来的**"领域 Schema + Prompt 工程 + 工作流嵌入"三件套**。我们愿意在任何"知识就是资产"的行业做 POC。

---

## 五、压力面场景模拟

### 场景 A：面试官挑战"你这个不就是加了 AI 的 Zotero 插件？"

**答**：「表面看像，本质不一样。Zotero 插件解决的是'文献管理'，BuildZotero 解决的是'知识生产'——把文献库变成可推理的**结构化知识库**。打个比方——同样是文字处理，Word 和 Notion 是不同物种。Zotero 插件是文件夹，BuildZotero 是知识图谱。」

### 场景 B：面试官追问"为什么你一个人能做完？是不是项目不够大？"

**答**：「问题好。体量上，33 个功能、119 版本、15 份产品文档、2 所高校部署——这不是玩具规模。能一个人完成的原因——①项目不做独立基础设施，在 Zotero 生态上做增强；②核心复杂度在 Prompt 与 Schema 设计，不在工程量；③迭代用户少而聚焦（课题组级），避免了大规模用户的协调成本。**单人能做完恰好证明了产品架构的优雅**，不是规模小。」

### 场景 C：面试官追问"70% 提速怎么算的"

**答**：「相对优化前同一条路径的端到端耗时估计。拆解——模型路由把简单任务甩到便宜模型省 30%；批处理减少重复解析 PDF 省 25%；缓存避免重复扣费省 15%。这是设计目标与自测口径，不是第三方审计数字。」

### 场景 D：面试官问"你对 PM 这个岗位怎么理解？"

**答**：「PM 的三个职责——**问题定义官**（Problem Definer）、**取舍决策官**（Trade-off Maker）、**组织翻译官**（Translator）。BuildZotero 让我完整练了前两个，第三个在课题组部署时也练过——把学术语言翻译成工程语言、把工程语言翻译成用户语言。我认为 PM 最难的是前两者——工具谁都能学，判断力不能速成。」

---

## 六、必背高频术语

| 术语 | 含义 | BuildZotero 关联 |
|---|---|---|
| **North Star Metric** | 北极星指标 | 每月完成综述的课题组数 |
| **PMF** | Product-Market Fit | 清华、中科大等采纳 |
| **MVP** | 最小可用产品 | P1+P3 |
| **AARRR** | 增长漏斗 | 推广路径 |
| **HEART** | Google 体验指标 | 产品指标体系 |
| **CIRCLES** | 产品设计题框架 | Notion AI 假设题 |
| **MECE** | 不重不漏 | 12 瓶颈 → 4 壁垒 |
| **GIGO** | 垃圾进垃圾出 | 先治理后生产 |
| **RACI** | 责任矩阵 | 课题组部署时的分工 |
| **SLA** | 服务等级 | 非商业化，不虚报 |
| **ICE Score** | 优先级打分（Impact/Confidence/Ease） | 功能排序 |
| **RICE Score** | 优先级打分（Reach/Impact/Confidence/Effort） | 进阶版 |
| **Kano Model** | 需求分类（必备/期望/兴奋） | 白盒可改=期望型 |
| **Jobs-to-be-Done** | 用户雇佣产品完成的任务 | 研究者"雇"BuildZotero 做综述 |

---

## 七、关联笔记

- [[项目-BuildZotero-01简历撰写]]
- [[项目-BuildZotero-02-任务主线与业务面试问答]]
- [[项目-BuildZotero-06项目介绍梳理]]
- [[项目-BuildZotero-07产品介绍文档]]
- [[项目-BuildZotero-08简历项目介绍-一分钟与三分钟版]]
- [[项目-BuildZotero-09宝洁行为面八大问题]]
- [[项目-BuildZotero-11商业数据分析岗面试问答]]
- [[项目-BuildZotero-12投行咨询研究岗面试问答]]
- [[项目-BuildZotero-13专业知识文档-AI应用与产品经理]]

### 外部参考

- [Product Manager Interview Questions and Answers — Enhancv](https://enhancv.com/blog/product-manager-interview-questions-and-answers/)
- [Top 16 Product Management Frameworks — David Olszewski](https://davidolszewski.com/top-16-frameworks-every-product-manager-wants-to-know)
- [Ace Your Product Manager Interview: Behavioral & Case Study Prep — AceJobi](https://acejobi.com/interviews/product-manager-interview-prep-behavioral-case/)
- [25 Product Management Frameworks — SaaS Funnel Lab](https://www.saasfunnellab.com/essay/product-management-frameworks/)
- [What is the HEART framework — LogRocket Blog](https://blog.logrocket.com/product-management/what-is-the-heart-framework/)
- [HEART & AARRR frameworks — PM Playbook on Medium](https://simran-pm.medium.com/heart-aarrr-frameworks-in-product-management-7596b6c717de)
- [Product Metrics: AARRR vs HEART vs North Star — Hyperact](https://www.hyperact.co.uk/blog/product-metrics-frameworks)
- [The Ultimate Prompt Engineering Guide for 2026 — Sariful Islam](https://sarifulislam.com/blog/prompt-engineering-2026/)
- [Agentic Retrieval-Augmented Generation: A Survey — arXiv](https://arxiv.org/abs/2501.09136)

---

*本文件为产品经理岗面试准备稿；对外前请与 01/02/06 原句逐条核对，不新增事实。*
