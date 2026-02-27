---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag1主题标签V4
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: [AI, JavaScript, Zotero, 七脚本生态, 主题标签, 代码, 内容分析, 学术研究, 文献分析, 智能标记, 标签体系, 知识管理]
RelatedNote:
CardRecord:
---


## ZoteroScript-P1-Tag1 主题标签 V4

```javascript
#🏷️ItemTag1[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
  for (let item of items) {
    try {
      // 跳过已处理的条目
      const existingTags = item.getTags();
      if (existingTags.some(tag => tag.tag && tag.tag.startsWith("Item/"))) {
        skipCount++;
        continue;
      }
      
      let content = "";
      
      // 内容获取 - 优先PDF前3000字符,备选摘要
      try {
        // 首先尝试获取PDF内容
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text) {
              // 优先使用PDF前3000字符
              content = fullTextData.text.substring(0, 3000).trim();
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
          item.addTag('Item/无内容');
          await item.saveTx();
          noContentCount++;
        } catch {
          errorCount++;
        }
        continue;
      }
      
      // AI分析生成主题标签
      try {
        const prompt = `分析以下学术文献内容，生成3个最能代表其核心内容的标签,第一优先级是变量名称，第二优先级是关键词：
${content}
要求：
1. 标签应简洁精确，3-6个字符
2. 使用简体中文
3. 重点关注：核心变量、核心概念为主、关键词是第二等级，关系结论是最后考虑
4. 避免研究方法、理论名称、研究对象（这些有单独标签），研究结论重合
5. 避免过于宽泛的词汇，要具体明确
6. 按照相关程度排序返回，第1个标签最相关，第3个标签相关度最低
请直接返回JSON数组格式：["标签1", "标签2", "标签3"]`;
        const response = await Meet.Global.views.ask(prompt);
        
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            failCount++;
            continue;
          }
          
          const tags = JSON.parse(jsonMatch[0]);
          if (!tags || tags.length === 0) {
            failCount++;
            continue;
          }
          
          // 验证和清理标签
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim())
            .slice(0, 3);
          
          if (validTags.length === 0) {
            failCount++;
            continue;
          }
          
          // 添加标签 - 按序号添加
          item.addTag('Item/1' + validTags[0]);
          if (validTags.length > 1) item.addTag('Item/2' + validTags[1]);
          if (validTags.length > 2) item.addTag('Item/3' + validTags[2]);
          
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
  
  return `主题标签处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$

```
