---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag2方法标签V5
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
- 三层架构
- 三维标签
- 代码
- 学术分析
- 学术标准
- 学术研究
- 层次理论
- 技术创新
- 文献管理
- 方法学分析
- 方法学理论
- 标签体系
- 样本分析
- 研究对象
- 研究方法
- 研究方法学
- 质量控制
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P1-Tag2 方法标签 V5

```javascript
#🏷️MethodTag1[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
  // 保存函数
  const saveItemWithTags = async (item, tags, countType) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        for (let tag of tags) {
          item.addTag(tag);
        }
        await item.save();
      });
      
      if (countType === 'success') successCount++;
      else if (countType === 'noContent') noContentCount++;
      else if (countType === 'fail') failCount++;
      
      return true;
    } catch {
      errorCount++;
      return false;
    }
  };
  
  for (let item of items) {
    try {
      // 检查是否已有sMeth/标签,有则跳过
      const existingTags = item.getTags();
      const hasMethodTags = existingTags.some(tag => tag.tag && tag.tag.startsWith("sMeth/"));
      if (hasMethodTags) {
        skipCount++;
        continue;
      }
      
      let content = "";
      
      // 内容获取
      try {
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text && fullTextData.text.trim().length > 100) {
              content = fullTextData.text.substring(0, 30000).trim();
            }
          } catch {}
        }
        
        if (!content) {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        }
      } catch {}
      
      if (!content) {
        await saveItemWithTags(item, ['sMeth/无内容'], 'noContent');
        continue;
      }
      
      // AI分析
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
        
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            await saveItemWithTags(item, ['sMeth/解析失败'], 'fail');
            continue;
          }
          
          const tags = JSON.parse(jsonMatch[0]);
          
          if (!Array.isArray(tags) || tags.length < 2 || tags.length > 6) {
            await saveItemWithTags(item, ['sMeth/格式错误'], 'fail');
            continue;
          }
          
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim());
            
          if (validTags.length < 2) {
            await saveItemWithTags(item, ['sMeth/标签不完整'], 'fail');
            continue;
          }
          
          const validMethodTypes = [
            '定性-案例', '综述-文献综述', '综述-方法指导', '综述-期刊社评',
            '定性-理论构建', '定量-计量实证', '定量-自然实验', '定量-准实验',
            '定量-问卷调查', '方法-建模', '方法-软件', '方法-程序'
          ];
          
          if (!validMethodTypes.includes(validTags[0])) {
            await saveItemWithTags(item, ['sMeth/方法类型无效'], 'fail');
            continue;
          }
          
          const tagsToAdd = ['sMeth/1' + validTags[0]];
          
          for (let i = 1; i < validTags.length && i < 6; i++) {
            tagsToAdd.push('sMeth/' + (i + 1) + validTags[i]);
          }
          
          await saveItemWithTags(item, tagsToAdd, 'success');
          
        } catch {
          await saveItemWithTags(item, ['sMeth/JSON解析失败'], 'fail');
        }
        
      } catch {
        await saveItemWithTags(item, ['sMeth/AI调用失败'], 'fail');
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `研究方法标签处理完成\n成功: ${successCount} | 跳过: ${skipCount} | 无内容: ${noContentCount} | 失败: ${failCount} | 错误: ${errorCount}`;
})()
}$
```
