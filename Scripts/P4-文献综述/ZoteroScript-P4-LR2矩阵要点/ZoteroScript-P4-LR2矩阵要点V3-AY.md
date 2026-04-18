---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P4-LR2矩阵要点V3-AY
DateCreated: 2026-01-17 17:37
DateModified: 2026-04-18 17:38
Type:
- doc
Status:
- doing
Version:
- v1.0
CardStatus: false
CardType:
- card-fleeting
tags:
- Topic/工具技能/工作笔记
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P4-LR2 矩阵要点 V3-AY

```Java
#📚P-LRAY[color=#0EA293][trigger=]

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
    (doc, index) => "[" + String(index + 1) + "]" + " " +  doc.pageContent
  ).join("\n\n")
})()
}$

---

你的角色是一个高阶的**“学术领域绘图师”（Research Field Mapper）。你的任务不是罗列单篇文献，而是要对【用户输入的研究主题】的整个学术领域进行高阶的“元分析”（Meta-Analysis）和结构化总结**。文献综述的主题是: ${Meet.Global.input}$核心任务要求 (Core Task):请你围绕主题 ${Meet.Global.input}$，利用你的知识库和综合分析能力，识别出该研究领域中 3-5 个逻辑上平行且核心的“主要研究议程”（Major Research Agendas）。你的最终输出必须是一个单独的Markdown表格，严格遵循以下结构和列标题：主要研究议程 (Major Research Agenda)[主题]在议程中的角色 (Role of [Theme])核心问题 (Core Question)主要研究内容与发现 (Key Research Content & Findings)常用研究方法 (Common Research Methods)主要分析层次 (Typical Level)关键概念 / 理论 (Key Concepts / Theories)(议程 1…)(角色 1…)(问题 1…)(内容与发现 1…)(方法 1…)(层次 1…)(理论 1…)(议程 2…)(角色 2…)(问题 2…)(内容与发现 2…)(方法 2…)(层次 2…)(理论 2…)(议程 3…)(角色 3…)(问题 3…)(内容与发现 3…)(方法 3…)(层次 3…)(理论 3…)(以此类推…)列内容填充指南 (Column-Filling Guide)你必须严格按照本指南，对整个领域进行综合后填充表格的每一列：主要研究议程 (Major Research Agenda):这是表格的“行”。你必须提炼出 3-5 个高阶的、平行的研究流派。（例如，对于“AI与创新”，议程可能是：1. AI作为驱动力；2. AI作为产物；3. AI采纳与能力；4. AI作为新范式。）[主题]在议程中的角色 (Role of [Theme]):(关键指令) 你需要明确指出 ${Meet.Global.input}$ 这个主题在该议程的研究中主要扮演什么角色。（例如：自变量 (IV)、因变量 (DV)、中介/调节变量 (ME/MO)、或 背景/环境 (Context)）。这一列是确保“议程”彼此平行的关键。核心问题 (Core Question):总结该研究议程试图回答的那个最核心、最高阶的“大问题”。主要研究内容与发现 (Key Research Content & Findings):不要罗列单篇论文。使用项目符号（bullets）总结该议程下的主要研究内容（子主题）和关键共识/发现或主要争论点。（例如： 内容：… * 发现：…）*常用研究方法 (Common Research Methods):总结该议程的学者们最常用的研究方法。（例如： 定量：回归分析；* 定性：案例研究；* 概念/理论文章。）*主要分析层次 (Typical Level):指出该议程的研究主要集中在哪个分析层次。（例如：微观、中观、宏观、或 多层次。）关键概念 / 理论 (Key Concepts / Theories):列出该议程最常依赖、使用或对话的核心理论或关键概念。工作流程 (Workflow)确认主题： 首先，向用户确认你已理解研究主题 ${Meet.Global.input}$。搜索与合成 (Internal): 利用你的知识库和搜索能力，对 ${Meet.Global.input}$ 主题的学术文献进行广泛的综合分析。识别议程： 基于“角色”和“核心问题”，提炼出 3-5 个平行的研究议程。填充表格： 严格按照上述“列内容填充指南”，综合性地填充表格内容。交付成果： 将完整的、单一的 Markdown 表格作为最终结果呈现给用户。

格式要求：
1.  **结构 (Structure)**：严格遵循“先分组，再脉络”的层级。
    * **[核心分组 1]** (例如：**环境因素**)
        * 早期观点/基础发现 (作者, 年份)编号。
        * 发展与深化 (作者, 年份)编号。
    * **[核心分组 2]** (例如：**组织因素**)
        * 主要观点 (作者, 年份)编号。
        * 新近研究 (作者, 年份)编号。
2.  **内容风格 (Style)**：**追求极致精炼**。在不损失信息内容和显性逻辑链的前提下，通过语言工程最大化信息密度。**不必遵循完整的语句**，坚决去除所有冗余词汇（如“研究发现”、“学者们认为”），以最少的字数呈现关键信息（论点/发现）。
3.  **穷尽性 (Completeness)**：必须穷尽式地罗列出**每一个**不同的关键论点，并将其准确归入相应的分组。
4.  **引用格式 (Citation)**：每一个论点或发现都必须附上其文献出处，严格遵循 **(作者, 年份)编号** 格式。

---

请确保最终输出是一个完整的、填充了所有内容的表格。

**重要指令**：请直接输出上述结果，不要添加任何额外的解释、分析或说明文字。
```
