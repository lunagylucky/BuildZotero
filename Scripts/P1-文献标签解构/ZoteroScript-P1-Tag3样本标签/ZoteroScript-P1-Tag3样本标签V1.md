---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag3样本标签V1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:26
Type:
Status:
Version:
CardStatus:
CardType:
tags: []
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P1-Tag3 样本标签 V1

### 

```javascript
#Sample Tags[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;      // 成功添加对象标签的条目
  let skipCount = 0;         // 已有Sample标签被跳过的条目
  let noContentCount = 0;    // 无摘要也无PDF的条目
  let failCount = 0;         // AI调用或解析失败的条目
  let errorCount = 0;        // 标签保存失败的条目
  
  for (let item of items) {
    try {
      // 跳过已处理的条目
      const existingTags = item.getTags();
      if (existingTags.some(tag => tag.tag && tag.tag.startsWith("Sample/"))) {
        skipCount++;
        continue;
      }
      
      let content = "";
      
      // 内容获取 - 优先摘要，备选PDF
      try {
        const abstract = item.getField("abstractNote");
        if (abstract && abstract.trim().length > 50) {
          content = abstract.trim();
        } else {
          try {
            const pdfItem = await item.getBestAttachment();
            if (pdfItem && pdfItem.isPDFAttachment()) {
              const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
              if (fullTextData && fullTextData.text) {
                // 取前3000字符，包含研究设计和样本信息
                content = fullTextData.text.substring(0, 3000).trim();
              }
            }
          } catch {}
        }
      } catch {}
      
      if (!content) {
        // 无内容，标注便于识别
        try {
          item.addTag('Sample/无摘要');
          await item.saveTx();
          noContentCount++;
        } catch {
          errorCount++;
        }
        continue;
      }
      
      // AI分析生成研究对象标签
      try {
        const prompt = `分析以下学术文献内容，识别研究对象的三个维度：

${content}

要求：
1. 第一个标签：研究层次 - 必须从以下选择：
   - 宏观-国家：国家层面的研究
   - 宏观-地区：地区/区域层面的研究
   - 宏观-行业：整个行业层面的研究
   - 宏观-政策：政策层面的研究
   - 中观-企业：单个企业/组织层面的研究
   - 中观-部门：企业内部门/组织单元层面的研究
   - 中观-网络：企业间网络/联盟层面的研究
   - 微观-员工：员工个体层面的研究
   - 微观-高管：高管个体层面的研究
   - 微观-消费者：消费者个体层面的研究
2. 第二个标签：研究领域/行业 - 具体的行业或领域名称，使用中文，3-8个字符
3. 第三个标签：具体样本/对象 - 具体描述研究对象，限制在15个中文字符以内
4. 全部使用简体中文

请直接返回JSON数组，格式：["层次标签", "研究领域", "具体对象"]`;

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
          
          // 添加标签 - 统一化格式
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
  
  return `研究对象标签处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```
