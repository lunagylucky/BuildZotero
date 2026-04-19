---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag4理论标签V1
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
- 代码
- 脚本
- 理论标签
- Zotero
- Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord: ''
---

## ZoteroScript-P1-Tag4 理论标签 V1

```javascript
#AddTags[color=#66dc79][trigger=]
${
(async ()=>{
  const items = ZoteroPane.getSelectedItems();
  
  // 定义新的理论名称列表
  const theoriesList = [
    {"name": "Ability-motivation-opportunity (AMO) model"},
    {"name": "Absorptive capacity theory"},
    {"name": "Actor-network theory"},
    {"name": "Translation sociology"},
    {"name": "Affective event theory"},
    {"name": "Agency theory"},
    {"name": "Agenda-setting theory"},
    {"name": "Attachment theory"},
    {"name": "Attribution theory"},
    {"name": "Balance theory"},
    {"name": "Consistency theory"},
    {"name": "Broaden-and-build theory"},
    {"name": "Co-evolution theory"},
    {"name": "Cognitive appraisal theory"},
    {"name": "Cognitive dissonance theory"},
    {"name": "Compensatory ethics theory"},
    {"name": "Complexity theory and organizations"},
    {"name": "Configural organizational theory"},
    {"name": "Conservation of resource theory"},
    {"name": "Construal level theory"},
    {"name": "Contingency theory"},
    {"name": "Control theory"},
    {"name": "Cybernetic theory"},
    {"name": "Co-opetition theory"},
    {"name": "Diffusion of innovation theory"},
    {"name": "Dual system theory"},
    {"name": "Dynamic capabilities theory"},
    {"name": "Efficient market theory"},
    {"name": "Ego depletion theory"},
    {"name": "Emotional contagion theory"},
    {"name": "Emotions as social information theory"},
    {"name": "Ethical leadership theory"},
    {"name": "Ethical theory"},
    {"name": "Event system theory"},
    {"name": "Fairness heuristic theory"},
    {"name": "Feelings-as-information theory"},
    {"name": "Field theory"},
    {"name": "Game theory"},
    {"name": "Goal orientation theory"},
    {"name": "Goal setting theory"},
    {"name": "Image theory"},
    {"name": "Implicit leadership theory"},
    {"name": "Institutional theory"},
    {"name": "Interaction ritual chain theory"},
    {"name": "Job demands-resources model"},
    {"name": "Knowledge-based theory"},
    {"name": "Leader-member exchange (LMX) theory"},
    {"name": "Loose coupling theory"},
    {"name": "Media richness theory"},
    {"name": "Mental models theory"},
    {"name": "Middle-status conformity theory"},
    {"name": "Minority influence theory"},
    {"name": "Moral licensing theory"},
    {"name": "Need-to-belong theory"},
    {"name": "Neo-institutional theory"},
    {"name": "Organizational ecology theory"},
    {"name": "Organizational justice theory"},
    {"name": "Organizational learning theory"},
    {"name": "Person-environment fit theory"},
    {"name": "Planned behavior theory"},
    {"name": "Population ecology theory"},
    {"name": "Prospect theory"},
    {"name": "Psychological contract theory"},
    {"name": "Regulatory focus theory"},
    {"name": "Relative deprivation theory"},
    {"name": "Resource allocation theory"},
    {"name": "Resource dependence theory"},
    {"name": "Resource-based view (RBV)"},
    {"name": "Role congruity theory"},
    {"name": "Role theory"},
    {"name": "Self-categorization theory"},
    {"name": "Self-control theory"},
    {"name": "Self-determination theory"},
    {"name": "Self-verification theory"},
    {"name": "Sensemaking theory"},
    {"name": "Servant leadership theory"},
    {"name": "Signaling theory"},
    {"name": "Social capital theory"},
    {"name": "Social cognitive theory"},
    {"name": "Social comparison theory"},
    {"name": "Social contagion theory"},
    {"name": "Social contract theory"},
    {"name": "Social dominance orientation theory"},
    {"name": "Social exchange theory"},
    {"name": "Social facilitation theory"},
    {"name": "Social identity theory"},
    {"name": "Social information processing theory"},
    {"name": "Social interdependence theory"},
    {"name": "Social learning theory"},
    {"name": "Social network theory"},
    {"name": "Social representation theory"},
    {"name": "Stakeholder theory"},
    {"name": "Stewardship theory"},
    {"name": "Strategic choice theory"},
    {"name": "Structural adaptation theory"},
    {"name": "Structural contingency theory"},
    {"name": "Structuration theory"},
    {"name": "Tournament theory"},
    {"name": "Trait activation theory"},
    {"name": "Transaction cost theory"},
    {"name": "Transformational leadership theory"},
    {"name": "Uncertainty management theory"},
    {"name": "Uncertainty-identity theory"},
    {"name": "Upper echelons theory"},
    {"name": "Configurational theory"},
    {"name": "Evolutionary theory"},
    {"name": "Middle range theory"},
    {"name": "Paradox theory"}
  ];

  // 提取理论标签的函数
  const extractTheory = async (abs) => {
    const res = await Meet.Global.views.ask(
      "Read the abstract of the article below:\n" + abs +
      "\n---\nReturns one theory name found in this abstract as a JSON list. Each theory name should be in English. The result can be parsed by JSON.parse()."
    );
    try {
      const theories = JSON.parse(res.match(/\[[\s\S]+\]/)[0]);
      if (theories.length > 0) {
        return theories[0];
      }
    } catch {
      // 错误捕获后返回空
    }
    return null;
  };

  // 合并所有触发关键词为一个数组
  const triggerKeywords = [
    // 理论相关关键词
    "theory", "theories", "theoretic", "理论",
    "view", "views", "观点",
    "framework", "model", "paradigm", "perspective",
    "观念", "模式", "模型", "范式", "视角",
    // 特殊情况关键词
    "Job demands-resources model", "JD-R", "工作要求—资源模型",
    "Servant leadership", "SL", "服务型领导",
    "Efficient market hypothesis", "EMH", "有效市场假说",
    "Resource-based view", "RBV", "资源基础观",
    "Resource-based theory", "RBT", "资源基础理论"
  ];

  // 串行处理每个条目（与主题脚本一致）
  for (let item of items) {
    try {
      let abs = item.getField("abstractNote");
      if (!abs) {
        const pdfItem = await item.getBestAttachment();
        if (pdfItem) {
          try {
            const pdfText = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            abs = pdfText.text;
          } catch (pdfError) {
            continue;  // PDF提取失败时跳过
          }
        } else {
          continue;  // 如果没有摘要且没有附件，跳过当前条目
        }
      }

      const shouldProcess = triggerKeywords.some(keyword => abs.toLowerCase().includes(keyword.toLowerCase()));
      if (shouldProcess) {
        try {
          const theoryTag = await extractTheory(abs);
          if (theoryTag) {
            // 匹配逻辑：宽松匹配（完全一致、包含、关键词交集）
            const getWords = str => str.toLowerCase().replace(/[^a-z0-9 ]/g, '').split(' ').filter(Boolean);
            const theoryWords = getWords(theoryTag);
            let foundTheory = theoriesList.find(t => 
              t.name.toLowerCase() === theoryTag.trim().toLowerCase() ||
              theoryTag.trim().toLowerCase().includes(t.name.toLowerCase()) ||
              t.name.toLowerCase().includes(theoryTag.trim().toLowerCase())
            );
            if (!foundTheory) {
              for (let t of theoriesList) {
                const listWords = getWords(t.name);
                const common = listWords.filter(w => theoryWords.includes(w));
                if (common.length >= 2) { // 2个及以上关键词重合
                  foundTheory = t;
                  break;
                }
              }
            }
            // 匹配成功用标准名，失败用AI原始名加-new
            const tagToAdd = foundTheory ? foundTheory.name : (theoryTag + '-new');
            item.addTag('Theory/' + tagToAdd);
            await item.saveTx();
          }
        } catch (aiError) {
          continue;  // AI处理失败时跳过
        }
      }
    } catch (itemError) {
      continue;  // 其他错误时跳过
    }
  }
})()
}$

请直接输出：完成。
