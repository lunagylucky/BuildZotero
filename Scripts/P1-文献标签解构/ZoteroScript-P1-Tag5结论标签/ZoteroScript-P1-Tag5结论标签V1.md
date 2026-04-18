---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag5结论标签V1
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
- 代码
- 结论提取
- 文献分析
- 学术研究
- 知识管理
- 自动化标签
- AI
- JavaScript
- Zotero
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P1-Tag5 结论标签 V1

```javascript
#🏷️ResultTags[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  let successCount = 0;      // 成功添加结论标签的条目
  let skipCount = 0;         // 已有Result标签被跳过的条目
  let noContentCount = 0;    // 无内容的条目
  let failCount = 0;         // AI调用或解析失败的条目
  let errorCount = 0;        // 标签保存失败的条目
  for (let item of items) {
    try {
      // 跳过已处理的条目
      const existingTags = item.getTags();
      if (existingTags.some(tag => tag.tag && tag.tag.startsWith("Result/"))) {
        skipCount++;
        continue;
      }
      let content = "";
      // 内容获取 - 优先摘要，备选PDF前3000字符
      try {
        // 首先尝试获取摘要
        const abstract = item.getField("abstractNote");
        if (abstract && abstract.trim().length > 50) {
          content = abstract.trim();
        } else {
          // 如果摘要获取失败，再使用PDF内容
          try {
            const pdfItem = await item.getBestAttachment();
            if (pdfItem && pdfItem.isPDFAttachment()) {
              const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
              if (fullTextData && fullTextData.text) {
                // 使用PDF前3000字符作为备选
                content = fullTextData.text.substring(0, 3000).trim();
              }
            }
          } catch {}
        }
      } catch {}
      if (!content) {
        // 无内容，标注便于识别
        try {
          item.addTag('Result/无内容');
          await item.saveTx();
          noContentCount++;
        } catch {
          errorCount++;
        }
        continue;
      }
      // AI分析生成结论标签
      try {
        const prompt = `分析以下学术文献内容，提取其主要研究结论和发现，生成3个结论标签：
${content}
要求：
1. 每个标签应简洁精确，限制在15个中文字符以内
2. 使用简体中文
3. 重点关注：
   - 主要研究发现和结论
   - 实证结果和统计发现
   - 理论贡献和实践启示
   - 因果关系和影响效应
   - 研究验证的假设结果
4. 避免研究过程描述，专注于结果和结论
5. 使用具体、明确的表述，避免模糊词汇
6. 优先提取定量结果、显著性发现、关键洞察
请直接返回JSON数组，格式：["结论1", "结论2", "结论3"]`;
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
            .slice(0, 3); // 确保最多3个标签
          if (validTags.length === 0) {
            failCount++;
            continue;
          }
          // 添加标签
          for (let tag of validTags) {
            item.addTag('Result/' + tag);
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
  return `研究结论标签处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```
