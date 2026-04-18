---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag4理论标签V4-Rnote
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
- AI分析
- JavaScript
- Zotero
- 代码
- 学术研究
- 文献管理
- 智能化工具
- 理论识别
- 知识管理
- 自动标签
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P1-Tag4 理论标签 V4-Rnote

```javascript
#🏷️R-TheoryTags[color=#66dc79][trigger=]
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
  let updateCount = 0;          // 笔记更新数量

  // 完整的理论库 - 保持完整性
  const theoriesDatabase = [
    {name: "Ability-Motivation-Opportunity (AMO) Model", aliases: ["AMO model", "AMO theory", "AMO framework"]},
    {name: "Absorptive Capacity Theory", aliases: ["ACAP", "absorptive capacity", "absorptive capacity model"]},
    {name: "Actor-Network Theory", aliases: ["ANT", "actor network theory", "actor-network"]},
    {name: "Translation Sociology", aliases: ["translation theory", "sociology of translation"]},
    {name: "Affective Event Theory", aliases: ["AET", "affective events theory"]},
    {name: "Agency Theory", aliases: ["principal-agent theory", "agency model", "principal agent"]},
    {name: "Agenda-Setting Theory", aliases: ["agenda setting", "agenda-setting model"]},
    {name: "Attachment Theory", aliases: ["attachment style", "attachment styles"]},
    {name: "Attribution Theory", aliases: ["attribution model", "causal attribution"]},
    {name: "Balance Theory", aliases: ["cognitive balance", "balance model"]},
    {name: "Consistency Theory", aliases: ["cognitive consistency", "consistency model"]},
    {name: "Broaden-and-Build Theory", aliases: ["broaden and build", "broaden-and-build model"]},
    {name: "Co-Evolution Theory", aliases: ["coevolution theory", "co-evolutionary theory"]},
    {name: "Cognitive Appraisal Theory", aliases: ["appraisal theory", "cognitive appraisal"]},
    {name: "Cognitive Dissonance Theory", aliases: ["dissonance theory", "cognitive dissonance"]},
    {name: "Compensatory Ethics Theory", aliases: ["compensatory ethics", "moral compensation"]},
    {name: "Complexity Theory and Organizations", aliases: ["complexity theory", "organizational complexity"]},
    {name: "Configural Organizational Theory", aliases: ["configural theory", "configuration theory"]},
    {name: "Conservation of Resource Theory", aliases: ["COR theory", "conservation of resources", "COR"]},
    {name: "Construal Level Theory", aliases: ["CLT", "construal theory", "psychological distance"]},
    {name: "Contingency Theory", aliases: ["situational theory", "contingency approach"]},
    {name: "Control Theory", aliases: ["control systems", "cybernetic control"]},
    {name: "Cybernetic Theory", aliases: ["cybernetics", "cybernetic systems"]},
    {name: "Co-Opetition Theory", aliases: ["coopetition theory", "cooperative competition"]},
    {name: "Diffusion of Innovation Theory", aliases: ["innovation diffusion", "rogers diffusion", "DOI"]},
    {name: "Dual System Theory", aliases: ["dual process theory", "dual-system model"]},
    {name: "Dynamic Capabilities Theory", aliases: ["dynamic capabilities", "DC theory", "dynamic capability"]},
    {name: "Efficient Market Theory", aliases: ["EMH", "efficient market hypothesis", "market efficiency"]},
    {name: "Ego Depletion Theory", aliases: ["ego depletion", "self-control depletion"]},
    {name: "Emotional Contagion Theory", aliases: ["emotional contagion", "emotion contagion"]},
    {name: "Emotions as Social Information Theory", aliases: ["EASI theory", "EASI model"]},
    {name: "Ethical Leadership Theory", aliases: ["ethical leadership", "ethical leader"]},
    {name: "Ethical Theory", aliases: ["ethics theory", "moral theory"]},
    {name: "Event System Theory", aliases: ["EST", "event systems theory"]},
    {name: "Fairness Heuristic Theory", aliases: ["fairness heuristic", "FHT"]},
    {name: "Feelings-as-Information Theory", aliases: ["feelings as information", "affect as information"]},
    {name: "Field Theory", aliases: ["organizational field", "institutional field"]},
    {name: "Game Theory", aliases: ["strategic games", "game theoretic"]},
    {name: "Goal Orientation Theory", aliases: ["achievement goal theory", "goal orientation"]},
    {name: "Goal Setting Theory", aliases: ["goal theory", "locke goal setting", "goal-setting"]},
    {name: "Image Theory", aliases: ["image model", "behavioral decision theory"]},
    {name: "Implicit Leadership Theory", aliases: ["ILT", "implicit leadership", "leadership prototype"]},
    {name: "Institutional Theory", aliases: ["neo-institutional theory", "institutionalism", "institutional"]},
    {name: "Interaction Ritual Chain Theory", aliases: ["IRC theory", "interaction ritual"]},
    {name: "Job Demands-Resources Model", aliases: ["JD-R model", "JDR", "job demands resources"]},
    {name: "Knowledge-Based Theory", aliases: ["KBV", "knowledge based view", "knowledge-based view"]},
    {name: "Leader-Member Exchange (LMX) Theory", aliases: ["LMX", "leader member exchange", "LMX theory"]},
    {name: "Loose Coupling Theory", aliases: ["loose coupling", "loosely coupled systems"]},
    {name: "Media Richness Theory", aliases: ["information richness", "media richness"]},
    {name: "Mental Models Theory", aliases: ["mental models", "shared mental models"]},
    {name: "Middle-Status Conformity Theory", aliases: ["middle status conformity", "middle-status"]},
    {name: "Minority Influence Theory", aliases: ["minority influence", "innovation theory"]},
    {name: "Moral Licensing Theory", aliases: ["moral licensing", "moral credentials"]},
    {name: "Need-to-Belong Theory", aliases: ["need to belong", "belongingness theory"]},
    {name: "Neo-Institutional Theory", aliases: ["institutional theory", "new institutionalism"]},
    {name: "Organizational Ecology Theory", aliases: ["population ecology", "organizational ecology"]},
    {name: "Organizational Justice Theory", aliases: ["organizational justice", "justice theory"]},
    {name: "Organizational Learning Theory", aliases: ["organizational learning", "learning organization"]},
    {name: "Person-Environment Fit Theory", aliases: ["PE fit", "person environment fit", "P-E fit"]},
    {name: "Planned Behavior Theory", aliases: ["TPB", "theory of planned behavior", "planned behaviour"]},
    {name: "Population Ecology Theory", aliases: ["organizational ecology", "population ecology"]},
    {name: "Prospect Theory", aliases: ["prospect model", "kahneman tversky", "loss aversion"]},
    {name: "Psychological Contract Theory", aliases: ["psychological contract", "psychological contracts"]},
    {name: "Regulatory Focus Theory", aliases: ["RFT", "promotion prevention", "regulatory focus"]},
    {name: "Relative Deprivation Theory", aliases: ["relative deprivation", "deprivation theory"]},
    {name: "Resource Allocation Theory", aliases: ["resource allocation", "allocation theory"]},
    {name: "Resource Dependence Theory", aliases: ["RDT", "resource dependence", "resource-dependence"]},
    {name: "Resource-Based View (RBV)", aliases: ["RBV", "resource based view", "resource-based theory"]},
    {name: "Role Congruity Theory", aliases: ["role congruity", "gender role congruity"]},
    {name: "Role Theory", aliases: ["role conflict", "role ambiguity", "organizational roles"]},
    {name: "Self-Categorization Theory", aliases: ["SCT", "self categorization", "social categorization"]},
    {name: "Self-Control Theory", aliases: ["self control", "self-regulation theory"]},
    {name: "Self-Determination Theory", aliases: ["SDT", "self determination", "intrinsic motivation"]},
    {name: "Self-Verification Theory", aliases: ["self verification", "identity verification"]},
    {name: "Sensemaking Theory", aliases: ["sensemaking", "sense making", "organizational sensemaking"]},
    {name: "Servant Leadership Theory", aliases: ["servant leadership", "servant leader"]},
    {name: "Signaling Theory", aliases: ["signal theory", "signalling", "signaling model"]},
    {name: "Social Capital Theory", aliases: ["social capital", "network capital"]},
    {name: "Social Cognitive Theory", aliases: ["SCT", "social learning theory", "social cognitive"]},
    {name: "Social Comparison Theory", aliases: ["social comparison", "comparison theory"]},
    {name: "Social Contagion Theory", aliases: ["social contagion", "behavioral contagion"]},
    {name: "Social Contract Theory", aliases: ["social contract", "contractual theory"]},
    {name: "Social Dominance Orientation Theory", aliases: ["SDO", "social dominance", "dominance orientation"]},
    {name: "Social Exchange Theory", aliases: ["SET", "exchange theory", "social exchange"]},
    {name: "Social Facilitation Theory", aliases: ["social facilitation", "audience effect"]},
    {name: "Social Identity Theory", aliases: ["SIT", "social identity", "group identity"]},
    {name: "Social Information Processing Theory", aliases: ["SIP theory", "social information processing"]},
    {name: "Social Interdependence Theory", aliases: ["interdependence theory", "social interdependence"]},
    {name: "Social Learning Theory", aliases: ["observational learning", "modeling theory"]},
    {name: "Social Network Theory", aliases: ["network theory", "social networks", "network analysis"]},
    {name: "Social Representation Theory", aliases: ["social representations", "representation theory"]},
    {name: "Stakeholder Theory", aliases: ["stakeholder approach", "stakeholder model"]},
    {name: "Stewardship Theory", aliases: ["stewardship model", "stewardship approach"]},
    {name: "Strategic Choice Theory", aliases: ["strategic choice", "strategic decision"]},
    {name: "Structural Adaptation Theory", aliases: ["structural adaptation", "adaptive structuration"]},
    {name: "Structural Contingency Theory", aliases: ["structural contingency", "contingency structure"]},
    {name: "Structuration Theory", aliases: ["giddens structuration", "structuration", "structure theory"]},
    {name: "Tournament Theory", aliases: ["tournament model", "promotion tournament"]},
    {name: "Trait Activation Theory", aliases: ["TAT", "trait activation", "personality activation"]},
    {name: "Transaction Cost Economics", aliases: ["TCE", "transaction cost theory", "TCT", "williamson TCE"]},
    {name: "Transformational Leadership Theory", aliases: ["transformational leadership", "transformational leader"]},
    {name: "Uncertainty Management Theory", aliases: ["uncertainty management", "UMT"]},
    {name: "Uncertainty-Identity Theory", aliases: ["uncertainty identity", "identity uncertainty"]},
    {name: "Upper Echelons Theory", aliases: ["upper echelon", "top management team", "TMT"]},
    {name: "Configurational Theory", aliases: ["configuration theory", "configurational approach"]},
    {name: "Evolutionary Theory", aliases: ["organizational evolution", "evolutionary approach"]},
    {name: "Middle Range Theory", aliases: ["middle-range theory", "merton theory"]},
    {name: "Paradox Theory", aliases: ["organizational paradox", "paradox management"]},
    {name: "Internal Market Efficiency Arguments", aliases: ["Internal market efficiency arguments theory"]},
    {name: "Portfolio Theory", aliases: ["Portfolio theory"]},
    {name: "Market Power Theory", aliases: ["Market power theory"]},
    {name: "Real Options Theory (ROT)", aliases: ["ROT", "real options theory"]},
    {name: "Neoclassical Investment Theory", aliases: ["Neoclassical investment theory"]},
    {name: "Organizational Ambidexterity (OA)", aliases: ["ambidexterity", "OA"]},
    {name: "Network Externalities", aliases: ["network externalities theory", "network externality"]},
    {name: "General Purpose Technology", aliases: ["GPT", "general purpose technology theory"]},
    {name: "Two-Sided Market Theory", aliases: ["two sided market", "platform theory"]},
    {name: "Structural Holes", aliases: ["structural holes theory", "burt structural holes"]},
    {name: "Network Effects", aliases: ["network effects theory", "network effect"]},
    {name: "Network Closure Theory", aliases: ["network closure", "closure theory"]},
    {name: "Sarnoff's Law", aliases: ["sarnoff law", "broadcasting network"]},
    {name: "Metcalfe's Law", aliases: ["metcalfe law", "network value"]},
    {name: "Reed's Law", aliases: ["reed law", "group forming networks"]},
    {name: "Flywheel Effect", aliases: ["flywheel theory", "momentum effect"]},
    {name: "Matthew Effect", aliases: ["matthew effect theory", "cumulative advantage"]},
    {name: "Penrosean Growth Theory", aliases: ["Penrose growth theory", "penrosean theory", "theory of the growth of the firm", "penrose theory", "edith penrose theory"]}
  ];

  // 精确匹配函数 - 采用第三个代码的方法
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

  // 修正：理论关键词检查 - 添加遗漏的逗号
  const triggerKeywords = ["theory", "theories", "theoretical", "framework", "model", "view", "lens", "concept", "理论", "视角", "概念"];

  const hasTheoryKeywords = (text) => {
    const lowerText = text.toLowerCase();
    return triggerKeywords.some(keyword => lowerText.includes(keyword));
  };

  // 检查📒标签
  const hasNotebookTag = (item) => {
    try {
      const tags = item.getTags();
      return tags && tags.some(tag => tag.tag && tag.tag.includes("📒"));
    } catch (e) {
      return false;
    }
  };

  // 安全获取字段
  const safeGetField = (item, fieldName) => {
    try {
      return item.getField(fieldName) || "";
    } catch (e) {
      return "";
    }
  };

  // 获取笔记内容 - 改进错误处理
  const getNoteContent = async (noteItem) => {
    try {
      if (!noteItem || !noteItem.isNote()) return "";
      
      // 优先使用BetterNotes
      if (Zotero.BetterNotes && Zotero.BetterNotes.api && Zotero.BetterNotes.api.convert) {
        return await Zotero.BetterNotes.api.convert.html2md(noteItem.getNote());
      } else {
        // 备用方案：简单HTML清理
        const content = noteItem.getNote() || "";
        return content.replace(/<[^>]*>/g, ' ').replace(/\s+/g, ' ').trim();
      }
    } catch (e) {
      return "";
    }
  };

  // 获取所有笔记内容并合并
  const getAllNotesContent = async (noteIds) => {
    if (!noteIds || noteIds.length === 0) return { content: "", noteCount: 0 };
    
    let allContent = "";
    let noteCount = 0;
    
    for (let noteId of noteIds) {
      try {
        const noteItem = Zotero.Items.get(noteId);
        if (noteItem) {
          const noteContent = await getNoteContent(noteItem);
          if (noteContent && noteContent.trim().length > 20) {
            if (allContent) {
              allContent += "\n\n---[笔记分隔]---\n\n";
            }
            allContent += noteContent.trim();
            noteCount++;
          }
        }
      } catch (e) {
        continue;
      }
    }
    
    return { content: allContent, noteCount: noteCount };
  };

  // 删除现有Theory标签
  const removeExistingTheoryTags = (item) => {
    try {
      const existingTags = item.getTags();
      if (!existingTags) return;
      
      const theoryTags = existingTags.filter(tag => tag.tag && tag.tag.startsWith("Theory/"));
      theoryTags.forEach(tag => item.removeTag(tag.tag));
    } catch (e) {
      console.error("删除标签失败:", e);
    }
  };

  // 优化的保存函数 - 使用数据库事务
  const saveItemWithTag = async (item, tagName, countType) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        item.addTag('Theory/' + tagName);
        await item.save();
      });
      
      // 更新计数器
      switch(countType) {
        case 'success':
          successCount++;
          break;
        case 'noTheory':
          noTheoryCount++;
          break;
        case 'fail':
          failCount++;
          break;
        case 'noAbstract':
          noAbstractCount++;
          break;
        case 'noKeyword':
          noKeywordCount++;
          break;
        case 'update':
          updateCount++;
          successCount++;
          break;
      }
      
      return true;
    } catch (error) {
      console.error('保存标签失败:', error);
      errorCount++;
      return false;
    }
  };

  // 批量保存多个理论标签
  const saveItemWithMultipleTags = async (item, tagNames, isNoteUpdate) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        tagNames.forEach(tag => item.addTag('Theory/' + tag));
        await item.save();
      });
      
      successCount++;
      if (isNoteUpdate) {
        updateCount++;
      }
      
      return true;
    } catch (error) {
      console.error('保存多个标签失败:', error);
      errorCount++;
      return false;
    }
  };

  // 简化AI调用
  const callAI = async (prompt) => {
    try {
      if (!Meet?.Global?.views?.ask) {
        throw new Error("AI接口不可用");
      }
      
      const timeoutPromise = new Promise((_, reject) => 
        setTimeout(() => reject(new Error('AI调用超时')), 30000)
      );
      
      const response = await Promise.race([
        Meet.Global.views.ask(prompt),
        timeoutPromise
      ]);
      
      return response && typeof response === 'string' ? response : null;
    } catch (e) {
      return null;
    }
  };

  // 主处理循环
  for (let item of items) {
    try {
      console.log(`处理条目: ${safeGetField(item, "title")}`);
      
      // 检查📒标签
      if (!hasNotebookTag(item)) {
        skipCount++;
        continue;
      }

      let content = "";
      let isNoteUpdate = false;
      let noteCount = 0;

      // 获取所有笔记内容
      const noteIds = item.getNotes();
      
      if (noteIds && noteIds.length > 0) {
        const notesResult = await getAllNotesContent(noteIds);
        if (notesResult.content && notesResult.content.trim().length > 50) {
          content = notesResult.content;
          noteCount = notesResult.noteCount;
          isNoteUpdate = true;
          removeExistingTheoryTags(item);
        }
      }

      // 如果没有笔记内容，使用摘要作为备选
      if (!content) {
        const abstract = safeGetField(item, "abstractNote");
        if (abstract && abstract.trim().length > 50) {
          content = abstract.trim();
          removeExistingTheoryTags(item);
        }
      }

      if (!content) {
        await saveItemWithTag(item, '无内容', 'noAbstract');
        continue;
      }

      // 关键词检查
      if (!hasTheoryKeywords(content)) {
        await saveItemWithTag(item, '无理论关键词', 'noKeyword');
        continue;
      }

      // AI分析 - 支持多理论识别，不截断内容
      const prompt = `Analyze the literature content and identify all explicitly mentioned theories. ${noteCount > 1 ? `This content includes ${noteCount} separate notes.` : ''}

${content}

Requirements:
1. Only identify clear theory, model, or framework names
2. Use English names for theories (translate Chinese theory names to English if necessary)
3. Return all theories found across all content, no quantity limit
4. Format: ["Theory 1", "Theory 2", "Theory 3", …] or ["No clear theories"]`;

      const response = await callAI(prompt);
      
      if (!response) {
        await saveItemWithTag(item, '解析失败', 'fail');
        continue;
      }

      // 解析AI响应
      try {
        const jsonMatch = response.match(/\[[\s\S]*?\]/);
        if (!jsonMatch) {
          await saveItemWithTag(item, '解析失败', 'fail');
          continue;
        }

        const theories = JSON.parse(jsonMatch[0]);
        
        if (!theories || theories.length === 0 || 
            (theories.length === 1 && theories[0].toLowerCase().includes("no clear"))) {
          await saveItemWithTag(item, '无明确理论', 'noTheory');
          continue;
        }

        // 处理所有识别到的理论
        const tagsToAdd = [];
        for (let theory of theories) {
          if (theory && theory.trim() && !theory.toLowerCase().includes("no clear")) {
            const theoryName = theory.trim();
            const foundTheory = findTheory(theoryName);
            
            if (foundTheory) {
              tagsToAdd.push(foundTheory);
            } else {
              tagsToAdd.push(theoryName + '-new');
            }
          }
        }

        if (tagsToAdd.length > 0) {
          await saveItemWithMultipleTags(item, tagsToAdd, isNoteUpdate);
        } else {
          await saveItemWithTag(item, '无有效理论', 'noTheory');
        }

      } catch (parseError) {
        await saveItemWithTag(item, '解析失败', 'fail');
      }

      // 条目间停顿
      await new Promise(resolve => setTimeout(resolve, 200));

    } catch (itemError) {
      console.error("处理条目失败:", itemError);
      errorCount++;
    }
  }

  return `理论标签处理完成 - 成功: ${successCount}, 无理论: ${noTheoryCount}, 跳过: ${skipCount}, 失败: ${failCount}, 无内容: ${noAbstractCount}, 无关键词: ${noKeywordCount}, 保存失败: ${errorCount}, 笔记更新: ${updateCount}`;
})()
}$
```

---
