---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: BuildZotero-功能清单
DateCreated: 2026-01-17 17:37
DateModified: 2026-04-18 17:38
Type:
- publish
Status:
- doing
Version:
- v1.0
CardStatus: true
CardType:
- card-project
tags:
- Topic/工具技能/工作笔记
- Pattern/Memo
RelatedNote:
RelatedProjects:
CardRecord: 围绕 BuildZotero 下的 BuildZotero-功能清单 沉淀可复用经验，支持检索、复盘与迭代。
---

> [!summary] 项目概览｜BuildZotero 功能清单
> 7 个核心模块、52 个功能点，覆盖文献管理全流程；多维标注体系驱动的 Prompt 工程架构。

# <span style="color:#3B8650;">功能总览</span>

> [!info] 功能总览
> BuildZotero 共包含 <span style="color:#048562; font-weight:bold;">7 个核心模块</span>，<span style="color:#048562; font-weight:bold;">52 个功能点</span>，覆盖文献管理的全流程。<span style="color:#048562; font-weight:bold;">核心架构</span>：多维标注体系驱动的 Prompt 工程架构，通过 8 维标签体系（标准化标注规范）实现智能化的文献内容提取。

| 模块 | 功能数 | 状态 | 优先级 |
|------|--------|------|--------|
| P0- 基础操作 | 4 | ✅ 已完成 | P1 |
| P1- 文献标签解构 | 21 | ✅ 已完成 | P0 |
| P2- 文献标签统计 | 2 | ✅ 已完成 | P1 |
| P3- 文献自定义筛选 | 4 | ✅ 已完成 | P1 |
| P4- 文献综述 | 7 | ✅ 已完成 | P0 |
| P5- 文献引用 | 3 | ✅ 已完成 | P0 |
| P6- 交互系统 | 11 | ✅ 已完成 | P1 |
| <span style="color:#048562; font-weight:bold;">总计</span> | <span style="color:#048562; font-weight:bold;">52</span> | <span style="color:#048562; font-weight:bold;">✅ 已完成</span> | - |

---


## <span style="color:#43973D;">P0 - 基础操作模块</span>

### <span style="color:#7CA531;">M1- 标签展示</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P0-M1
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V2)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 自定义标签属性列的需要展示的标签，可根据当前的需求自定义，比如，当前学者正在做方法论上的研究，那么就可以只显示方法标签，如果正在做专题研究，则可以显示主题、变量等标签，所有的标签高度自定义，按照正则表达式，随意组合和展示
- <span style="color:#048562; font-weight:bold;">输入</span>: 文献库的所有文献
- <span style="color:#048562; font-weight:bold;">输出</span>: 标签列表
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 快速检查文献标签状态，根据研究需求自定义展示


### <span style="color:#7CA531;">M2- 标签清理</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P0-M2
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V4)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 智能清理重复或无效标签，保留系统标签
- <span style="color:#048562; font-weight:bold;">输入</span>: 选中文献
- <span style="color:#048562; font-weight:bold;">输出</span>: 清理后的标签列表
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 数据清洗、标签规范化


### <span style="color:#7CA531;">M3- 标签删除</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P0-M3
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V4)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 批量删除指定前缀的标签
- <span style="color:#048562; font-weight:bold;">输入</span>: 选中文献 + 标签前缀
- <span style="color:#048562; font-weight:bold;">输出</span>: 删除结果统计
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 批量标签管理


### <span style="color:#7CA531;">M4- 标题清理</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P0-M4
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V1)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 规范化文献标题格式
- <span style="color:#048562; font-weight:bold;">输入</span>: 选中文献
- <span style="color:#048562; font-weight:bold;">输出</span>: 清理后的标题
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 标题标准化

---


## <span style="color:#43973D;">P1 - 文献标签解构模块（核心 - 多维标注体系）</span>
<span style="color:#048562; font-weight:bold;">核心目标</span>：利用 Prompt 工程将 PDF 全文强制映射为一套标准化的学术标注规范（逻辑结构协议）

<span style="color:#048562; font-weight:bold;">多维标注体系与标准化治理</span>:
- <span style="color:#048562; font-weight:bold;">分析规范设计</span>：独立设计一套包含 11 个核心维度的学术分析协议（Standardized Protocol），涵盖核心变量（IV/DV）、理论架构、研究方法与样本特征等
- <span style="color:#048562; font-weight:bold;">数据结构化映射</span>：利用提示词工程驱动 LLM 将长文本核心逻辑映射为 Zotero 的标准化标签体系，实现了从"非结构化文献"向"标准化知识组件"的物理级转换

<span style="color:#048562; font-weight:bold;">资产维护特性</span>：
- ✅ 生成的标签直接写入 Zotero 数据库
- ✅ <span style="color:#048562; font-weight:bold;">白盒化维护</span>：用户可在阅读过程中随时手动修改标签，确保数据库的 100% 准确性
- ✅ 支持增量更新，避免重复处理

<span style="color:#048562; font-weight:bold;">理论库构建与循环更新机制</span>:
- ✅ <span style="color:#048562; font-weight:bold;">标准理论库匹配</span>：理论标签基于标准理论库进行标准匹配
- ✅ **新理论标记**：AI 从文献中总结出的理论标签将会加上 `new` 标记
- ✅ <span style="color:#048562; font-weight:bold;">用户核对</span>：用户可以去根据文献内容去核对
- ✅ <span style="color:#048562; font-weight:bold;">理论库更新</span>：如果这是一项标准理论库没有的，那么则可以纳入标准理论库
- ✅ <span style="color:#048562; font-weight:bold;">循环更新</span>：循环下去，自己的标准理论库就会一直更新与完善
- ✅ <span style="color:#048562; font-weight:bold;">标签正确性</span>：确保了所有理论标签的正确性，解决了理论查找困难的问题
- ✅ <span style="color:#048562; font-weight:bold;">知识资产</span>：这些理论库是自己的知识资产

### <span style="color:#7CA531;">Tag1- 主题标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag1
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V5, V5-R, V5-Rnote
- <span style="color:#048562; font-weight:bold;">描述</span>: 提取研究主题与核心概念
- **输出格式**: `Item/序号`
- <span style="color:#048562; font-weight:bold;">AI 能力</span>: 基于 PDF/摘要/笔记提取主题
- <span style="color:#048562; font-weight:bold;">标注规范设计</span>: 关键变量、核心概念
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 主题聚类、研究方向识别


### <span style="color:#7CA531;">Tag3- 方法标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag3
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V6, V6-R, V6-Rnote
- <span style="color:#048562; font-weight:bold;">描述</span>: 识别研究方法与技术
- **输出格式**: `sMeth/1-6`
- <span style="color:#048562; font-weight:bold;">分类</span>: 定性、定量、综述、方法
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 方法比较、研究设计分析


### <span style="color:#7CA531;">Tag4- 样本标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag4
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V4, V4-R, V4-Rnote
- <span style="color:#048562; font-weight:bold;">描述</span>: 提取研究层次与样本特征
- **输出格式**: `Sample/1-3`
- <span style="color:#048562; font-weight:bold;">分类</span>: 宏观、中观、微观
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 样本特征统计、研究范围分析


### <span style="color:#7CA531;">Tag5- 理论标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag5
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V6, V6-R, V6-Rnote
- <span style="color:#048562; font-weight:bold;">描述</span>: 提取理论框架与机制
- **输出格式**: `Theory/序号`（标准理论库匹配）或 `Theory/new-序号`（新理论标记）
- <span style="color:#048562; font-weight:bold;">特点</span>: 
  - <span style="color:#048562; font-weight:bold;">标准理论库匹配</span>：理论标签基于标准理论库进行标准匹配
  - **新理论标记**：AI 从文献中总结出的理论标签将会加上 `new` 标记
  - <span style="color:#048562; font-weight:bold;">用户核对</span>：用户可以去根据文献内容去核对
  - <span style="color:#048562; font-weight:bold;">理论库更新</span>：如果这是一项标准理论库没有的，那么则可以纳入标准理论库
  - <span style="color:#048562; font-weight:bold;">循环更新</span>：理论库持续更新与完善，确保所有理论标签的正确性
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 理论梳理、机制分析、理论库构建


### <span style="color:#7CA531;">Tag6- 结论标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag6
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V2, V2-R
- <span style="color:#048562; font-weight:bold;">描述</span>: 提取研究发现与结论
- **输出格式**: `Result/序号`
- <span style="color:#048562; font-weight:bold;">分类</span>: 主要、次要、其他
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 结果对比、发现汇总


### <span style="color:#7CA531;">Tag7- 变量标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag7
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V2, V2-R, V2-Rnote
- <span style="color:#048562; font-weight:bold;">描述</span>: 提取变量定义与关系
- <span style="color:#048562; font-weight:bold;">输出格式</span>: 
  - `A1-DV/` (因变量)
  - `A2-IV/` (自变量)
  - `A3-MO/` (调节变量)
  - `A4-ME/` (中介变量)
  - `A5-INV/` (工具变量)
  - `A6-CV/` (控制变量)
- <span style="color:#048562; font-weight:bold;">标注规范设计</span>: <span style="color:#048562; font-weight:bold;">因果链条</span> - 自动识别并标注变量关系
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 变量关系分析、模型构建


### <span style="color:#7CA531;">Tag8- 条目细节标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P1-Tag8
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V2, V2-R, V2-Rnote
- <span style="color:#048562; font-weight:bold;">描述</span>: 提取研究贡献与展望
- <span style="color:#048562; font-weight:bold;">输出格式</span>:
  - `V1-def/` (变量定义)
  - `V2-mea/` (测量方法)
  - `V3-con/` (研究贡献)
  - `V4-fut/` (研究展望)
  - `V5-sor/` (样本数据来源)
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 研究贡献分析、未来方向识别

---


## <span style="color:#43973D;">P2 - 文献标签统计模块（全景情报看板与统计透视）</span>
<span style="color:#048562; font-weight:bold;">全景情报看板与统计透视</span>:
- <span style="color:#048562; font-weight:bold;">全局态势感知（Situational Awareness）</span>：开发 P2 统计模块，打破了"单点检索"的局限。通过将文献库情报表格化、看板化呈现，实现了从"条目逻辑"到"标签逻辑"的全量透视
- <span style="color:#048562; font-weight:bold;">动态回溯与穿透</span>：支持对研究议题分布、方法论占比等核心指标进行统计。所有统计结果均支持秒级回溯（Traceability），确保研究者在掌握宏观趋势的同时，能瞬间下钻定位至具体的文献来源

### <span style="color:#7CA531;">Table1- 统计条目</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P2-Table1
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V4)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 生成文献条目统计表格
- <span style="color:#048562; font-weight:bold;">输出</span>: Markdown 表格
- <span style="color:#048562; font-weight:bold;">统计维度</span>: 年份、期刊、作者、标签
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 研究领域概览、全局态势感知


### <span style="color:#7CA531;">Table2- 统计标签</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P2-Table2
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V6)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 生成标签使用频率统计
- <span style="color:#048562; font-weight:bold;">输出</span>: Markdown 表格
- <span style="color:#048562; font-weight:bold;">统计维度</span>: 标签内容、次数、年份、文献编号
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 标签使用趋势分析、动态回溯与穿透

---


## <span style="color:#43973D;">P3 - 文献自定义筛选模块（多维逻辑检索）</span>
<span style="color:#048562; font-weight:bold;">核心目标</span>：利用打标后的结构化数据，实现"逻辑组合寻找文献"

<span style="color:#048562; font-weight:bold;">原生功能赋能与二层检索架构</span>:
- <span style="color:#048562; font-weight:bold;">激活原生搜索价值</span>：通过注入结构化标签，极大程度发挥了 Zotero 高级检索（Advanced Search）的实战价值，将原本单一的关键词搜索升级为"变量+方法+样本"的复合逻辑筛选
- <span style="color:#048562; font-weight:bold;">二层检索逻辑（Two-tier Retrieval）</span>：支持流程化、高度自定义的二次检索与标注。该设计确保了 AI 结构化标签与用户原有标签架构的解耦（Decoupling），用户可完全自主地进行标签清理与删除管理，不干扰原有的分类秩序

<span style="color:#048562; font-weight:bold;">典型场景</span>：
> "帮我找出所有：<span style="color:#048562; font-weight:bold;">[变量包含：数字化]</span> 且 <span style="color:#048562; font-weight:bold;">[方法包含：双重差分]</span> 且 <span style="color:#048562; font-weight:bold;">[样本包含：制造业]</span> 的文献。"

<span style="color:#048562; font-weight:bold;">价值点</span>：彻底解决"我记得读过这类文章，但死活找不到"的科研痛点。

### <span style="color:#7CA531;">TagM1- 变量定义与衡量</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P3-TagM1
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V2)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 筛选特定变量的定义与测量方法
- **筛选规则**: 基于 `V1-def/`, `V2-mea/` 标签
- <span style="color:#048562; font-weight:bold;">输出</span>: 筛选结果 + 自定义标签
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 变量定义与衡量方法整理


### <span style="color:#7CA531;">TagM2- 主题变量分类</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P3-TagM2
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V3)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 筛选特定主题的变量研究
- <span style="color:#048562; font-weight:bold;">筛选规则</span>: 基于变量标签 + 关键词匹配
- <span style="color:#048562; font-weight:bold;">输出</span>: 筛选结果 + 主题标签
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 主题变量研究筛选


### <span style="color:#7CA531;">TagM3- 控制变量类型</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P3-TagM3
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V2)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 筛选特定类型的控制变量
- **筛选规则**: 基于 `A6-CV/` 标签 + 关键词
- <span style="color:#048562; font-weight:bold;">输出</span>: 筛选结果 + 控制变量类型标签
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 控制变量类型分析


### <span style="color:#7CA531;">TagM4- 地区固定效应</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P3-TagM4
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V2)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 筛选固定效应相关研究
- <span style="color:#048562; font-weight:bold;">筛选规则</span>: 基于地区、年份、行业关键词
- <span style="color:#048562; font-weight:bold;">输出</span>: 筛选结果 + 固定效应标签
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 固定效应研究筛选

---


## <span style="color:#43973D;">P4 - 文献综述模块（结构化综述矩阵生成）</span>
<span style="color:#048562; font-weight:bold;">核心目标</span>：将"数据库数据"转化为"学术产出文稿"，不再是基于摘要乱写，而是读取已校准的 P1 标签，自动填充 11 维综述矩阵

<span style="color:#048562; font-weight:bold;">逻辑驱动的智能生产</span>:
- <span style="color:#048562; font-weight:bold;">高质量生成任务</span>：利用"预先治理、逻辑对齐"的输入策略，显著提升了 RAG 在文献综述、文档问答场景下的严谨性。由于输入端是高度逻辑化的"知识组件"，生成内容的逻辑深度远超常规 AI 阅读工具
- <span style="color:#048562; font-weight:bold;">全链路证据溯源</span>：确保所有生成任务均具备物理级溯源能力，支持一键回溯来源，从根源抑制 AI 幻觉，确保科研产出的 100% 确定性
- <span style="color:#048562; font-weight:bold;">大规模输入能力</span>：本系统一次性同时将两三百篇文献作为输入，这是目前的 AI 阅读工具以及其他的 AI 科研工具无法达到的

### <span style="color:#7CA531;">LR0- 摘要简洁</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P4-LR0
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V1)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 清理摘要中的翻译标记
- <span style="color:#048562; font-weight:bold;">输出</span>: 清理后的摘要
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 摘要预处理


### <span style="color:#7CA531;">LR1- 矩阵标签生成</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P4-LR1
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V1-5AY, V1-2AYJ, V1-2YJ
- <span style="color:#048562; font-weight:bold;">描述</span>: 生成 11 维文献综述矩阵
- <span style="color:#048562; font-weight:bold;">输出格式</span>:
  - V1-5AY: 5 列表格（详细版）
  - V1-2AYJ: 2 列表格（作者 - 年份 - 期刊）
  - V1-2YJ: 2 列表格（年份 - 期刊）
- <span style="color:#048562; font-weight:bold;">分析维度</span>: 11 个标准化维度
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 文献综述撰写


### <span style="color:#7CA531;">LR2- 矩阵标签要点</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P4-LR2
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V3-AY)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 生成要点式文献综述
- <span style="color:#048562; font-weight:bold;">输出格式</span>: 3-7 列表格
- <span style="color:#048562; font-weight:bold;">特点</span>: 要点式、知识图谱矩阵
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 要点式综述


### <span style="color:#7CA531;">LR3- 矩阵标签测量</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P4-LR3
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V2- 测度)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 生成变量定义与衡量的专门综述
- <span style="color:#048562; font-weight:bold;">输出格式</span>: 6 列表格
- <span style="color:#048562; font-weight:bold;">列内容</span>: 测量方法、核心内涵、常用指标、数据来源、应用层次、优缺点
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 变量定义与衡量方法整理


### <span style="color:#7CA531;">LR4- 文献综述（研究议程识别）</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P4-LR4
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成 (V1)
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">描述</span>: 生成研究议程矩阵
- <span style="color:#048562; font-weight:bold;">输出格式</span>: 4 列表格
- <span style="color:#048562; font-weight:bold;">特点</span>: <span style="color:#048562; font-weight:bold;">基于标签分布，自动识别该领域的 3-5 个核心研究议程（Research Agendas）</span>
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 研究议程识别

---


## <span style="color:#43973D;">P5 - 文献引用模块</span>

### <span style="color:#7CA531;">Cite1- 文献引用添加</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P5-Cite1
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P0
- <span style="color:#048562; font-weight:bold;">版本</span>: V1-AY, V1-AYN, V1-YJ
- <span style="color:#048562; font-weight:bold;">描述</span>: 智能添加文献引用
- <span style="color:#048562; font-weight:bold;">核心原则</span>:
  - V1-AY: 精准匹配（1-2 个最相关）
  - V1-AYN: 穷尽式（所有相关）
  - V1-YJ: 穷尽式（年份 - 期刊格式）
- <span style="color:#048562; font-weight:bold;">引用格式</span>:
  - AY: (作者, 年份, 期刊) 编号
  - YJ: (年份, 期刊) 编号
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 论文写作、引用添加

---


## <span style="color:#43973D;">P6 - 交互系统模块（证据溯源型对话）</span>
**核心目标**：基于已治理的库，进行零幻觉的对话。对话不仅基于 PDF 全文，还参考 P1 提取的结构化标签，回答必须带 `[number]` 锚点

<span style="color:#048562; font-weight:bold;">逻辑驱动的智能生产与问答</span>:
- <span style="color:#048562; font-weight:bold;">高质量生成任务</span>：利用"预先治理、逻辑对齐"的输入策略，显著提升了 RAG 在文献综述、文档问答场景下的严谨性。由于输入端是高度逻辑化的"知识组件"，生成内容的逻辑深度远超常规 AI 阅读工具
- <span style="color:#048562; font-weight:bold;">全链路证据溯源</span>：确保所有生成任务均具备物理级溯源能力，支持一键回溯来源，从根源抑制 AI 幻觉，确保科研产出的 100% 确定性

<span style="color:#048562; font-weight:bold;">核心特性</span>:
- ✅ <span style="color:#048562; font-weight:bold;">嵌入式交互系统</span>：能够将这些交互笔记直接添加在 Zotero 中，而不是其他软件中
- ✅ <span style="color:#048562; font-weight:bold;">直接跳转特定段落</span>：由于是嵌入式的，还可以直接跳转特定段落
- ✅ <span style="color:#048562; font-weight:bold;">知识库问答</span>：能够作为自己的知识库，将自己的文献库作为知识来源，提问，然后根据文献库的内容继续进行回答
- ✅ <span style="color:#048562; font-weight:bold;">多源输入支持</span>：可以自由选择输入来源，不论是 Zotero 中的文献、标记、标注，还是剪贴板内容、外部文件，都可以进行交互
- ✅ <span style="color:#048562; font-weight:bold;">工作流集成</span>：P6 的交互笔记以及任意的 Zotero 中的任何内容与 Obsidian 都是双向链接的，同步更新，这样可以充分利用 Zotero 与 Obsidian 的特性

### <span style="color:#7CA531;">ASK- 文献阅读交互</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P6-ASK
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P1
- <span style="color:#048562; font-weight:bold;">子功能</span>:
  - ASK0: 文献阅读交互系统设计
  - AskS1: AskSelection (选中文本查询)
  - AskS2: AskAnnotations (注释查询)
  - AskS3: AskClipboard (剪贴板查询)
  - AskS4: AskPDF (PDF 查询)
  - AskS5: AskPDFFullText (PDF 全文查询)
  - AskS6: AskNote (笔记查询)
  - AskS8: AskItemMeta (元数据查询)
  - **AskS9: AskZotero (Zotero 查询)** - 核心功能，对话不仅基于 PDF 全文，还参考 P1 提取的结构化标签，回答必须带 `[number]` 锚点
- <span style="color:#048562; font-weight:bold;">描述</span>: 对话式文献查询与问答，<span style="color:#048562; font-weight:bold;">证据溯源型对话</span>
- <span style="color:#048562; font-weight:bold;">技术特点</span>: 基于已校准标签，确保可追溯性
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 快速文献查询、内容理解、知识库问答


### <span style="color:#7CA531;">Upload- 文件上传</span>
- <span style="color:#048562; font-weight:bold;">功能 ID</span>: P6-Upload
- <span style="color:#048562; font-weight:bold;">状态</span>: ✅ 已完成
- <span style="color:#048562; font-weight:bold;">优先级</span>: P2
- <span style="color:#048562; font-weight:bold;">子功能</span>:
  - Upload1: UploadFile (文件上传)
  - UploadS2: UploadPDF (PDF 上传)
- <span style="color:#048562; font-weight:bold;">描述</span>: 支持文件上传与处理
- <span style="color:#048562; font-weight:bold;">使用场景</span>: 文件管理、内容提取

---


## <span style="color:#43973D;">功能优先级矩阵</span>

```mermaid
quadrantChart
    title 功能优先级矩阵
    x-axis 低影响 --> 高影响
    y-axis 低复杂度 --> 高复杂度
    quadrant-1 快速实现
    quadrant-2 战略重点
    quadrant-3 谨慎考虑
    quadrant-4 长期规划
    P1-标签解构: [0.9, 0.7]
    P4-文献综述: [0.9, 0.6]
    P5-文献引用: [0.8, 0.5]
    P2-标签统计: [0.6, 0.4]
    P3-自定义筛选: [0.7, 0.5]
    P0-基础操作: [0.5, 0.3]
    P6-交互系统: [0.6, 0.7]
```

---


## <span style="color:#43973D;">功能完成度统计</span>
| 模块 | 计划功能 | 已完成 | 进行中 | 待开发 | 完成率 |
|------|---------|--------|--------|--------|--------|
| P0 | 4 | 4 | 0 | 0 | 100% |
| P1 | 21 | 21 | 0 | 0 | 100% |
| P2 | 2 | 2 | 0 | 0 | 100% |
| P3 | 4 | 4 | 0 | 0 | 100% |
| P4 | 7 | 7 | 0 | 0 | 100% |
| P5 | 3 | 3 | 0 | 0 | 100% |
| P6 | 11 | 11 | 0 | 0 | 100% |
| <span style="color:#048562; font-weight:bold;">总计</span> | <span style="color:#048562; font-weight:bold;">52</span> | <span style="color:#048562; font-weight:bold;">52</span> | <span style="color:#048562; font-weight:bold;">0</span> | <span style="color:#048562; font-weight:bold;">0</span> | <span style="color:#048562; font-weight:bold;">100%</span> |

---


## <span style="color:#43973D;">未来功能规划</span>

### <span style="color:#7CA531;">Q1 2026</span>
- 🔲 性能优化功能
- 🔲 批量处理增强
- 🔲 错误处理改进


### <span style="color:#7CA531;">Q2 2026</span>
- 🔲 协作功能
- 🔲 版本控制
- 🔲 权限管理


### <span style="color:#7CA531;">Q3 2026</span>
- 🔲 可视化增强
- 🔲 知识图谱
- 🔲 数据仪表盘


### <span style="color:#7CA531;">Q4 2026</span>
- 🔲 API 开放
- 🔲 插件市场
- 🔲 移动端支持

---

<span style="color:#048562; font-weight:bold;">文档状态</span>: ✅ 已完成（v3.0）  
<span style="color:#048562; font-weight:bold;">最后更新</span>: 2026-01-14

