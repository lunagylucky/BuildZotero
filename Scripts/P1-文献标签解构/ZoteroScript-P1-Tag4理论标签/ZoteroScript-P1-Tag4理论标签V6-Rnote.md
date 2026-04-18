---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag4理论标签V6-Rnote
DateCreated: 2026-01-17 17:37
DateModified: 2026-04-18 17:38
Type:
  - doc
Status:
  - doing
Version: v1.0
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
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P1-Tag4 理论标签 V6-Rnote

```javascript
#🏷️Rnote-TheoryTag3[color=#66dc79][trigger=]
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
     // 完整的理论库 - 标准化大小写和新增理论
const theoriesDatabase = [
    {name: "(能力-动机-机会模型)Ability-Motivation-Opportunity (AMO) Model", aliases: ["Ability-Motivation-Opportunity (AMO) Model", "AMO model", "AMO theory", "AMO framework", "Strategic Human Resource Management", "Strategic Human Resource Management (SHRM)", "High-Performance Work Systems (HPWS)", "Ability-Motivation-Opportunity Framework", "High Commitment Work Systems"]},
    {name: "(吸收能力理论)Absorptive Capacity Theory", aliases: ["Absorptive Capacity Theory", "ACAP", "absorptive capacity", "absorptive capacity model"]},
    {name: "(行动者网络理论)Actor-Network Theory", aliases: ["Actor-Network Theory", "ANT", "actor network theory", "actor-network"]},
    {name: "(转译社会学)Translation Sociology", aliases: ["Translation Sociology", "translation theory", "sociology of translation"]},
    {name: "(情感事件理论)Affective Event Theory", aliases: ["Affective Event Theory", "AET", "affective events theory"]},
    {name: "(代理理论)Agency Theory", aliases: ["Agency Theory", "principal-agent theory", "agency model", "principal agent", "Moral Hazard"]},
    {name: "(议程设置理论)Agenda-Setting Theory", aliases: ["Agenda-Setting Theory", "agenda setting", "agenda-setting model"]},
    {name: "(依恋理论)Attachment Theory", aliases: ["Attachment Theory", "attachment style", "attachment styles"]},
    {name: "(归因理论)Attribution Theory", aliases: ["Attribution Theory", "attribution model", "causal attribution"]},
    {name: "(平衡理论)Balance Theory", aliases: ["Balance Theory", "cognitive balance", "balance model"]},
    {name: "(一致性理论)Consistency Theory", aliases: ["Consistency Theory", "cognitive consistency", "consistency model"]},
    {name: "(拓展-建构理论)Broaden-and-Build Theory", aliases: ["Broaden-and-Build Theory", "broaden and build", "broaden-and-build model"]},
    {name: "(共同演化理论)Co-Evolution Theory", aliases: ["Co-Evolution Theory", "coevolution theory", "co-evolutionary theory"]},
    {name: "(认知评估理论)Cognitive Appraisal Theory", aliases: ["Cognitive Appraisal Theory", "appraisal theory", "cognitive appraisal", "Cognitive Appraisal Theory of Stress"]},
    {name: "(认知失调理论)Cognitive Dissonance Theory", aliases: ["Cognitive Dissonance Theory", "dissonance theory", "cognitive dissonance"]},
    {name: "(补偿道德理论)Compensatory Ethics Theory", aliases: ["Compensatory Ethics Theory", "compensatory ethics", "moral compensation"]},
    {name: "(复杂性理论)Complexity Theory and Organizations", aliases: ["Complexity Theory and Organizations", "complexity theory", "organizational complexity"]},
    {name: "(组态理论)Configural Organizational Theory", aliases: ["Configural Organizational Theory", "configural theory", "configuration theory"]},
    {name: "(资源保存理论)Conservation of Resource Theory", aliases: ["Conservation of Resource Theory", "COR theory", "conservation of resources", "COR"]},
    {name: "(解释水平理论)Construal Level Theory", aliases: ["Construal Level Theory", "CLT", "construal theory", "psychological distance"]},
    {name: "(权变理论)Contingency Theory", aliases: ["Contingency Theory", "situational theory", "contingency approach"]},
    {name: "(控制理论)Control Theory", aliases: ["Control Theory", "Cybernetic Theory", "cybernetic theory", "cybernetic control", "control theory"]},
    {name: "(竞合理论)Co-Opetition Theory", aliases: ["Co-Opetition Theory", "coopetition theory", "cooperative competition"]},
    {name: "(创新扩散理论)Diffusion of Innovation Theory", aliases: ["Diffusion of Innovation Theory", "Diffusion of Innovations", "innovation diffusion", "rogers diffusion", "DOI"]},
    {name: "(双系统理论)Dual System Theory", aliases: ["Dual System Theory", "dual process theory", "dual-system model", "Dual-process theory"]},
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
    {name: "(制度理论)Institutional Theory", aliases: ["Institutional Theory", "neo-institutional theory", "institutionalism", "institutional", "Institution-Based Trust Theory", "Institution-Based View", "Institution Theory", "Decoupling Theory", "North's Institutions Theory"]},
    {name: "(互动仪式链理论)Interaction Ritual Chain Theory", aliases: ["Interaction Ritual Chain Theory", "IRC theory", "interaction ritual"]},
    {name: "(工作要求-资源模型)Job Demands-Resources Model", aliases: ["Job Demands-Resources Model", "JD-R model", "JDR", "job demands resources", "Challenge–Hindrance Stressor Framework"]},
    {name: "(知识基础观)Knowledge-Based Theory", aliases: ["Knowledge-Based Theory", "KBV", "knowledge based view", "knowledge-based view", "Knowledge Search View"]},
    {name: "(领导-成员交换理论)Leader-Member Exchange (LMX) Theory", aliases: ["Leader-Member Exchange (LMX) Theory", "LMX", "leader member exchange", "LMX theory"]},
    {name: "(松散耦合理论)Loose Coupling Theory", aliases: ["Loose Coupling Theory", "loose coupling", "loosely coupled systems"]},
    {name: "(媒介丰富度理论)Media Richness Theory", aliases: ["Media Richness Theory", "information richness", "media richness"]},
    {name: "(心智模型理论)Mental Models Theory", aliases: ["Mental Models Theory", "mental models", "shared mental models"]},
    {name: "(中等地位从众理论)Middle-Status Conformity Theory", aliases: ["Middle-Status Conformity Theory", "middle status conformity", "middle-status"]},
    {name: "(少数派影响理论)Minority Influence Theory", aliases: ["Minority Influence Theory", "minority influence", "innovation theory"]},
    {name: "(道德许可理论)Moral Licensing Theory", aliases: ["Moral Licensing Theory", "moral licensing", "moral credentials"]},
    {name: "(归属需求理论)Need-to-Belong Theory", aliases: ["Need-to-Belong Theory", "need to belong", "belongingness theory"]},
    {name: "(新制度理论)Neo-Institutional Theory", aliases: ["Neo-Institutional Theory", "institutional theory", "new institutionalism"]},
    {name: "(组织生态理论)Organizational Ecology Theory", aliases: ["Organizational Ecology Theory", "Population Ecology Theory", "population ecology", "organizational ecology"]},
    {name: "(组织公正理论)Organizational Justice Theory", aliases: ["Organizational Justice Theory", "organizational justice", "justice theory"]},
    {name: "(组织学习理论)Organizational Learning Theory", aliases: ["Organizational Learning Theory", "organizational learning", "learning organization", "Organizational Unlearning"]},
    {name: "(人-环境匹配理论)Person-Environment Fit Theory", aliases: ["Person-Environment Fit Theory", "PE fit", "person environment fit", "P-E fit"]},
    {name: "(计划行为理论)Planned Behavior Theory", aliases: ["Planned Behavior Theory", "TPB", "theory of planned behavior", "planned behaviour"]},
    {name: "(前景理论)Prospect Theory", aliases: ["Prospect Theory", "prospect model", "kahneman tversky", "loss aversion", "Mixed Gamble Theory"]},
    {name: "(心理契约理论)Psychological Contract Theory", aliases: ["Psychological Contract Theory", "psychological contract", "psychological contracts"]},
    {name: "(调节聚焦理论)Regulatory Focus Theory", aliases: ["Regulatory Focus Theory", "RFT", "promotion prevention", "regulatory focus"]},
    {name: "(相对剥夺理论)Relative Deprivation Theory", aliases: ["Relative Deprivation Theory", "relative deprivation", "deprivation theory"]},
    {name: "(资源配置理论)Resource Allocation Theory", aliases: ["Resource Allocation Theory", "resource allocation", "allocation theory"]},
    {name: "(资源依赖理论)Resource Dependence Theory", aliases: ["Resource Dependence Theory", "RDT", "resource dependence", "resource-dependence"]},
    {name: "(资源基础观)Resource-Based View (RBV)", aliases: ["Resource-Based View (RBV)", "RBV", "resource based view", "resource-based theory", "Operational Slack Theory", "Slack resources theory", "Slack Resources Theory", "Organizational Slack", "Resource Management View", "Ricardian rents", "Monopoly rents", "Financial Slack Theory"]},
    {name: "(角色一致性理论)Role Congruity Theory", aliases: ["Role Congruity Theory", "role congruity", "gender role congruity"]},
    {name: "(角色理论)Role Theory", aliases: ["Role Theory", "role conflict", "role ambiguity", "organizational roles"]},
    {name: "(自我分类理论)Self-Categorization Theory", aliases: ["Self-Categorization Theory", "self categorization", "social categorization"]},
    {name: "(自我控制理论)Self-Control Theory", aliases: ["Self-Control Theory", "self control", "self-regulation theory"]},
    {name: "(自我决定理论)Self-Determination Theory", aliases: ["Self-Determination Theory", "SDT", "self determination", "intrinsic motivation"]},
    {name: "(自我验证理论)Self-Verification Theory", aliases: ["Self-Verification Theory", "self verification", "identity verification"]},
    {name: "(意义建构理论)Sensemaking Theory", aliases: ["Sensemaking Theory", "sensemaking", "sense making", "organizational sensemaking"]},
    {name: "(服务型领导理论)Servant Leadership Theory", aliases: ["Servant Leadership Theory", "servant leadership", "servant leader"]},
    {name: "(信号理论)Signaling Theory", aliases: ["Signaling Theory", "signal theory", "signalling", "signaling model"]},
    {name: "(社会资本理论)Social Capital Theory", aliases: ["Social Capital Theory", "social capital", "network capital"]},
    {name: "(社会认知理论)Social Cognitive Theory", aliases: ["Social Cognitive Theory", "SCT", "social learning theory", "social cognitive", "Reciprocal Determinism Theory"]},
    {name: "(社会比较理论)Social Comparison Theory", aliases: ["Social Comparison Theory", "social comparison", "comparison theory"]},
    {name: "(社会传染理论)Social Contagion Theory", aliases: ["Social Contagion Theory", "social contagion", "behavioral contagion"]},
    {name: "(社会契约论)Social Contract Theory", aliases: ["Social Contract Theory", "social contract", "contractual theory"]},
    {name: "(社会支配倾向理论)Social Dominance Orientation Theory", aliases: ["Social Dominance Orientation Theory", "SDO", "social dominance", "dominance orientation"]},
    {name: "(社会交换理论)Social Exchange Theory", aliases: ["Social Exchange Theory", "SET", "exchange theory", "social exchange", "Reciprocity Theory"]},
    {name: "(社会促进理论)Social Facilitation Theory", aliases: ["Social Facilitation Theory", "social facilitation", "audience effect"]},
    {name: "(社会认同理论)Social Identity Theory", aliases: ["Social Identity Theory", "SIT", "social identity", "group identity"]},
    {name: "(社会信息加工理论)Social Information Processing Theory", aliases: ["Social Information Processing Theory", "SIP theory", "social information processing"]},
    {name: "(社会互赖理论)Social Interdependence Theory", aliases: ["Social Interdependence Theory", "interdependence theory", "social interdependence"]},
    {name: "(社会学习理论)Social Learning Theory", aliases: ["Social Learning Theory", "observational learning", "modeling theory"]},
    {name: "(社会网络理论)Social Network Theory", aliases: ["Social Network Theory", "network theory", "social networks", "network analysis", "Logic of Embeddedness", "Network embeddedness", "Embeddedness Theory", "Relational Embeddedness", "Structural Embeddedness"]},
    {name: "(社会表征理论)Social Representation Theory", aliases: ["Social Representation Theory", "social representations", "representation theory"]},
    {name: "(利益相关者理论)Stakeholder Theory", aliases: ["Stakeholder Theory", "stakeholder approach", "stakeholder model", "Stakeholder Salience Framework", "Stakeholder View"]},
    {name: "(管家理论)Stewardship Theory", aliases: ["Stewardship Theory", "stewardship model", "stewardship approach"]},
    {name: "(战略选择理论)Strategic Choice Theory", aliases: ["Strategic Choice Theory", "strategic choice", "strategic decision", "Miles and Snow Business Strategy", "Miles and Snow's Typology"]},
    {name: "(结构适应理论)Structural Adaptation Theory", aliases: ["Structural Adaptation Theory", "structural adaptation", "adaptive structuration"]},
    {name: "(结构权变理论)Structural Contingency Theory", aliases: ["Structural Contingency Theory", "structural contingency", "contingency structure", "Organization Design Theory"]},
    {name: "(结构化理论)Structuration Theory", aliases: ["Structuration Theory", "giddens structuration", "structuration", "structure theory"]},
    {name: "(锦标赛理论)Tournament Theory", aliases: ["Tournament Theory", "tournament model", "promotion tournament"]},
    {name: "(特质激活理论)Trait Activation Theory", aliases: ["Trait Activation Theory", "TAT", "trait activation", "personality activation"]},
    {name: "(交易成本理论)Transaction Cost Theory", aliases: ["Transaction Cost Theory", "Transaction Cost Economics", "transaction cost economics", "Williamson's Theory of the Firm", "Williamson Theory of the Firm", "Williamson theory"]},
    {name: "(变革型领导理论)Transformational Leadership Theory", aliases: ["Transformational Leadership Theory", "transformational leadership", "transformational leader"]},
    {name: "(不确定性管理理论)Uncertainty Management Theory", aliases: ["Uncertainty Management Theory", "uncertainty management", "UMT"]},
    {name: "(不确定性-认同理论)Uncertainty-Identity Theory", aliases: ["Uncertainty-Identity Theory", "uncertainty identity", "identity uncertainty"]},
    {name: "(高阶理论)Upper Echelons Theory", aliases: ["Upper Echelons Theory", "upper echelon", "top management team", "TMT", "Upper-Echelon Theory"]},
    {name: "(演化理论)Evolutionary Theory", aliases: ["Evolutionary Theory", "organizational evolution", "evolutionary approach"]},
    {name: "(中层理论)Middle Range Theory", aliases: ["Middle Range Theory", "middle-range theory", "merton theory"]},
    {name: "(悖论理论)Paradox Theory", aliases: ["Paradox Theory", "organizational paradox", "paradox management"]},
    {name: "(内部市场效率论)Internal Market Efficiency Arguments", aliases: ["Internal Market Efficiency Arguments", "Internal market efficiency arguments theory"]},
    {name: "(投资组合理论)Portfolio Theory", aliases: ["Portfolio Theory", "Portfolio theory"]},
    {name: "(市场力量理论)Market Power Theory", aliases: ["Market Power Theory", "Market power theory"]},
    {name: "(实物期权理论)Real Options Theory (ROT)", aliases: ["Real Options Theory (ROT)", "ROT", "real options theory", "Option to Defer", "Option to Grow"]},
    {name: "(新古典投资理论)Neoclassical Investment Theory", aliases: ["Neoclassical Investment Theory", "Neoclassical investment theory"]},
    {name: "(组织双元性)Organizational Ambidexterity (OA)", aliases: ["Organizational Ambidexterity (OA)", "ambidexterity", "OA", "Ambidextrous Search", "Ambidextrous Organizational Culture", "Ambidextrous Innovation", "Ambidextrous Knowledge Strategy"]},
    {name: "(网络外部性)Network Externalities", aliases: ["Network Externalities", "network externalities theory", "network externality"]},
    {name: "(通用技术)General Purpose Technology", aliases: ["General Purpose Technology", "General-Purpose Technology", "General-Purpose Technologies Theory", "GPT", "general purpose technology theory"]},
    {name: "(双边市场理论)Two-Sided Market Theory", aliases: ["Two-Sided Market Theory", "Two-Sided Markets", "two sided market", "platform theory"]},
    {name: "(结构洞理论)Structural Holes", aliases: ["Structural Holes", "structural holes theory", "burt structural holes"]},
    {name: "(网络效应)Network Effects", aliases: ["Network Effects", "network effects theory", "network effect"]},
    {name: "(网络闭合理论)Network Closure Theory", aliases: ["Network Closure Theory", "network closure", "closure theory"]},
    {name: "(萨诺夫定律)Sarnoff's Law", aliases: ["Sarnoff's Law", "sarnoff law", "broadcasting network"]},
    {name: "(梅特卡夫定律)Metcalfe's Law", aliases: ["Metcalfe's Law", "metcalfe law", "network value"]},
    {name: "(里德定律)Reed's Law", aliases: ["Reed's Law", "reed law", "group forming networks"]},
    {name: "(飞轮效应)Flywheel Effect", aliases: ["Flywheel Effect", "flywheel theory", "momentum effect"]},
    {name: "(马太效应)Matthew Effect", aliases: ["Matthew Effect", "matthew effect theory", "cumulative advantage"]},
    {name: "(逆向选择理论)Adverse Selection Theory", aliases: ["Adverse Selection Theory", "The Lemons Problem", "Adverse Selection"]},
    {name: "(注意力基础观)Attention-Based View", aliases: ["Attention-Based View", "ABV", "Attention-Based View of the Firm", "Managerial Cognition"]},
    {name: "(核心能力)Competence-Based Theory", aliases: ["Competence-Based Theory", "Competence-Based View", "CBV", "CBT", "Core Competence Theory"]},
    {name: "(内生增长理论)Endogenous Growth Theory", aliases: ["Endogenous Growth Theory", "New Growth Theory", "Endogenous Growth Model", "Romer model", "Dixit-Stiglitz model"]},
    {name: "(期望理论)Expectancy Theory", aliases: ["Expectancy Theory", "Vroom's Expectancy Theory", "VIE Theory", "Expectancy-Valence Model"]},
    {name: "(内部化理论)Internalization Theory", aliases: ["Internalization Theory", "Internalisation Theory"]},
    {name: "(知识溢出理论)Knowledge Spillover Theory", aliases: ["Knowledge Spillover Theory", "R&D Spillover Theory", "Technological Spillover Theory"]},
    {name: "(合法性理论)Legitimacy Theory", aliases: ["Legitimacy Theory", "Organizational Legitimacy"]},
    {name: "(马歇尔集聚理论)Marshallian Agglomeration Theory", aliases: ["Marshallian Agglomeration Theory", "Marshall's Theory of Agglomeration", "Marshallian Externalities"]},
    {name: "(路径依赖理论)Path Dependency Theory", aliases: ["Path Dependency Theory", "Path Dependence"]},
    {name: "(区域分支理论)Regional Branching Theory", aliases: ["Regional Branching Theory", "Theory of Regional Branching", "Regional Diversification Theory"]},
    {name: "(关系租金理论)Relational Rent Theory", aliases: ["Relational Rent Theory", "Relational View", "Relational View of Strategy"]},
    {name: "(熊彼特增长理论)Schumpeterian Growth Theory", aliases: ["Schumpeterian Growth Theory", "Schumpeter Growth Theory", "Creative Destruction Theory"]},
    {name: "(三螺旋模型)Triple Helix Model", aliases: ["Triple Helix Model", "Triple Helix Theory", "University-Industry-Government Triple Helix Model", "Regional Innovation System"]},
    {name: "(集群理论)Cluster Theory", aliases: ["Cluster Theory", "Industry Cluster Theory", "Theory of Competitive Clusters", "Porter's diamond model"]},
    {name: "(互补性理论)Complementarity Theory", aliases: ["Complementarity Theory", "Theory of Strategic Complementarities", "Supermodularity Theory", "Theory of Synergy"]},
    {name: "(临界质量理论)Critical Mass Theory", aliases: ["Critical Mass Theory", "Threshold Models of Collective Behavior"]},
    {name: "(雅各布斯外部性理论)Jacobs Externalities Theory", aliases: ["Jacobs Externalities Theory", "Jacobs' Theory of Externalities", "Urbanization Economies"]},
    {name: "(模块化理论)Modularity Theory", aliases: ["Modularity Theory", "Theory of Modularity", "Modular Systems Theory"]},
    {name: "(普遍达尔文主义)Universal Darwinism", aliases: ["Universal Darwinism", "Universal Selection Theory", "Generalized Darwinism"]},
    {name: "(人才增益理论)Brain Gain Theory", aliases: ["Brain Gain Theory", "The New Economics of the Brain Drain"]},
    {name: "(人才流失理论)Brain Drain Theory", aliases: ["Brain Drain Theory", "Human Capital Flight"]},
    {name: "(乌普萨拉模型)Uppsala Model", aliases: ["Uppsala Model", "Internationalization Process Theory", "Stages Model of Internationalization"]},
    {name: "(资本主义多样性)Varieties of Capitalism", aliases: ["Varieties of Capitalism", "VoC", "Comparative Institutional Theory"]},
    {name: "(SECI 模型)SECI Model", aliases: ["SECI Model", "SECI", "Knowledge Conversion Model", "Nonaka SECI", "Knowledge Life Cycle"]},
    {name: "(一般系统论)General Systems Theory", aliases: ["General Systems Theory", "GST", "General System Theory"]},
    {name: "(一阶控制论)First-Order Cybernetics", aliases: ["First-Order Cybernetics", "First order cybernetics"]},
    {name: "(二阶控制论)Second-Order Cybernetics", aliases: ["Second-Order Cybernetics", "Second order cybernetics"]},
    {name: "(企业行为理论)Behavioral Theory of the Firm", aliases: ["Behavioral Theory of the Firm", "BTF", "Cyert and March", "Performance Feedback Theory", "Problemistic search"]},
    {name: "(分工理论)Division of Labor Theory", aliases: ["Division of Labor Theory", "Division of labour theory", "Division of labor"]},
    {name: "(动机的社会认知理论)Social-Cognitive Theories of Motivation", aliases: ["Social-Cognitive Theories of Motivation", "Social cognitive motivation", "Social cognitive theory of motivation"]},
    {name: "(复杂系统理论)Complex Systems Theory", aliases: ["Complex Systems Theory", "Complex systems", "Complexity science"]},
    {name: "(彭罗斯的企业增长理论)Penrose's Theory of the Growth of the Firm", aliases: ["Penrose's Theory of the Growth of the Firm", "Penrose theory of the firm", "Penrose growth theory"]},
    {name: "(心理逻辑理论)Mental Logic Theory", aliases: ["Mental Logic Theory", "Mental logic"]},
    {name: "(技术范式理论)Techno-Economic Paradigm Theory", aliases: ["Techno-Economic Paradigm Theory", "Techno-Socio-Economic Paradigm", "TEP", "Techno-economic paradigm"]},
    {name: "(新古典增长模型)Neoclassical Growth Model", aliases: ["Neoclassical Growth Model", "Neoclassical growth theory", "Solow model"]},
    {name: "(期望价值理论)Expectancy-Value Theory", aliases: ["Expectancy-Value Theory", "Expectancy Value Theory", "EVT"]},
    {name: "(构型理论)Configurational Theory", aliases: ["Configurational Theory", "Configuration theory", "Configural theory"]},
    {name: "(自我效能感)Self-Efficacy", aliases: ["Self-Efficacy", "Self efficacy", "Perceived self-efficacy", "Bandura self-efficacy"]},
    {name: "(自组织理论)Self-organization Theory", aliases: ["Self-organization Theory", "Self organization theory", "Self-organization"]},
    {name: "(认知图式理论)Cognitive Schema Theory", aliases: ["Cognitive Schema Theory", "Schema theory", "Schemas"]},
    {name: "(长波理论)Long Wave Theory", aliases: ["Long Wave Theory", "Kondratiev waves", "K-waves", "Long waves"]},
    {name: "(间断平衡理论)Punctuated Equilibrium Theory", aliases: ["Punctuated Equilibrium Theory", "Punctuated equilibrium", "Punctuated equilibrium model"]},
    {name: "(家庭系统理论)Family Systems Theory", aliases: ["Family Systems Theory", "Bowen Family Systems Theory", "FST", "systems theory in psychology", "Family Dynamics Theory"]},
    {name: "(匹配理论)Matching Theory", aliases: ["Matching Theory", "Stable Matching", "Market Design", "Gale-Shapley Algorithm", "Two-Sided Matching"]},
    {name: "(组织信息处理理论)Organizational Information-Processing Theory", aliases: ["Organizational Information-Processing Framework", "OIPT", "Galbraith's Information-Processing Model", "Information Processing Theory (Organizational)"]},
    {name: "(社会情感财富理论)Socioemotional Wealth Theory", aliases: ["Socioemotional Wealth Theory", "SEW", "SEW Theory", "Socio-emotional Wealth"]},
    {name: "(权衡理论)Tradeoff Theory", aliases: ["Tradeoff Theory of Capital Structure", "Static Tradeoff Theory", "Dynamic Tradeoff Theory", "Capital Structure Tradeoff Theory", "Tradeoff Model", "Financial Flexibility Theory"]},
    {name: "(跨主动记忆系统)Transactive Memory Systems", aliases: ["Transactive Memory Systems", "TMS", "Group Memory", "Collective Memory Systems"]},
    {name: "(美德知识论)Virtue Epistemology", aliases: ["Virtue Epistemology", "VE", "Virtue Reliabilism", "Virtue Responsibilism", "Epistemic Virtue"]},
    {name: "(社会技术系统理论)Socio-technical Systems Theory", aliases: ["Socio-technical Systems Theory", "STS", "Sociotechnical Systems", "Sociotech", "Socio-technical Systems Approach"]},
    {name: "(创造力成分理论)Componential Theory of Creativity", aliases: ["Componential Theory of Creativity", "Amabile's Componential Theory", "Amabile's Model of Creativity", "Three-Factor Theory of Creativity"]},
    {name: "(有限理性)Bounded Rationality", aliases: ["Bounded Rationality", "Bounded Rationality Theory", "Simon's Bounded Rationality", "Satisficing"]},
    {name: "(常态事故理论)Normal Accident Theory", aliases: ["Normal Accident Theory", "NAT", "High-Risk Systems Theory", "Perrow's Normal Accident Theory", "Normal Accident Theory (Chinese)"]},
    {name: "(意识—动机—能力框架)Awareness–Motivation–Capability Framework", aliases: ["Awareness–Motivation–Capability Framework", "AMC Framework", "AMC Model", "Competitor Analysis AMC Framework"]},
    {name: "(理性疏忽理论)Rational Inattention Theory", aliases: ["Rational Inattention Theory", "Rational Inattention", "RI", "Theory of Rational Inattention"]},
    {name: "(寻租理论)Rent-seeking Theory", aliases: ["Rent-seeking Theory", "Rent Seeking", "Unproductive Profit Seeking"]},
    {name: "(威胁僵化理论)Threat Rigidity Theory", aliases: ["Threat Rigidity Theory", "Threat Rigidity Effect", "Threat Rigidity Hypothesis", "Staw's Threat Rigidity Model"]},
    {name: "(高可靠性组织理论)High Reliability Organization Theory", aliases: ["High Reliability Organization Theory", "HRO Theory", "HRO", "High Reliability Organizing"]},
    {name: "(X-效率理论)X-efficiency Theory", aliases: ["X-efficiency Theory", "X-Efficiency", "Leibenstein's X-efficiency"]},
    {name: "(技术—组织—环境框架)Technology–Organization–Environment Framework", aliases: ["Technology–Organization–Environment Framework", "TOE Framework", "TOE Model"]},
    {name: "(可供性理论)Affordance Theory", aliases: ["Affordance Theory", "Theory of Affordances", "Ecological Approach to Visual Perception"]},
    {name: "(建构主义理论)Constructivist Theory", aliases: ["Constructivism", "Learning Theory", "Constructivist Theory", "Cognitive Constructivism", "Social Constructivism"]},
    {name: "(精細加工可能性模型)Elaboration Likelihood Model", aliases: ["Elaboration Likelihood Model", "ELM", "ELM Theory", "Petty & Cacioppo's ELM"]},
    {name: "(刺激—有机体—反应理论)Stimulus–Organism–Response Theory", aliases: ["Stimulus–Organism–Response Theory", "S-O-R Theory", "S-O-R Model", "SOR Paradigm"]},
    {name: "(压力与应对的交易模型)Transactional Model of Stress and Coping", aliases: ["Transactional Model of Stress and Coping", "Transactional Theory of Stress", "Cognitive Appraisal Theory of Stress", "Lazarus and Folkman's Stress Model", "Stress Transaction Theory"]},
    {name: "(三支决策理论)Three-way Decision Theory", aliases: ["Three-way Decision", "3WD", "Decision-Theoretic Rough Sets", "Three-way Decision (Chinese)"]},
    {name: "(经济落后假说)Economic Backwardness Hypothesis", aliases: ["Gerschenkron's Backwardness Hypothesis", "Gerschenkron Effect", "Advantage of Backwardness", "Gerschenkron's Backwardness Theory", "Advantage of Backwardness (Chinese)", "Gerschenkron Effect (Chinese)"]},
    {name: "(任务-技术匹配理论)Task-Technology Fit", aliases: ["Task-Technology Fit", "TTF", "TTF Model", "Technology-Task Fit"]},
    {name: "(凯恩斯主义经济学)Keynesian Economics", aliases: ["Keynesianism", "Demand-side Economics", "Demand-side Theory"]},
    {name: "(消费价值理论)Theory of Consumption Values", aliases: ["Theory of Consumption Values", "TCV", "Sheth-Newman-Gross Model"]},
    {name: "(外国人劣势)Liability of Foreignness", aliases: ["Liability of Foreignness", "LOF", "costs of foreignness"]},
    {name: "(多模态)Multimodal Theory", aliases: ["Multimodality", "Multimodal Discourse Analysis", "Social Semiotics of Multimodality"]},
    {name: "(技术退守)Technological Retrenchment", aliases: ["Technological Retrenchment", "R&D Retrenchment", "Technology Scope Reduction"]},
    {name: "(注意力残留)Attention Residue", aliases: ["Attention Residue", "Attention Residue Effect"]},
    {name: "(创效理论)Effectuation", aliases: ["Effectuation Theory", "Effectual Logic", "Effectuation Process"]},
    {name: "(心流理论)Flow Theory", aliases: ["Flow", "Optimal Experience", "Zone"]},
    {name: "(通才理论)Jacks-of-all-trades Theory", aliases: ["Jacks-of-all-trades Theory", "Lazear's JOAT theory"]},
    {name: "(多市场竞争理论)Multimarket Competition Theory", aliases: ["Multimarket Competition", "MMC", "Mutual Forbearance Theory", "Multimarket Contact"]},
    {name: "(标尺竞争理论)Yardstick Competition Theory", aliases: ["Yardstick Competition", "Benchmarking Competition"]},
    {name: "(商品理论)Commodity Theory", aliases: ["Commodity Theory", "Scarcity Theory of Value"]},
    {name: "(精英理论)Elite Theory", aliases: ["Elite Theory", "Classical Elite Theory", "Power Elite Theory"]},
    {name: "(断裂带理论)Faultline Theory", aliases: ["Faultline Theory", "Group Faultlines", "Team Faultlines"]},
    {name: "(公平理论)Equity Theory", aliases: ["Equity Theory of Motivation", "Adams' Equity Theory"]}
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
    // 关键词匹配(≥2个词重合)
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
  // 理论关键词检查
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
        // 备用方案:简单HTML清理
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
  // 批量保存多个理论标签（带序号）
  const saveItemWithMultipleTags = async (item, tagNames, isNoteUpdate) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        // 为每个理论添加序号标签
        tagNames.forEach((tag, index) => {
          const numberedTag = 'Theory/' + (index + 1) + tag;
          item.addTag(numberedTag);
        });
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
  // 保存单个标签（用于错误状态）
  const saveItemWithTag = async (item, tagName, countType) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        item.addTag('Theory/' + tagName);
        await item.save();
      });
      // 更新计数器
      switch(countType) {
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
      }
      return true;
    } catch (error) {
      console.error('保存标签失败:', error);
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
      // 如果没有笔记内容,使用摘要作为备选
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
      // AI分析 - 支持多理论识别,不截断内容
      const prompt = `Analyze the literature content and identify all explicitly mentioned theories. ${noteCount > 1 ? `This content includes ${noteCount} separate notes.` : ''}
${content}
Requirements:
1. Only identify clear theory, model, or framework names
2. Use English names for theories (translate Chinese theory names to English if necessary)
3. Return at most three theories found across all content
4. Order by relevance: most important theory first
5. Format: ["Theory 1", "Theory 2", "Theory 3"] or ["No clear theories"]`;
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
        // 处理所有识别到的理论,准备带序号的标签
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
