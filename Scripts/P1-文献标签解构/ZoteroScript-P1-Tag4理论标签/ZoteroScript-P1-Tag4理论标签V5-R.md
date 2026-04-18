---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag4理论标签V5-R
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
- AI
- JavaScript
- Zotero
- 代码
- 学术研究
- 文献分析
- 理论框架
- 理论识别
- 知识管理
- 自动化标签
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P1-Tag4 理论标签 V5-R

```javascript
#🏷️R-TheoryTag2[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";

  let successCount = 0;         // 成功标注到理论
  let noTheoryCount = 0;        // AI分析后没有理论
  let failCount = 0;            // 解析失败
  let noAbstractCount = 0;      // 没有摘要
  let noKeywordCount = 0;       // 无理论关键词
  let errorCount = 0;           // 保存失败

  // 修复的保存函数
  const saveItemWithTags = async (item, tags, countType) => {
    try {
      // 确保在事务中保存
      await Zotero.DB.executeTransaction(async function () {
        for (let tag of tags) {
          item.addTag(tag);
        }
        await item.save();
      });
      
      // 更新对应的计数器
      switch(countType) {
        case 'success':
          successCount++;
          break;
        case 'noTheory':
          noTheoryCount++;
          break;
        case 'noContent':
          noAbstractCount++;
          break;
        case 'noKeyword':
          noKeywordCount++;
          break;
        case 'fail':
          failCount++;
          break;
      }
      
      return true;
    } catch (error) {
      console.error('保存标签失败:', error);
      errorCount++;
      return false;
    }
  };

  // 删除现有Theory/标签的函数
  const removeExistingTheoryTags = async (item) => {
    const existingTags = item.getTags();
    const theoryTags = existingTags.filter(tag => tag.tag && tag.tag.startsWith("Theory/"));
    for (let theoryTag of theoryTags) {
      item.removeTag(theoryTag.tag);
    }
  };

  // 完整的理论库 - 保持完整性
  const theoriesDatabase = [
    {name: "(能力-动机-机会模型)Ability-Motivation-Opportunity (AMO) Model", aliases: ["Ability-Motivation-Opportunity (AMO) Model", "AMO model", "AMO theory", "AMO framework"]},
    {name: "(吸收能力理论)Absorptive Capacity Theory", aliases: ["Absorptive Capacity Theory", "ACAP", "absorptive capacity", "absorptive capacity model"]},
    {name: "(行动者网络理论)Actor-Network Theory", aliases: ["Actor-Network Theory", "ANT", "actor network theory", "actor-network"]},
    {name: "(转译社会学)Translation Sociology", aliases: ["Translation Sociology", "translation theory", "sociology of translation"]},
    {name: "(情感事件理论)Affective Event Theory", aliases: ["Affective Event Theory", "AET", "affective events theory"]},
    {name: "(代理理论)Agency Theory", aliases: ["Agency Theory", "principal-agent theory", "agency model", "principal agent"]},
    {name: "(议程设置理论)Agenda-Setting Theory", aliases: ["Agenda-Setting Theory", "agenda setting", "agenda-setting model"]},
    {name: "(依恋理论)Attachment Theory", aliases: ["Attachment Theory", "attachment style", "attachment styles"]},
    {name: "(归因理论)Attribution Theory", aliases: ["Attribution Theory", "attribution model", "causal attribution"]},
    {name: "(平衡理论)Balance Theory", aliases: ["Balance Theory", "cognitive balance", "balance model"]},
    {name: "(一致性理论)Consistency Theory", aliases: ["Consistency Theory", "cognitive consistency", "consistency model"]},
    {name: "(拓展-建构理论)Broaden-and-Build Theory", aliases: ["Broaden-and-Build Theory", "broaden and build", "broaden-and-build model"]},
    {name: "(共同演化理论)Co-Evolution Theory", aliases: ["Co-Evolution Theory", "coevolution theory", "co-evolutionary theory"]},
    {name: "(认知评估理论)Cognitive Appraisal Theory", aliases: ["Cognitive Appraisal Theory", "appraisal theory", "cognitive appraisal"]},
    {name: "(认知失调理论)Cognitive Dissonance Theory", aliases: ["Cognitive Dissonance Theory", "dissonance theory", "cognitive dissonance"]},
    {name: "(补偿道德理论)Compensatory Ethics Theory", aliases: ["Compensatory Ethics Theory", "compensatory ethics", "moral compensation"]},
    {name: "(复杂性理论)Complexity Theory and Organizations", aliases: ["Complexity Theory and Organizations", "complexity theory", "organizational complexity"]},
    {name: "(组态理论)Configural Organizational Theory", aliases: ["Configural Organizational Theory", "configural theory", "configuration theory"]},
    {name: "(资源保存理论)Conservation of Resource Theory", aliases: ["Conservation of Resource Theory", "COR theory", "conservation of resources", "COR"]},
    {name: "(解释水平理论)Construal Level Theory", aliases: ["Construal Level Theory", "CLT", "construal theory", "psychological distance"]},
    {name: "(权变理论)Contingency Theory", aliases: ["Contingency Theory", "situational theory", "contingency approach"]},
    {name: "(控制理论)Control Theory", aliases: ["Control Theory", "cybernetic theory", "cybernetic control"]},
    {name: "(控制论)Cybernetic Theory", aliases: ["Cybernetic Theory", "cybernetics", "control theory"]},
    {name: "(竞合理论)Co-Opetition Theory", aliases: ["Co-Opetition Theory", "coopetition theory", "cooperative competition"]},
    {name: "(创新扩散理论)Diffusion of Innovation Theory", aliases: ["Diffusion of Innovation Theory", "innovation diffusion", "rogers diffusion", "DOI"]},
    {name: "(双系统理论)Dual System Theory", aliases: ["Dual System Theory", "dual process theory", "dual-system model"]},
    {name: "(动态能力理论)Dynamic Capabilities Theory", aliases: ["Dynamic Capabilities Theory", "dynamic capabilities", "DC theory", "dynamic capability"]},
    {name: "(有效市场理论)Efficient Market Theory", aliases: ["Efficient Market Theory", "EMH", "efficient market hypothesis", "market efficiency"]},
    {name: "(自我损耗理论)Ego Depletion Theory", aliases: ["Ego Depletion Theory", "ego depletion", "self-control depletion"]},
    {name: "(情绪传染理论)Emotional Contagion Theory", aliases: ["Emotional Contagion Theory", "emotional contagion", "emotion contagion"]},
    {name: "(情绪社会信息理论)Emotions as Social Information Theory", aliases: ["Emotions as Social Information Theory", "EASI theory", "EASI model"]},
    {name: "(道德领导理论)Ethical Leadership Theory", aliases: ["Ethical Leadership Theory", "ethical leadership", "ethical leader"]},
    {name: "(伦理学理论)Ethical Theory", aliases: ["Ethical Theory", "ethics theory", "moral theory"]},
    {name: "(事件系统理论)Event System Theory", aliases: ["Event System Theory", "EST", "event systems theory"]},
    {name: "(公平启发式理论)Fairness Heuristic Theory", aliases: ["Fairness Heuristic Theory", "fairness heuristic", "FHT"]},
    {name: "(情感即信息理论)Feelings-as-Information Theory", aliases: ["Feelings-as-Information Theory", "feelings as information", "affect as information"]},
    {name: "(场域理论)Field Theory", aliases: ["Field Theory", "organizational field", "institutional field"]},
    {name: "(博弈论)Game Theory", aliases: ["Game Theory", "strategic games", "game theoretic"]},
    {name: "(目标导向理论)Goal Orientation Theory", aliases: ["Goal Orientation Theory", "achievement goal theory", "goal orientation"]},
    {name: "(目标设定理论)Goal Setting Theory", aliases: ["Goal Setting Theory", "goal theory", "locke goal setting", "goal-setting"]},
    {name: "(形象理论)Image Theory", aliases: ["Image Theory", "image model", "behavioral decision theory"]},
    {name: "(内隐领导理论)Implicit Leadership Theory", aliases: ["Implicit Leadership Theory", "ILT", "implicit leadership", "leadership prototype"]},
    {name: "(制度理论)Institutional Theory", aliases: ["Institutional Theory", "neo-institutional theory", "institutionalism", "institutional"]},
    {name: "(互动仪式链理论)Interaction Ritual Chain Theory", aliases: ["Interaction Ritual Chain Theory", "IRC theory", "interaction ritual"]},
    {name: "(工作要求-资源模型)Job Demands-Resources Model", aliases: ["Job Demands-Resources Model", "JD-R model", "JDR", "job demands resources"]},
    {name: "(知识基础观)Knowledge-Based Theory", aliases: ["Knowledge-Based Theory", "KBV", "knowledge based view", "knowledge-based view"]},
    {name: "(领导-成员交换理论)Leader-Member Exchange (LMX) Theory", aliases: ["Leader-Member Exchange (LMX) Theory", "LMX", "leader member exchange", "LMX theory"]},
    {name: "(松散耦合理论)Loose Coupling Theory", aliases: ["Loose Coupling Theory", "loose coupling", "loosely coupled systems"]},
    {name: "(媒介丰富度理论)Media Richness Theory", aliases: ["Media Richness Theory", "information richness", "media richness"]},
    {name: "(心智模型理论)Mental Models Theory", aliases: ["Mental Models Theory", "mental models", "shared mental models"]},
    {name: "(中等地位从众理论)Middle-Status Conformity Theory", aliases: ["Middle-Status Conformity Theory", "middle status conformity", "middle-status"]},
    {name: "(少数派影响理论)Minority Influence Theory", aliases: ["Minority Influence Theory", "minority influence", "innovation theory"]},
    {name: "(道德许可理论)Moral Licensing Theory", aliases: ["Moral Licensing Theory", "moral licensing", "moral credentials"]},
    {name: "(归属需求理论)Need-to-Belong Theory", aliases: ["Need-to-Belong Theory", "need to belong", "belongingness theory"]},
    {name: "(新制度理论)Neo-Institutional Theory", aliases: ["Neo-Institutional Theory", "institutional theory", "new institutionalism"]},
    {name: "(组织生态理论)Organizational Ecology Theory", aliases: ["Organizational Ecology Theory", "population ecology", "organizational ecology"]},
    {name: "(组织公正理论)Organizational Justice Theory", aliases: ["Organizational Justice Theory", "organizational justice", "justice theory"]},
    {name: "(组织学习理论)Organizational Learning Theory", aliases: ["Organizational Learning Theory", "organizational learning", "learning organization"]},
    {name: "(人-环境匹配理论)Person-Environment Fit Theory", aliases: ["Person-Environment Fit Theory", "PE fit", "person environment fit", "P-E fit"]},
    {name: "(计划行为理论)Planned Behavior Theory", aliases: ["Planned Behavior Theory", "TPB", "theory of planned behavior", "planned behaviour"]},
    {name: "(种群生态理论)Population Ecology Theory", aliases: ["Population Ecology Theory", "organizational ecology", "population ecology"]},
    {name: "(前景理论)Prospect Theory", aliases: ["Prospect Theory", "prospect model", "kahneman tversky", "loss aversion"]},
    {name: "(心理契约理论)Psychological Contract Theory", aliases: ["Psychological Contract Theory", "psychological contract", "psychological contracts"]},
    {name: "(调节聚焦理论)Regulatory Focus Theory", aliases: ["Regulatory Focus Theory", "RFT", "promotion prevention", "regulatory focus"]},
    {name: "(相对剥夺理论)Relative Deprivation Theory", aliases: ["Relative Deprivation Theory", "relative deprivation", "deprivation theory"]},
    {name: "(资源配置理论)Resource Allocation Theory", aliases: ["Resource Allocation Theory", "resource allocation", "allocation theory"]},
    {name: "(资源依赖理论)Resource Dependence Theory", aliases: ["Resource Dependence Theory", "RDT", "resource dependence", "resource-dependence"]},
    {name: "(资源基础观)Resource-Based View (RBV)", aliases: ["Resource-Based View (RBV)", "RBV", "resource based view", "resource-based theory"]},
    {name: "(角色一致性理论)Role Congruity Theory", aliases: ["Role Congruity Theory", "role congruity", "gender role congruity"]},
    {name: "(角色理论)Role Theory", aliases: ["Role Theory", "role conflict", "role ambiguity", "organizational roles"]},
    {name: "(自我分类理论)Self-Categorization Theory", aliases: ["Self-Categorization Theory", "SCT", "self categorization", "social categorization"]},
    {name: "(自我控制理论)Self-Control Theory", aliases: ["Self-Control Theory", "self control", "self-regulation theory"]},
    {name: "(自我决定理论)Self-Determination Theory", aliases: ["Self-Determination Theory", "SDT", "self determination", "intrinsic motivation"]},
    {name: "(自我验证理论)Self-Verification Theory", aliases: ["Self-Verification Theory", "self verification", "identity verification"]},
    {name: "(意义建构理论)Sensemaking Theory", aliases: ["Sensemaking Theory", "sensemaking", "sense making", "organizational sensemaking"]},
    {name: "(服务型领导理论)Servant Leadership Theory", aliases: ["Servant Leadership Theory", "servant leadership", "servant leader"]},
    {name: "(信号理论)Signaling Theory", aliases: ["Signaling Theory", "signal theory", "signalling", "signaling model"]},
    {name: "(社会资本理论)Social Capital Theory", aliases: ["Social Capital Theory", "social capital", "network capital"]},
    {name: "(社会认知理论)Social Cognitive Theory", aliases: ["Social Cognitive Theory", "SCT", "social learning theory", "social cognitive"]},
    {name: "(社会比较理论)Social Comparison Theory", aliases: ["Social Comparison Theory", "social comparison", "comparison theory"]},
    {name: "(社会传染理论)Social Contagion Theory", aliases: ["Social Contagion Theory", "social contagion", "behavioral contagion"]},
    {name: "(社会契约论)Social Contract Theory", aliases: ["Social Contract Theory", "social contract", "contractual theory"]},
    {name: "(社会支配倾向理论)Social Dominance Orientation Theory", aliases: ["Social Dominance Orientation Theory", "SDO", "social dominance", "dominance orientation"]},
    {name: "(社会交换理论)Social Exchange Theory", aliases: ["Social Exchange Theory", "SET", "exchange theory", "social exchange"]},
    {name: "(社会促进理论)Social Facilitation Theory", aliases: ["Social Facilitation Theory", "social facilitation", "audience effect"]},
    {name: "(社会认同理论)Social Identity Theory", aliases: ["Social Identity Theory", "SIT", "social identity", "group identity"]},
    {name: "(社会信息加工理论)Social Information Processing Theory", aliases: ["Social Information Processing Theory", "SIP theory", "social information processing"]},
    {name: "(社会互赖理论)Social Interdependence Theory", aliases: ["Social Interdependence Theory", "interdependence theory", "social interdependence"]},
    {name: "(社会学习理论)Social Learning Theory", aliases: ["Social Learning Theory", "observational learning", "modeling theory"]},
    {name: "(社会网络理论)Social Network Theory", aliases: ["Social Network Theory", "network theory", "social networks", "network analysis"]},
    {name: "(社会表征理论)Social Representation Theory", aliases: ["Social Representation Theory", "social representations", "representation theory"]},
    {name: "(利益相关者理论)Stakeholder Theory", aliases: ["Stakeholder Theory", "stakeholder approach", "stakeholder model"]},
    {name: "(管家理论)Stewardship Theory", aliases: ["Stewardship Theory", "stewardship model", "stewardship approach"]},
    {name: "(战略选择理论)Strategic Choice Theory", aliases: ["Strategic Choice Theory", "strategic choice", "strategic decision"]},
    {name: "(结构适应理论)Structural Adaptation Theory", aliases: ["Structural Adaptation Theory", "structural adaptation", "adaptive structuration"]},
    {name: "(结构权变理论)Structural Contingency Theory", aliases: ["Structural Contingency Theory", "structural contingency", "contingency structure"]},
    {name: "(结构化理论)Structuration Theory", aliases: ["Structuration Theory", "giddens structuration", "structuration", "structure theory"]},
    {name: "(锦标赛理论)Tournament Theory", aliases: ["Tournament Theory", "tournament model", "promotion tournament"]},
    {name: "(特质激活理论)Trait Activation Theory", aliases: ["Trait Activation Theory", "TAT", "trait activation", "personality activation"]},
    {name: "(交易成本理论)Transaction Cost Theory", aliases: ["Transaction Cost Theory", "TCT", "transaction cost economics", "TCE"]},
    {name: "(变革型领导理论)Transformational Leadership Theory", aliases: ["Transformational Leadership Theory", "transformational leadership", "transformational leader"]},
    {name: "(不确定性管理理论)Uncertainty Management Theory", aliases: ["Uncertainty Management Theory", "uncertainty management", "UMT"]},
    {name: "(不确定性-认同理论)Uncertainty-Identity Theory", aliases: ["Uncertainty-Identity Theory", "uncertainty identity", "identity uncertainty"]},
    {name: "(高阶理论)Upper Echelons Theory", aliases: ["Upper Echelons Theory", "upper echelon", "top management team", "TMT"]},
    {name: "(组态理论)Configurational Theory", aliases: ["Configurational Theory", "configuration theory", "configurational approach"]},
    {name: "(演化理论)Evolutionary Theory", aliases: ["Evolutionary Theory", "organizational evolution", "evolutionary approach"]},
    {name: "(中层理论)Middle Range Theory", aliases: ["Middle Range Theory", "middle-range theory", "merton theory"]},
    {name: "(悖论理论)Paradox Theory", aliases: ["Paradox Theory", "organizational paradox", "paradox management"]},
    {name: "(内部市场效率论)Internal Market Efficiency Arguments", aliases: ["Internal Market Efficiency Arguments", "Internal market efficiency arguments theory"]},
    {name: "(投资组合理论)Portfolio Theory", aliases: ["Portfolio Theory", "Portfolio theory"]},
    {name: "(市场力量理论)Market Power Theory", aliases: ["Market Power Theory", "Market power theory"]},
    {name: "(实物期权理论)Real Options Theory (ROT)", aliases: ["Real Options Theory (ROT)", "ROT", "real options theory"]},
    {name: "(新古典投资理论)Neoclassical Investment Theory", aliases: ["Neoclassical Investment Theory", "Neoclassical investment theory"]},
    {name: "(组织双元性)Organizational Ambidexterity (OA)", aliases: ["Organizational Ambidexterity (OA)", "ambidexterity", "OA"]},
    {name: "(网络外部性)Network Externalities", aliases: ["Network Externalities", "network externalities theory", "network externality"]},
    {name: "(通用技术)General Purpose Technology", aliases: ["General Purpose Technology", "GPT", "general purpose technology theory"]},
    {name: "(交易成本经济学)Transaction Cost Economics", aliases: ["Transaction Cost Economics", "TCE", "transaction cost economics theory"]},
    {name: "(双边市场理论)Two-Sided Market Theory", aliases: ["Two-Sided Market Theory", "two sided market", "platform theory"]},
    {name: "(结构洞理论)Structural Holes", aliases: ["Structural Holes", "structural holes theory", "burt structural holes"]},
    {name: "(网络效应)Network Effects", aliases: ["Network Effects", "network effects theory", "network effect"]},
    {name: "(网络闭合理论)Network Closure Theory", aliases: ["Network Closure Theory", "network closure", "closure theory"]},
    {name: "(萨诺夫定律)Sarnoff's Law", aliases: ["Sarnoff's Law", "sarnoff law", "broadcasting network"]},
    {name: "(梅特卡夫定律)Metcalfe's Law", aliases: ["Metcalfe's Law", "metcalfe law", "network value"]},
    {name: "(里德定律)Reed's Law", aliases: ["Reed's Law", "reed law", "group forming networks"]},
    {name: "(飞轮效应)Flywheel Effect", aliases: ["Flywheel Effect", "flywheel theory", "momentum effect"]},
    {name: "(马太效应)Matthew Effect", aliases: ["Matthew Effect", "matthew effect theory", "cumulative advantage"]}
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
      // 删除原有Theory/标签
      await removeExistingTheoryTags(item);

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
        await saveItemWithTags(item, ['Theory/无内容'], 'noContent');
        continue;
      }

      // 快速关键词检查
      if (!hasTheoryKeywords(content)) {
        // 无理论关键词，直接标注
        await saveItemWithTags(item, ['Theory/无理论关键词'], 'noKeyword');
        continue;
      }

      // AI分析 - 修改prompt
      try {
        const prompt = `Analyze the following academic literature content and identify 1-3 theoretical frameworks explicitly mentioned:

${content}

Requirements:
1. Extract ONLY explicit theory/model/framework names mentioned in the text
2. Return English names ONLY (maximum 50 characters per theory)
3. Use precise, standard academic terminology (e.g., "Social Exchange Theory", "Resource-Based View")
4. Avoid descriptive phrases or paragraphs - only theory names
5. Do NOT force non-theory content - only include genuine theoretical frameworks
6. Return 1-3 theories based on relevance and explicitness
7. Order by relevance: most important theory first
8. Format: Return a JSON array like ["Theory Name 1", "Theory Name 2", "Theory Name 3"]
9. If no clear theory found, return ["No explicit theory"]

Example valid outputs:
["Social Identity Theory"]
["Resource-Based View", "Dynamic Capabilities Theory"]
["Transaction Cost Economics", "Agency Theory", "Institutional Theory"]

Return JSON array only:`;

        const response = await Meet.Global.views.ask(prompt);

        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            await saveItemWithTags(item, ['Theory/解析失败'], 'fail');
            continue;
          }

          const theories = JSON.parse(jsonMatch[0]);

          // 验证和清理理论名称
          const validTheories = theories
            .filter(theory => typeof theory === 'string' && theory.trim().length > 0 && theory.trim().length <= 50)
            .map(theory => theory.trim())
            .slice(0, 3); // 确保最多3个

          if (validTheories.length === 0 || validTheories[0] === "No explicit theory") {
            // 无明确理论
            await saveItemWithTags(item, ['Theory/无明确理论'], 'noTheory');
            continue;
          }

          // 准备要添加的标签 - 按序号排序
          const tagsToAdd = [];
          for (let i = 0; i < validTheories.length; i++) {
            const identifiedTheory = validTheories[i];
            const foundTheory = findTheory(identifiedTheory);
            
            let tagToAdd = "";
            if (foundTheory) {
              tagToAdd = foundTheory;
            } else {
              tagToAdd = identifiedTheory + '-new';
            }
            
            // 添加序号标签
            tagsToAdd.push('Theory/' + (i + 1) + tagToAdd);
          }

          // 使用统一的保存函数
          await saveItemWithTags(item, tagsToAdd, 'success');

        } catch (parseError) {
          // JSON解析失败
          console.error('JSON解析失败:', parseError);
          await saveItemWithTags(item, ['Theory/JSON解析失败'], 'fail');
        }

      } catch (aiError) {
        // AI调用失败
        console.error('AI调用失败:', aiError);
        await saveItemWithTags(item, ['Theory/AI调用失败'], 'fail');
      }

    } catch (itemError) {
      console.error('处理条目失败:', itemError);
      errorCount++;
    }
  }

  return `理论标签重新处理完成 - 成功: ${successCount}, 无理论: ${noTheoryCount}, 失败: ${failCount}, 无内容: ${noAbstractCount}, 无关键词: ${noKeywordCount}, 保存失败: ${errorCount}`;
})()
}$
```
