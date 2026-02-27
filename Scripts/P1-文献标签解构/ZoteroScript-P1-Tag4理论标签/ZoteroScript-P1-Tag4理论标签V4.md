---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag4理论标签V4
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: [AI, JavaScript, Zotero, 代码, 学术研究, 文献分析, 理论框架, 理论识别, 知识管理, 自动化标签]
RelatedNote:
CardRecord:
---


## ZoteroScript-P1-Tag4 理论标签 V4

```javascript
#🏷️TheoryTags[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";

  let successCount = 0;         // 成功标注到理论（标准理论+新理论）
  let noTheoryCount = 0;        // AI分析后没有理论
  let skipCount = 0;            // 已有理论标签跳过
  let failCount = 0;            // 解析失败
  let noAbstractCount = 0;      // 没有摘要
  let noKeywordCount = 0;       // 无理论关键词
  let errorCount = 0;           // 保存失败

  // 完整的理论库 - 保持所有理论，添加核心别名
  const theoriesDatabase = [
    {name: "Ability-motivation-opportunity (AMO) model", aliases: ["AMO model", "AMO theory", "AMO framework"]},
    {name: "Absorptive capacity theory", aliases: ["ACAP", "absorptive capacity", "absorptive capacity model"]},
    {name: "Actor-network theory", aliases: ["ANT", "actor network theory", "actor-network"]},
    {name: "Translation sociology", aliases: ["translation theory", "sociology of translation"]},
    {name: "Affective event theory", aliases: ["AET", "affective events theory"]},
    {name: "Agency theory", aliases: ["principal-agent theory", "agency model", "principal agent"]},
    {name: "Agenda-setting theory", aliases: ["agenda setting", "agenda-setting model"]},
    {name: "Attachment theory", aliases: ["attachment style", "attachment styles"]},
    {name: "Attribution theory", aliases: ["attribution model", "causal attribution"]},
    {name: "Balance theory", aliases: ["cognitive balance", "balance model"]},
    {name: "Consistency theory", aliases: ["cognitive consistency", "consistency model"]},
    {name: "Broaden-and-build theory", aliases: ["broaden and build", "broaden-and-build model"]},
    {name: "Co-evolution theory", aliases: ["coevolution theory", "co-evolutionary theory"]},
    {name: "Cognitive appraisal theory", aliases: ["appraisal theory", "cognitive appraisal"]},
    {name: "Cognitive dissonance theory", aliases: ["dissonance theory", "cognitive dissonance"]},
    {name: "Compensatory ethics theory", aliases: ["compensatory ethics", "moral compensation"]},
    {name: "Complexity theory and organizations", aliases: ["complexity theory", "organizational complexity"]},
    {name: "Configural organizational theory", aliases: ["configural theory", "configuration theory"]},
    {name: "Conservation of resource theory", aliases: ["COR theory", "conservation of resources", "COR"]},
    {name: "Construal level theory", aliases: ["CLT", "construal theory", "psychological distance"]},
    {name: "Contingency theory", aliases: ["situational theory", "contingency approach"]},
    {name: "Control theory", aliases: ["cybernetic theory", "cybernetic control"]},
    {name: "Cybernetic theory", aliases: ["cybernetics", "control theory"]},
    {name: "Co-opetition theory", aliases: ["coopetition theory", "cooperative competition"]},
    {name: "Diffusion of innovation theory", aliases: ["innovation diffusion", "rogers diffusion", "DOI"]},
    {name: "Dual system theory", aliases: ["dual process theory", "dual-system model"]},
    {name: "Dynamic capabilities theory", aliases: ["dynamic capabilities", "DC theory", "dynamic capability"]},
    {name: "Efficient market theory", aliases: ["EMH", "efficient market hypothesis", "market efficiency"]},
    {name: "Ego depletion theory", aliases: ["ego depletion", "self-control depletion"]},
    {name: "Emotional contagion theory", aliases: ["emotional contagion", "emotion contagion"]},
    {name: "Emotions as social information theory", aliases: ["EASI theory", "EASI model"]},
    {name: "Ethical leadership theory", aliases: ["ethical leadership", "ethical leader"]},
    {name: "Ethical theory", aliases: ["ethics theory", "moral theory"]},
    {name: "Event system theory", aliases: ["EST", "event systems theory"]},
    {name: "Fairness heuristic theory", aliases: ["fairness heuristic", "FHT"]},
    {name: "Feelings-as-information theory", aliases: ["feelings as information", "affect as information"]},
    {name: "Field theory", aliases: ["organizational field", "institutional field"]},
    {name: "Game theory", aliases: ["strategic games", "game theoretic"]},
    {name: "Goal orientation theory", aliases: ["achievement goal theory", "goal orientation"]},
    {name: "Goal setting theory", aliases: ["goal theory", "locke goal setting", "goal-setting"]},
    {name: "Image theory", aliases: ["image model", "behavioral decision theory"]},
    {name: "Implicit leadership theory", aliases: ["ILT", "implicit leadership", "leadership prototype"]},
    {name: "Institutional theory", aliases: ["neo-institutional theory", "institutionalism", "institutional"]},
    {name: "Interaction ritual chain theory", aliases: ["IRC theory", "interaction ritual"]},
    {name: "Job demands-resources model", aliases: ["JD-R model", "JDR", "job demands resources"]},
    {name: "Knowledge-based theory", aliases: ["KBV", "knowledge based view", "knowledge-based view"]},
    {name: "Leader-member exchange (LMX) theory", aliases: ["LMX", "leader member exchange", "LMX theory"]},
    {name: "Loose coupling theory", aliases: ["loose coupling", "loosely coupled systems"]},
    {name: "Media richness theory", aliases: ["information richness", "media richness"]},
    {name: "Mental models theory", aliases: ["mental models", "shared mental models"]},
    {name: "Middle-status conformity theory", aliases: ["middle status conformity", "middle-status"]},
    {name: "Minority influence theory", aliases: ["minority influence", "innovation theory"]},
    {name: "Moral licensing theory", aliases: ["moral licensing", "moral credentials"]},
    {name: "Need-to-belong theory", aliases: ["need to belong", "belongingness theory"]},
    {name: "Neo-institutional theory", aliases: ["institutional theory", "new institutionalism"]},
    {name: "Organizational ecology theory", aliases: ["population ecology", "organizational ecology"]},
    {name: "Organizational justice theory", aliases: ["organizational justice", "justice theory"]},
    {name: "Organizational learning theory", aliases: ["organizational learning", "learning organization"]},
    {name: "Person-environment fit theory", aliases: ["PE fit", "person environment fit", "P-E fit"]},
    {name: "Planned behavior theory", aliases: ["TPB", "theory of planned behavior", "planned behaviour"]},
    {name: "Population ecology theory", aliases: ["organizational ecology", "population ecology"]},
    {name: "Prospect theory", aliases: ["prospect model", "kahneman tversky", "loss aversion"]},
    {name: "Psychological contract theory", aliases: ["psychological contract", "psychological contracts"]},
    {name: "Regulatory focus theory", aliases: ["RFT", "promotion prevention", "regulatory focus"]},
    {name: "Relative deprivation theory", aliases: ["relative deprivation", "deprivation theory"]},
    {name: "Resource allocation theory", aliases: ["resource allocation", "allocation theory"]},
    {name: "Resource dependence theory", aliases: ["RDT", "resource dependence", "resource-dependence"]},
    {name: "Resource-based view (RBV)", aliases: ["RBV", "resource based view", "resource-based theory"]},
    {name: "Role congruity theory", aliases: ["role congruity", "gender role congruity"]},
    {name: "Role theory", aliases: ["role conflict", "role ambiguity", "organizational roles"]},
    {name: "Self-categorization theory", aliases: ["SCT", "self categorization", "social categorization"]},
    {name: "Self-control theory", aliases: ["self control", "self-regulation theory"]},
    {name: "Self-determination theory", aliases: ["SDT", "self determination", "intrinsic motivation"]},
    {name: "Self-verification theory", aliases: ["self verification", "identity verification"]},
    {name: "Sensemaking theory", aliases: ["sensemaking", "sense making", "organizational sensemaking"]},
    {name: "Servant leadership theory", aliases: ["servant leadership", "servant leader"]},
    {name: "Signaling theory", aliases: ["signal theory", "signalling", "signaling model"]},
    {name: "Social capital theory", aliases: ["social capital", "network capital"]},
    {name: "Social cognitive theory", aliases: ["SCT", "social learning theory", "social cognitive"]},
    {name: "Social comparison theory", aliases: ["social comparison", "comparison theory"]},
    {name: "Social contagion theory", aliases: ["social contagion", "behavioral contagion"]},
    {name: "Social contract theory", aliases: ["social contract", "contractual theory"]},
    {name: "Social dominance orientation theory", aliases: ["SDO", "social dominance", "dominance orientation"]},
    {name: "Social exchange theory", aliases: ["SET", "exchange theory", "social exchange"]},
    {name: "Social facilitation theory", aliases: ["social facilitation", "audience effect"]},
    {name: "Social identity theory", aliases: ["SIT", "social identity", "group identity"]},
    {name: "Social information processing theory", aliases: ["SIP theory", "social information processing"]},
    {name: "Social interdependence theory", aliases: ["interdependence theory", "social interdependence"]},
    {name: "Social learning theory", aliases: ["observational learning", "modeling theory"]},
    {name: "Social network theory", aliases: ["network theory", "social networks", "network analysis"]},
    {name: "Social representation theory", aliases: ["social representations", "representation theory"]},
    {name: "Stakeholder theory", aliases: ["stakeholder approach", "stakeholder model"]},
    {name: "Stewardship theory", aliases: ["stewardship model", "stewardship approach"]},
    {name: "Strategic choice theory", aliases: ["strategic choice", "strategic decision"]},
    {name: "Structural adaptation theory", aliases: ["structural adaptation", "adaptive structuration"]},
    {name: "Structural contingency theory", aliases: ["structural contingency", "contingency structure"]},
    {name: "Structuration theory", aliases: ["giddens structuration", "structuration", "structure theory"]},
    {name: "Tournament theory", aliases: ["tournament model", "promotion tournament"]},
    {name: "Trait activation theory", aliases: ["TAT", "trait activation", "personality activation"]},
    {name: "Transaction cost theory", aliases: ["TCT", "transaction cost economics", "TCE"]},
    {name: "Transformational leadership theory", aliases: ["transformational leadership", "transformational leader"]},
    {name: "Uncertainty management theory", aliases: ["uncertainty management", "UMT"]},
    {name: "Uncertainty-identity theory", aliases: ["uncertainty identity", "identity uncertainty"]},
    {name: "Upper echelons theory", aliases: ["upper echelon", "top management team", "TMT"]},
    {name: "Configurational theory", aliases: ["configuration theory", "configurational approach"]},
    {name: "Evolutionary theory", aliases: ["organizational evolution", "evolutionary approach"]},
    {name: "Middle range theory", aliases: ["middle-range theory", "merton theory"]},
    {name: "Paradox theory", aliases: ["organizational paradox", "paradox management"]},
    {name: "Internal market efficiency arguments", aliases: ["Internal market efficiency arguments theory"]},
    {name: "Portfolio theory", aliases: ["Portfolio theory"]},
    {name: "Market power theory", aliases: ["Market power theory"]},
    {name: "Real options theory(ROT)", aliases: ["ROT"]},
    {name: "Neoclassical investment Theory", aliases: ["Neoclassical investment Theory"]},
    {name: "Organizational ambidexterity (OA)", aliases: ["ambidexterity", "OA"]},
    {name: "Agglomeration theory", aliases: ["agglomeration", "cluster theory", "industrial agglomeration"]},
    {name: "Ricardian theory", aliases: ["Ricardo theory", "comparative advantage theory", "ricardian model"]},
    {name: "Economic geography theory", aliases: ["economic geography", "spatial economics", "geographical economics"]},
    {name: "Structure of preferences", aliases: ["preference structure", "preferences theory", "preference model"]},
    {name: "Knowledge base theory", aliases: ["knowledge base", "KB theory", "knowledge-base theory"]}
  ];

  // 快速匹配函数
  const findTheory = (searchTerm) => {
    if (!searchTerm) return null;
    const term = searchTerm.toLowerCase().trim();

    // 完全匹配和包含匹配
    for (let theory of theoriesDatabase) {
      if (theory.name.toLowerCase() === term ||
          theory.name.toLowerCase().includes(term) ||
          term.includes(theory.name.toLowerCase())) {
        return theory.name;
      }

      if (theory.aliases) {
        for (let alias of theory.aliases) {
          if (alias.toLowerCase() === term ||
              alias.toLowerCase().includes(term) ||
              term.includes(alias.toLowerCase())) {
            return theory.name;
          }
        }
      }
    }

    // 关键词匹配（≥2个词重合）
    const getWords = str => str.toLowerCase().replace(/[^a-z0-9 ]/g, '').split(' ').filter(w => w.length > 2);
    const searchWords = getWords(term);

    if (searchWords.length >= 2) {
      for (let theory of theoriesDatabase) {
        const theoryWords = getWords(theory.name);
        const common = theoryWords.filter(w => searchWords.includes(w));
        if (common.length >= 2) return theory.name;
      }
    }

    return null;
  };

  // 精简触发关键词
  const triggerKeywords = ["theory", "theories", "theoretical", "framework", "model", "view", "lens", "理论", "视角"];

  // 简化的内容检查函数
  const hasTheoryKeywords = (text) => {
    const lowerText = text.toLowerCase();
    return triggerKeywords.some(keyword => lowerText.includes(keyword));
  };

  for (let item of items) {
    try {
      // 跳过已处理的条目
      const existingTags = item.getTags();
      if (existingTags.some(tag => tag.tag && tag.tag.startsWith("Theory/"))) {
        skipCount++;
        continue;
      }

      let content = "";

      // 内容获取 - 优先PDF前3000字符，备选摘要
      try {
        // 首先尝试获取PDF内容
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text) {
              // 优先使用PDF前3000字符，包含引言和理论基础
              content = fullTextData.text.substring(0, 3000).trim();
            }
          } catch {}
        }

        // 如果PDF获取失败，再使用摘要
        if (!content) {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        }
      } catch {}

      if (!content) {
        // 既没有摘要也没有附件，标注"无内容"
        try {
          item.addTag('Theory/无内容');
          await item.saveTx();
          noAbstractCount++;
        } catch {
          errorCount++;
        }
        continue;
      }

      // 快速关键词检查
      if (!hasTheoryKeywords(content)) {
        // 无理论关键词，直接标注
        try {
          item.addTag('Theory/无理论关键词');
          await item.saveTx();
          noKeywordCount++;
        } catch {
          errorCount++;
        }
        continue;
      }

      // AI分析 - 简化prompt减少token消耗
      try {
        const prompt = `分析文献内容，找出明确提到的理论：

${content}

要求：
1. 仅识别明确的理论、模型或框架名称
2. 英文文献用英文，中文文献用中文
3. 使用原文表述
4. 返回最主要的1个理论
5. 格式：["理论名"] 或 ["无明确理论"]`;

        const response = await Meet.Global.views.ask(prompt);

        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            item.addTag('Theory/解析失败');
            await item.saveTx();
            failCount++;
            continue;
          }

          const theories = JSON.parse(jsonMatch[0]);
          let identifiedTheory = "";

          if (!theories || theories.length === 0 || !theories[0] || theories[0].trim() === "" || theories[0].trim() === "无明确理论") {
            identifiedTheory = "无明确理论";
          } else {
            identifiedTheory = theories[0].trim();
          }

          // 理论匹配和标签添加
          let tagToAdd = "";
          if (identifiedTheory === "无明确理论") {
            tagToAdd = "无明确理论";
            noTheoryCount++;
          } else {
            const foundTheory = findTheory(identifiedTheory);
            if (foundTheory) {
              tagToAdd = foundTheory;
              successCount++;
            } else {
              tagToAdd = identifiedTheory + '-new';
              successCount++;
            }
          }

          try {
            item.addTag('Theory/' + tagToAdd);
            await item.saveTx();
          } catch {
            errorCount++;
          }

        } catch {
          // JSON解析失败，标注为解析失败
          try {
            item.addTag('Theory/解析失败');
            await item.saveTx();
            failCount++;
          } catch {
            errorCount++;
          }
        }

      } catch {
        // AI调用失败
        try {
          item.addTag('Theory/解析失败');
          await item.saveTx();
          failCount++;
        } catch {
          errorCount++;
        }
      }

    } catch {
      errorCount++;
    }
  }

  return `理论标签处理完成 - 成功: ${successCount}, 无理论: ${noTheoryCount}, 跳过: ${skipCount}, 失败: ${failCount}, 无内容: ${noAbstractCount}, 无关键词: ${noKeywordCount}, 保存失败: ${errorCount}`;
})()
}$
```
