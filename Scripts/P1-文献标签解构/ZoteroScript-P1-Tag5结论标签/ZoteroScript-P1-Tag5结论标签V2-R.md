---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag5结论标签V2-R
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
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P1-Tag5 结论标签 V2-R

```javascript
#🏷️R-ResultTag2[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;      // 成功添加结论标签的条目
  let skipCount = 0;         // 跳过的条目
  let noContentCount = 0;    // 无内容的条目
  let failCount = 0;         // AI调用或解析失败的条目
  let errorCount = 0;        // 标签保存失败的条目
  
  // 删除现有Result/标签
  const removeExistingResultTags = async (item) => {
    const existingTags = item.getTags();
    const resultTags = existingTags.filter(tag => tag.tag && tag.tag.startsWith("Result/"));
    for (let resultTag of resultTags) {
      item.removeTag(resultTag.tag);
    }
  };
  
  for (let item of items) {
    try {
      // 删除原有Result/标签
      await removeExistingResultTags(item);
      
      let content = "";
      
      // 内容获取 - 优先PDF前3000字符，备选摘要
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
        
        // 如果PDF获取失败，再使用摘要
        if (!content) {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
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
        const prompt = `分析以下学术文献内容，提取其主要研究结论和发现，生成2-5个结论标签：
${content}
要求：
1. 每个标签应简洁精确，优先限制在15个中文字符以内，如果能完整表达结论可扩展到20个字符
2. 使用简体中文
3. 重点关注：
   - 主要研究发现和核心结论（第一优先）
   - 实证结果必须明确表述关系方向：正向、负向、倒U型、U型等
   - 调节效应必须明确：正向调节、负向调节
   - 中介作用必须明确表述
   - 理论贡献和实践启示（最后考虑）
4. 避免研究过程描述，专注于结果和结论
5. 使用具体、明确的表述，避免模糊词汇
6. 如果文献构建了理论模型、开发了测量工具、提出了新方法，应精准反映这些核心成果
7. 结论优先，如果字数允许则完整表达，字数不够时可使用变量名词简洁表达
8. 按重要程度排序，第1个结论最重要，后续依次递减
9. 根据文献实际内容确定结论数量（2-5个），简单研究2-3个，复杂研究4-5个
请直接返回JSON数组，格式：["结论1", "结论2", "结论3", "结论4", "结论5"]
如果少于5个结论，只返回实际的结论数量即可，例如：["结论1", "结论2"]`;
        const response = await Meet.Global.views.ask(prompt);
        
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            failCount++;
            continue;
          }
          
          const tags = JSON.parse(jsonMatch[0]);
          if (!tags || tags.length === 0 || tags.length < 2) {
            failCount++;
            continue;
          }
          
          // 验证和清理标签
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim())
            .slice(0, 5); // 确保最多5个标签
          
          if (validTags.length < 2) {
            failCount++;
            continue;
          }
          
          // 添加标签 - 按序号添加
          for (let i = 0; i < validTags.length; i++) {
            item.addTag('Result/' + (i + 1) + validTags[i]);
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
  
  return `研究结论标签重新处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```
