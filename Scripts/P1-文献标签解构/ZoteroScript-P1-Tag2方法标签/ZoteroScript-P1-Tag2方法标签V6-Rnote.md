---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag2方法标签V6-Rnote
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
RelatedNote:
RelatedProjects:
CardRecord: ''
---

## ZoteroScript-P1-Tag2 方法标签 V6-Rnote（笔记优先版本）

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
- 工具变量法
- 两阶段最小二乘法
- 广义矩估计-工具变量
- 双重差分法
- 三重差分法
- 多期双重差分
- 交错双重差分
- 断点回归设计
- 模糊断点回归
- 倾向得分匹配
- 核匹配
- 马氏距离匹配
- 遗传匹配
- 合成控制法
- 广义合成控制
- 事件研究法

◆ 中介效应具体方法(禁止使用"中介效应分析"):
- Sobel检验
- Bootstrap中介检验
- 逐步回归法-中介
- 结构方程-中介路径
- 因果中介分析

◆ 调节效应具体方法(禁止使用"调节效应分析"):
- 交互项回归
- 分层回归-调节
- 简单斜率检验
- Johnson-Neyman区间
- 三重交互项
- 分样本回归
- 分组检验-调节

◆ 面板数据方法:
- 固定效应模型
- 随机效应模型
- 混合效应模型
- 差分GMM
- 系统GMM
- 面板门槛回归
- 面板平滑转换回归
- 面板分位数回归
- 面板向量自回归
- 动态面板模型

◆ 时间序列方法:
- 向量自回归
- 结构向量自回归
- 向量误差修正模型
- Johansen协整检验
- Engle-Granger协整
- 格兰杰因果检验
- 脉冲响应函数
- 方差分解
- ARIMA模型
- GARCH模型
- EGARCH模型
- 马尔可夫区制转换
- 状态空间模型
- 卡尔曼滤波

◆ 空间计量方法:
- 空间滞后模型
- 空间误差模型
- 空间杜宾模型
- 地理加权回归
- 空间面板模型
- Moran指数检验

◆ 微观计量方法:
- Probit模型
- Logit模型
- 多元Logit
- 条件Logit
- 混合Logit
- Tobit模型
- 泊松回归
- 负二项回归
- 零膨胀泊松
- 零膨胀负二项
- Heckman两阶段
- 生存分析
- Cox比例风险模型
- 分数响应模型
- 排序Probit
- 排序Logit

◆ 主体建模方法:
- 博弈论模型
- 纳什均衡求解
- 委托代理模型
- 信号博弈
- 动态规划模型
- 最优控制理论
- 拍卖理论模型
- 机制设计模型
- 匹配理论模型
- 一般均衡模型
- 可计算一般均衡
- 动态随机一般均衡
- 世代交叠模型
- 实物期权模型
- 投资组合优化
- 资产定价模型
- 随机折现因子
- 结构性信用风险模型

◆ 仿真模拟方法:
- 蒙特卡洛模拟
- Agent-Based建模
- 系统动力学
- 离散事件仿真
- 马尔可夫链蒙特卡洛
- 吉布斯抽样
- Metropolis-Hastings

◆ 定性分析方法:
- 扎根理论编码
- 主题分析法
- 内容分析法
- 话语分析
- 叙事分析
- 现象学分析
- 民族志方法
- 参与式观察
- 深度访谈法
- 焦点小组访谈
- 定性比较分析
- 模糊集定性比较
- 过程追踪法
- 案例内分析
- 跨案例分析
- 模式匹配
- 解释性建构
- 批判话语分析

◆ 混合方法:
- 序贯解释设计
- 序贯探索设计
- 嵌入式设计
- 趋同设计
- 多阶段混合设计

◆ 结构模型:
- 结构方程模型
- 验证性因子分析
- 多层线性模型
- 三层嵌套模型
- 潜类别分析
- 潜剖面分析
- 潜增长曲线
- 潜变量交互

◆ 机器学习方法:
- 随机森林
- 梯度提升树
- XGBoost
- LightGBM
- CatBoost
- 神经网络
- 卷积神经网络
- 循环神经网络
- 长短期记忆网络
- Transformer模型
- 支持向量机
- LASSO回归
- 岭回归
- 弹性网络
- 因果森林
- 双重机器学习
- 去偏机器学习

◆ 实验方法:
- 随机对照试验
- 田野实验
- 实验室实验
- 自然实验
- 完全析因设计
- 部分析因设计
- 拉丁方设计
- 重复测量设计
- 混合设计实验
- 准实验设计

◆ 内生性处理具体方法(禁止使用"内生性检验/分析"):
- 豪斯曼检验
- Durbin-Wu-Hausman检验
- 工具变量有效性检验
- 弱工具变量检验
- Sargan过度识别检验
- Hansen J统计量
- Cragg-Donald F统计量
- Stock-Yogo临界值

◆ 模型诊断具体方法:
- 平行趋势检验
- 安慰剂检验
- 排列检验
- 伪处理组检验
- 伪时间检验
- 断点连续性检验
- 密度连续性检验
- McCrary密度检验
- 协变量平衡检验
- 共同支撑假设检验

◆ 标准误调整方法:
- 异方差稳健标准误
- White标准误
- 聚类标准误
- 双向聚类标准误
- 自助法标准误
- Jackknife标准误
- 野自助法
- Newey-West标准误

◆ 其他高级方法(中高优先级):
- 贝叶斯估计
- 贝叶斯因子
- 变分贝叶斯
- 非参数回归
- 核回归
- 局部加权回归
- 样条回归
- 分位数回归
- 半参数回归
- 加性模型
- 多项式回归
- 岭回归
- 主成分回归
- 偏最小二乘回归
- 文本分析-主题模型
- 文本分析-情感分析
- 文本分析-词嵌入
- 网络分析-中心性
- 网络分析-社区发现
- 社会网络分析

◆ 替代性检验方法(最低优先级,仅在主要方法不足时使用):
- 变量替换检验
- 核心变量滞后
- 样本期调整
- 样本筛选调整
- 控制变量扩展
- 聚类层级调整
- 分位数回归对比
- 子样本敏感性
- 极端值剔除
- Winsorize处理

重要说明:
1. **严格禁止使用以下模糊术语**:
   - ❌ "调节效应分析" → ✅ 使用"交互项回归"、"分样本回归"等
   - ❌ "中介效应分析" → ✅ 使用"Sobel检验"、"Bootstrap中介检验"等
   - ❌ "稳健性检验"、"稳健性分析" → ✅ 使用"变量替换检验"、"样本期调整"等
   - ❌ "内生性检验"、"内生性分析" → ✅ 使用"豪斯曼检验"、"工具变量有效性检验"等
   - ❌ "回归分析"、"实证分析"、"统计检验"、"模型验证"

2. **必须识别具体方法**: 
   - 例如"工具变量法"而非"内生性处理"
   - 例如"双重差分法"而非"政策评估"
   - 例如"交互项回归"而非"调节效应"

3. **优先级排序原则**:
   - 最高优先级: 因果推断核心方法 (IV/DID/PSM/RDD/SCM等)
   - 高优先级: 主体建模、估计技术、具体中介/调节方法、仿真模拟、定性方法
   - 中高优先级: 其他高级方法(贝叶斯、非参数、文本分析、网络分析等)
   - 中优先级: 模型诊断具体方法、内生性处理具体方法、标准误调整
   - 最低优先级: 替代性检验方法

4. **方法数量**: 识别2-5个方法标签
   - 按重要性从高到低排序
   - 替代性检验方法仅在其他重要方法标注完后才考虑
   - 如果文献使用了主体建模或定性方法,必须优先标注

5. **特殊区分**:
   - GMM: 区分"差分GMM"、"系统GMM"、"广义矩估计-工具变量"
   - 中介: 必须标注具体方法如"Sobel检验"而非"中介效应分析"
   - 调节: 必须标注"交互项回归"、"分样本回归"等而非"调节效应分析"

6. **每个标签只能包含一个具体方法名称**,不要用顿号连接多个方法

7. **所有术语使用简体中文标准名称**

请严格按照以下JSON格式返回(仅返回JSON,无其他文字):
["研究范式", "核心方法1", "核心方法2", …]

最少2个标签,最多5个标签。标签从第2个开始都是具体方法名称。

示例输出:
["定量-准实验", "双重差分法", "倾向得分匹配", "平行趋势检验"]
["定量-计量实证", "工具变量法", "差分GMM", "豪斯曼检验"]
["方法-建模", "博弈论模型", "纳什均衡求解"]
["定性-案例", "扎根理论编码", "主题分析法", "跨案例分析"]
["定量-计量实证", "固定效应模型", "交互项回归", "异方差稳健标准误"]
["方法-建模", "动态规划模型", "蒙特卡洛模拟"]
["定量-计量实证", "Bootstrap中介检验", "Sobel检验"]`;
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
