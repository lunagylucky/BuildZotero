---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag2方法标签V4-R
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
- 三维标签
- 代码
- 学术分析
- 层次理论
- 文献管理
- 标签体系
- 样本分析
- 研究对象
- 研究方法学
- Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord: ''
---

## ZoteroScript-P1-Tag2 方法标签 V4-R（重新处理版本）

```javascript
#🏷️R-MethodTag2[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  let successCount = 0;
  let skipCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
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
        case 'noContent':
          noContentCount++;
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
  
  for (let item of items) {
    try {
      // 删除原有sMeth/和Method/标签
      await removeExistingMethodTags(item);
      
      let content = "";
      // 内容获取 - 优先PDF前30000字符,备选摘要
      try {
        // 首先尝试获取PDF内容
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text && fullTextData.text.trim().length > 100) {
              // 优先使用PDF前30000字符,包含更详细的方法学描述
              content = fullTextData.text.substring(0, 30000).trim();
            }
          } catch {}
        }
        // 如果PDF获取失败,再使用摘要
        if (!content) {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        }
      } catch {}
      if (!content) {
        // 无内容,标注便于识别
        await saveItemWithTags(item, ['sMeth/无内容'], 'noContent');
        continue;
      }
      // AI分析生成研究方法标签
      try {
        const prompt = `分析以下学术文献内容,识别其核心研究方法。请特别关注方法学部分、模型构建、数据分析技术、统计方法和实验设计:
${content}
要求:
1. 第一个标签:方法类型 - 必须从以下选择:
   - 定性-案例:案例研究方法
   - 综述-文献综述:文献综述研究
   - 综述-方法指导:方法论指导文章
   - 综述-期刊社评:期刊编辑评论
   - 定性-理论构建:理论构建研究
   - 定量-计量实证:计量经济学实证研究
   - 定量-自然实验:自然实验研究
   - 定量-准实验:准实验研究
   - 定量-问卷调查:问卷调查研究
   - 方法-建模:建模方法研究
   - 方法-软件:软件工具开发
   - 方法-程序:算法程序开发
2. 第二个标签:核心分析方法 - 研究中使用最重要的分析技术,必须是具体的方法名称(3-8个字符)
   统计模型类:
   - 多元线性回归、逻辑回归、泊松回归、负二项回归、分位数回归、岭回归、套索回归
   - 固定效应模型、随机效应模型、混合效应模型、多层线性模型、分层贝叶斯模型
   - 结构方程模型、潜变量模型、因子分析、主成分分析、聚类分析、判别分析
   计量经济学类:
   - 工具变量法、两阶段最小二乘、广义矩估计、断点回归、双重差分法
   - 倾向得分匹配、协整检验、格兰杰因果、向量自回归、误差修正模型
   - 面板门槛模型、空间计量模型、动态面板模型、系统GMM估计
   机器学习类:
   - 神经网络、卷积神经网络、循环神经网络、支持向量机、随机森林
   - 梯度提升树、朴素贝叶斯、决策树、K近邻算法、深度学习、强化学习
   实验方法类:
   - 随机对照试验、田野实验、实验室实验、准实验设计、自然实验
   - A/B测试、析因实验、正交实验、响应面法
   定性方法类:
   - 内容分析、扎根理论、现象学研究、叙事分析、话语分析、案例研究
3. 第三个标签:次要分析方法 - 研究中使用的辅助分析技术,必须是具体的方法名称(3-8个字符)
   检验方法类:
   - T检验、F检验、卡方检验、KS检验、白异方差检验、DW检验、LM检验
   - 单位根检验、协整检验、豪斯曼检验、沃尔德检验、拉格朗日乘数检验
   验证方法类:
   - 交叉验证、留一验证、自助法、蒙特卡洛模拟、敏感性分析
   - 残差分析、异常值检测、多重共线性检验、异方差检验
   数据处理类:
   - 数据标准化、主成分降维、缺失值插补、异常值处理、数据变换
   - 特征选择、特征工程、数据平滑、滤波处理、数据融合
4. 全部使用简体中文
5. 严格要求:必须选择具体的方法名称,禁止使用模糊词汇如"稳健性检验"、"内生性分析"、"模型验证"等
6. 按照重要程度排序,第1个标签是方法类型,第2个标签是最核心的方法,第3个标签是次要方法
请直接返回JSON数组,格式:["方法类型", "核心分析方法", "次要分析方法"]`;
        const response = await Meet.Global.views.ask(prompt);
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            await saveItemWithTags(item, ['sMeth/解析失败'], 'fail');
            continue;
          }
          const tags = JSON.parse(jsonMatch[0]);
          if (!tags || tags.length !== 3) {
            await saveItemWithTags(item, ['sMeth/格式错误'], 'fail');
            continue;
          }
          // 验证和清理标签
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim());
          if (validTags.length !== 3) {
            await saveItemWithTags(item, ['sMeth/标签不完整'], 'fail');
            continue;
          }
          // 验证第一个标签必须是标准的方法类型
          const validMethodTypes = [
            '定性-案例', '综述-文献综述', '综述-方法指导', '综述-期刊社评',
            '定性-理论构建', '定量-计量实证', '定量-自然实验', '定量-准实验',
            '定量-问卷调查', '方法-建模', '方法-软件', '方法-程序'
          ];
          if (!validMethodTypes.includes(validTags[0])) {
            await saveItemWithTags(item, ['sMeth/方法类型无效'], 'fail');
            continue;
          }
          // 准备要添加的标签 - 按序号排序
          const tagsToAdd = [
            'sMeth/1' + validTags[0],
            'sMeth/2' + validTags[1],
            'sMeth/3' + validTags[2]
          ];
          // 使用统一的保存函数
          await saveItemWithTags(item, tagsToAdd, 'success');
        } catch (parseError) {
          // JSON解析失败
          console.error('JSON解析失败:', parseError);
          await saveItemWithTags(item, ['sMeth/JSON解析失败'], 'fail');
        }
      } catch (aiError) {
        // AI调用失败
        console.error('AI调用失败:', aiError);
        await saveItemWithTags(item, ['sMeth/AI调用失败'], 'fail');
      }
    } catch (itemError) {
      console.error('处理条目失败:', itemError);
      errorCount++;
    }
  }
  return `研究方法标签重新处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```
