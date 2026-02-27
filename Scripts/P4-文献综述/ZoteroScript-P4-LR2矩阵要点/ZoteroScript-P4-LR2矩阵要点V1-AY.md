---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P4-LR2矩阵要点V1-AY
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:48
Type:
Status:
Version:
CardStatus:
CardType:
tags: []
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P4-LR2 矩阵要点 V1

```javascript
#📚P-LRAY[color=#0EA293][trigger=/(这|本)(个|些|篇)(文献|论文|文章|条目)/]

These are Zotero item informations:

${

(async function() {

  let items = ZoteroPane.getSelectedItems()

  const docs = Meet.Zotero.items2documents(items)

  

  // 为每个文档添加标签信息

  const docsWithTags = docs.map((doc, idx) => {

    const item = items[idx]

    const tags = item.getTags().map(tag => tag.tag).join(', ')

    

    // 将标签信息添加到 pageContent 中

    return {

      …doc,

      pageContent: doc.pageContent + (tags ? `\nTags: ${tags}` : '\nTags: No tags')

    }

  })

  

  Meet.Global.views.insertAuxiliary(docsWithTags)

  

  return docsWithTags.map(

    (doc, index) => "[" + String(index + 1) + "]" + " " +  doc.pageContent

  ).join("\n\n")

})()

}$

---

The output language will be determined based on the user's request. If no specific language is requested, the default will be Simplified Chinese.n. Make sure to cite results using [number] notation after the reference. 

My question is: ${Meet.Global.input || 你的角色是一个不知疲倦的“学术档案管理员”，确保**每一篇**被分析文献中的**每一个**相关论点都被记录在案。

标签含义：A1-DV：因变量，用“+”连接不同变量；A2-IV：自变量，用“+”连接不同变量；A3-MO：调节变量，用“+”连接不同变量；A4-ME：中介变量，用“+”连接不同变量；A5-INV：工具变量，用“+”连接不同变量；A6-CV：控制变量，用“+”连接不同变量；V1-def:变量定义； V2-mea:变量衡量； V3-con:研究贡献； V4-fut:未来展望；

任务要求：请你围绕【用户输入的研究主题】，对【用户提供的文献集合】进行一次严格、客观、穷尽式的文献内容提取，并以一个包含三列的“知识图谱矩阵”表格形式呈现你的分析结果。

该表格的纵向结构应包含以下11个分析维度：

研究背景 (如政策变革、技术范式转移、宏观经济冲击、社会文化趋势、全球化与地缘政治等)

构念定义 (如核心定义、维度划分、内涵演变、竞争性构念、新兴构念等)

测量方法 (如量化指标(熵指数/赫芬达尔指数)、档案数据(专利/财务)、调查问卷(李克特量表)、定性编码(案例/访谈)、文本挖掘等)

理论视角 (如资源基础观(RBV)、动态能力理论、组织学习理论、制度理论、网络理论、交易成本经济学等)

对象层次 (如国家、区域/产业集群、产业、企业、业务单元/项目、团队、个人等)

研究情境 (如发达经济体 vs. 新兴市场、高科技产业 vs. 传统产业、制造业 vs. 服务业、企业所有制(国有/民营/家族)、特定历史时期(危机前后)等)

研究方法 (如定量研究(面板回归/结构方程模型/生存分析)、定性研究(案例比较/扎根理论)、混合方法、实验设计、模拟仿真等)

前因因素 (总结研究主题受到哪些因素影响) (如外部环境因素(环境动态性/竞争强度)、组织内部资源与战略(组织冗余/战略导向)、高管团队特征(TMT)、网络关系(联盟/网络位置)等)

作用后果 (总结研究主题对哪些因素产生影响) (如财务绩效、创新绩效(探索式/利用式)、市场绩效、组织生存、组织韧性、组织学习与适应性等)

中介机制 (识别并总结解释“如何”起作用的内在机制) (如吸收能力、知识整合、组织学习、动态能力、探索与利用平衡等)

调节因素 (识别并总结影响效果强弱的边界条件) (如环境不确定性、制度环境、组织冗余、组织文化、企业规模/年龄、网络位置等)

对于表格中的每一行（即每一个分析维度），你的输出必须严格按照以下三个横向列进行构建：

第一列：要点 (Key Points)

要求：穷尽式地列举在所有文献中识别出的每一个不同的关键主题或论点，并将其提炼为简练的关键词或短语。每一个要点都必须附上其代表性文献出处，并遵循“最新优先”原则。格式为：关键词 作者 年份[文献编号];。

第二列：关键发现与文献支撑 (Key Findings & Literature Support

要求：以要点列表形式，精准陈述该维度的关键发现。对于每一个发现，都必须穷尽式地罗列出所有支持该观点的文献引用，并遵循“最新优先”原则进行排序。格式为：作者 年份[文献编号]。

第三列：发展脉络与趋势 (Development Trajectory & Trends)

要求：基于第二列穷尽式罗列的全部信息，客观、精炼地描述该维度下研究焦点的演进过程。必须在段落中嵌入相应的文献引用来支撑你的脉络梳理。

请确保最终输出是一个完整的、填充了所有内容的Markdown表格。

"}$

**重要指令**：请直接输出上述结果，不要添加任何额外的解释、分析或说明文字。
```

📒
