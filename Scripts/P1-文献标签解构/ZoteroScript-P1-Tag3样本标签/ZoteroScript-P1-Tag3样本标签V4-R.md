---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag3样本标签V4-R
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

## ZoteroScript-P1-Tag3 样本标签 V4-R（重新处理版本）

```javascript
#🏷️R-SampleTag2[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  let successCount = 0;      // 成功添加对象标签的条目
  let skipCount = 0;         // 跳过的条目
  let noContentCount = 0;    // 无摘要也无PDF的条目
  let failCount = 0;         // AI调用或解析失败的条目
  let errorCount = 0;        // 标签保存失败的条目
  
  // 删除现有Sample/标签
  const removeExistingSampleTags = async (item) => {
    const existingTags = item.getTags();
    const sampleTags = existingTags.filter(tag => tag.tag && tag.tag.startsWith("Sample/"));
    for (let sampleTag of sampleTags) {
      item.removeTag(sampleTag.tag);
    }
  };
  
  for (let item of items) {
    try {
      // 删除原有Sample/标签
      await removeExistingSampleTags(item);
      
      let content = "";
      // 内容获取 - 优先PDF附件前25000字符,备选摘要
      try {
        // 首先尝试获取PDF内容
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text) {
              // 优先使用PDF前25000字符,包含更丰富的方法学信息
              content = fullTextData.text.substring(0, 25000).trim();
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
        try {
          item.addTag('Sample/无内容');
          await item.saveTx();
          noContentCount++;
        } catch {
          errorCount++;
        }
        continue;
      }
      // AI分析生成研究对象标签
      try {
        const prompt = `分析以下学术文献内容,识别研究对象的三个维度:
${content}
要求:
1. 第一个标签:研究层次 - 必须从以下选择:
   - 宏观-国家:国家层面的研究
   - 宏观-地区:地区/区域层面的研究
   - 宏观-行业:整个行业层面的研究
   - 宏观-政策:政策层面的研究
   - 中观-企业:单个企业/组织层面的研究
   - 中观-部门:企业内部门/组织单元层面的研究
   - 中观-网络:企业间网络/联盟层面的研究
   - 微观-员工:员工个体层面的研究
   - 微观-高管:高管个体层面的研究
   - 微观-消费者:消费者个体层面的研究
1. 第二个标签:文献研究领域 - 具体的学科领域或研究方向,尽可能使用以下示例,使用中文-8个字符
   例如:AI与创新,GPT 与创新,GPT 研究,管理学、心理学、经济学、金融学、市场营销、人力资源、战略管理、组织行为,信息系统,运筹学,运营管理,技术创新,管理科学,社会学,认知科学,行为经济学,神经科学,统计学,计算机科学,商业分析,大数据管理,商业智能,工业工程学,系统工程学, 创新管理,可持续发展学,生态环境学,计量经济学
2. 第三个标签:研究样本 - 研究采用的具体样本对象与规模,尽可能采用年份+规模+具体样本名称,如果限制在20个中文字符以内
   例如:2000-2010年60家英国制造业公司、2000-2010年60家中国上市公司、0位美国MBA学生、1990-2023年德国汽车企业、2000-2019年5000名日本银行高管、2010-2025年6000名印度IT员工等
   如果文中有更具体的样本描述(如具体公司名称、特定年份、特定规模等),请采用更具体的表述
请直接返回JSON数组,格式:["层次标签", "文献领域", "研究样本"]`;
        const response = await Meet.Global.views.ask(prompt);
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            failCount++;
            continue;
          }
          const tags = JSON.parse(jsonMatch[0]);
          if (!tags || tags.length !== 3) {
            failCount++;
            continue;
          }
          // 验证和清理标签
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim());
          if (validTags.length !== 3) {
            failCount++;
            continue;
          }
          // 验证第一个标签必须是标准的层次标签
          const validLevels = [
            '宏观-国家', '宏观-地区', '宏观-行业', '宏观-政策',
            '中观-企业', '中观-部门', '中观-网络',
            '微观-员工', '微观-高管', '微观-消费者'
          ];
          if (!validLevels.includes(validTags[0])) {
            failCount++;
            continue;
          }
          // 添加标签 - 明确标签含义
          item.addTag('Sample/1' + validTags[0]);
          item.addTag('Sample/2领域-' + validTags[1]);
          item.addTag('Sample/3样本-' + validTags[2]);
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
  return `研究对象标签重新处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```
