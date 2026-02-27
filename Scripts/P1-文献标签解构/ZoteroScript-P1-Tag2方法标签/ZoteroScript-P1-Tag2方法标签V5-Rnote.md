---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag2方法标签V5-Rnote
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: [AI, JavaScript, Zotero, 三层架构, 三维标签, 代码, 学术分析, 学术标准, 学术研究, 层次理论, 技术创新, 文献管理, 方法学分析, 方法学理论, 标签体系, 样本分析, 研究对象, 研究方法, 研究方法学, 质量控制]
RelatedNote:
CardRecord:
---


## ZoteroScript-P1-Tag2 方法标签 V5-Rnote（笔记优先版本）

```javascript
#🏷️Rnote-MethodTag3[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  let successCount = 0;
  let skipCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
  // 检查是否有📒标签
  const hasNotebookTag = (item) => {
    const tags = item.getTags();
    return tags.some(tag => tag.tag && tag.tag.includes("📒"));
  };
  
  // 获取笔记内容 - 使用BetterNotes转换为markdown
  const getNoteContent = async (noteItem) => {
    try {
      return await Zotero.BetterNotes.api.convert.html2md(noteItem.getNote());
    } catch {
      return "";
    }
  };
  
  // 删除现有sMeth/和Method/标签
  const removeExistingMethodTags = async (item) => {
    const existingTags = item.getTags();
    const methodTags = existingTags.filter(tag => 
      tag.tag && (tag.tag.startsWith("sMeth/") || tag.tag.startsWith("Method/"))
    );
    for (let methodTag of methodTags) {
      item.removeTag(methodTag.tag);
    }
  };
  
  for (let item of items) {
    try {
      // 检查📒标签,没有则跳过
      if (!hasNotebookTag(item)) {
        skipCount++;
        continue;
      }
      
      // 有📒标签则处理,删除原有sMeth/和Method/标签
      await removeExistingMethodTags(item);
      
      let content = "";
      
      // 第一优先:获取笔记内容
      const noteIds = item.getNotes();
      
      if (noteIds.length > 0) {
        // 使用第一个笔记
        const noteItem = Zotero.Items.get(noteIds[0]);
        if (noteItem) {
          const noteContent = await getNoteContent(noteItem);
          if (noteContent && noteContent.trim().length > 50) {
            content = noteContent;
          }
        }
      }
      
      // 第二优先:如果没有笔记内容,尝试获取PDF内容
      if (!content) {
        try {
          const pdfItem = await item.getBestAttachment();
          if (pdfItem && pdfItem.isPDFAttachment()) {
            try {
              const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
              if (fullTextData && fullTextData.text && fullTextData.text.trim().length > 100) {
                // 使用PDF前30000字符,包含更详细的方法学描述
                content = fullTextData.text.substring(0, 30000).trim();
              }
            } catch {}
          }
        } catch {}
      }
      
      // 第三优先:如果没有PDF内容,使用摘要
      if (!content) {
        try {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        } catch {}
      }
      
      if (!content) {
        // 无内容,标注便于识别
        try {
          item.addTag('sMeth/无内容');
          await item.saveTx();
          noContentCount++;
        } catch {
          errorCount++;
        }
        continue;
      }
      
      // AI分析生成研究方法标签
      try {
        const prompt = `你是一位计量经济学和实证研究方法专家。请分析以下学术文献内容,识别其核心研究方法。重点关注方法学部分(Method/Methodology)、识别策略(Identification Strategy)、模型设定(Model Specification)和估计技术(Estimation Techniques)。
文献内容:
${content}
标签要求:
【标签1: 研究范式】必须从以下12类中精确选择一个:
- 定性-案例: 案例研究、比较案例、多案例设计
- 综述-文献综述: 系统性文献回顾、元分析
- 综述-方法指导: 方法论教程、研究设计指南
- 综述-期刊社评: 编者按、研究前沿述评
- 定性-理论构建: 概念模型开发、理论框架构建
- 定量-计量实证: 基于观察数据的计量分析
- 定量-自然实验: 利用外生冲击的准实验研究
- 定量-准实验: 匹配、DID、RDD等准实验设计
- 定量-问卷调查: 基于调查数据的实证研究
- 方法-建模: 理论模型、优化模型、仿真模拟
- 方法-软件: 统计软件包、工具包开发
- 方法-程序: 算法设计、计算方法创新
【标签2-5: 核心方法与辅助技术】(识别2-5个方法,按重要性排序)
请从以下方法中选择,严格使用标准术语:
◆ 因果推断核心方法(最高优先级):
- 工具变量法 (IV/2SLS/GMM-IV)
- 双重差分法 (DID/三重差分)
- 断点回归设计 (RDD/模糊RDD)
- 倾向得分匹配 (PSM/核匹配)
- 合成控制法 (SCM)
- 事件研究法 (Event Study)
- 中介效应分析 (Mediation)
- 调节效应分析 (Moderation)
◆ 面板数据方法:
- 固定效应模型 (FE)
- 随机效应模型 (RE)
- 动态面板GMM (差分GMM/系统GMM)
- 面板门槛回归
- 面板分位数回归
- 面板向量自回归 (PVAR)
◆ 时间序列方法:
- 向量自回归 (VAR/SVAR)
- 协整分析 (Cointegration)
- 误差修正模型 (ECM/VECM)
- ARIMA模型
- GARCH族模型
- 状态空间模型
◆ 空间计量方法:
- 空间滞后模型 (SAR)
- 空间误差模型 (SEM)
- 空间杜宾模型 (SDM)
- 地理加权回归 (GWR)
◆ 微观计量方法:
- Probit/Logit模型
- Tobit模型
- 泊松回归
- 负二项回归
- 零膨胀模型
- Heckman两阶段
- 生存分析
◆ 结构模型:
- 结构方程模型 (SEM)
- 多层线性模型 (HLM)
- 潜类别分析 (LCA)
- 潜增长曲线 (LGM)
◆ 机器学习方法:
- 随机森林
- 梯度提升树 (XGBoost/LightGBM)
- 神经网络/深度学习
- 支持向量机 (SVM)
- LASSO/岭回归
- 因果森林 (Causal Forest)
◆ 实验方法:
- 随机对照试验 (RCT)
- 田野实验
- 实验室实验
- 自然实验
- 析因设计
◆ 定性方法:
- 扎根理论
- 内容分析
- 话语分析
- 案例研究法
- QCA定性比较分析
◆ 辅助分析技术(优先级高):
- 平行趋势检验 (针对DID)
- 安慰剂检验
- 工具变量有效性检验 (弱工具变量/过度识别)
- 内生性检验 (豪斯曼检验/DWH检验)
- 异方差稳健标准误
- 聚类标准误
- 自助法 (Bootstrap)
- 分样本回归
- 交互项分析
◆ 稳健性检验方法(优先级最低,仅在其他方法不足时使用):
- 变量替换检验
- 变量滞后检验
- 样本调整检验
- 时间窗口调整
- 控制变量调整
- 聚类层级调整
- 分位数回归检验
- 子样本检验
- 测量误差处理
- 极端值处理
重要说明:
1. 禁止使用模糊术语: "回归分析"、"实证分析"、"统计检验"、"模型验证"等
2. 必须识别具体方法: 例如"工具变量法"而非"内生性处理","双重差分法"而非"政策评估"
3. 优先级排序原则:
   - 最高优先级: 因果推断核心方法 (IV/DID/PSM/RDD/SCM等)
   - 高优先级: 估计技术与辅助分析 (GMM/面板模型/内生性检验等)
   - 最低优先级: 基础稳健性检验 (变量替换/样本调整/时间窗口等)
4. 方法数量: 识别2-5个方法标签
   - 按重要性从高到低排序
   - 稳健性检验方法仅在其他重要方法标注完后才考虑
5. GMM注意区分: 动态面板GMM vs 工具变量GMM
6. 每个标签只能包含一个具体方法名称,不要用顿号连接多个方法
7. 所有术语使用简体中文标准名称
请严格按照以下JSON格式返回(仅返回JSON,无其他文字):
["研究范式", "核心方法1", "核心方法2", …]
最少2个标签,最多5个标签。标签从第2个开始都是具体方法名称。
示例输出:
["定量-准实验", "双重差分法", "倾向得分匹配", "平行趋势检验"]
["定量-计量实证", "工具变量法", "动态面板GMM", "弱工具变量检验"]
["方法-建模", "随机森林", "因果森林"]
["定量-计量实证", "固定效应模型", "异方差稳健标准误"]`;
        const response = await Meet.Global.views.ask(prompt);
        
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            failCount++;
            continue;
          }
          
          const tags = JSON.parse(jsonMatch[0]);
          
          if (!Array.isArray(tags) || tags.length < 2 || tags.length > 6) {
            failCount++;
            continue;
          }
          
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim());
          
          if (validTags.length < 2) {
            failCount++;
            continue;
          }
          
          const validMethodTypes = [
            '定性-案例', '综述-文献综述', '综述-方法指导', '综述-期刊社评',
            '定性-理论构建', '定量-计量实证', '定量-自然实验', '定量-准实验',
            '定量-问卷调查', '方法-建模', '方法-软件', '方法-程序'
          ];
          
          if (!validMethodTypes.includes(validTags[0])) {
            failCount++;
            continue;
          }
          
          // 添加标签
          item.addTag('sMeth/1' + validTags[0]);
          
          for (let i = 1; i < validTags.length && i < 6; i++) {
            item.addTag('sMeth/' + (i + 1) + validTags[i]);
          }
          
          try {
            await item.saveTx();
            successCount++;
          } catch {
            errorCount++;
          }
        } catch {
          // JSON解析失败
          failCount++;
        }
      } catch {
        // AI调用失败
        failCount++;
      }
    } catch {
      errorCount++;
    }
  }
  
  return `研究方法标签处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```
