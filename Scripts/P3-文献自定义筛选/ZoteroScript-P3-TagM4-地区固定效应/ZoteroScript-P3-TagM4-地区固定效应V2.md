---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P3-TagM4-地区固定效应V2
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: [JavaScript, V2版本, Zotero, 代码, 固定效应, 地区标签, 学术研究, 实证方法, 年份标签, 智能清理, 标签体系, 标签维护, 行业标签, 面板数据]
RelatedNote:
CardRecord:
---


## ZoteroScript-P3-TagM4- 地区固定效应 V2

```javascript
#🏷️FE[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processCount = 0;
  let skipCount = 0;
  let errorCount = 0;
  let addedTags = {
    location: 0,
    year: 0,
    industry: 0
  };
  let removedTags = {
    location: 0,
    year: 0,
    industry: 0
  };
  
  // 定义固定效应类型映射
  const feMapping = [
    {
      type: "地区固定效应",
      keywords: ["省份", "城市", "地区"],
      tag: "/@省份/FE",
      key: "location"
    },
    {
      type: "年份固定效应",
      keywords: ["年份", "年度"],
      tag: "/@年份/FE",
      key: "year"
    },
    {
      type: "行业固定效应",
      keywords: ["行业", "产业"],
      tag: "/@行业/FE",
      key: "industry"
    }
  ];
  
  for (let item of items) {
    try {
      // 获取条目的所有标签
      const existingTags = item.getTags();
      
      // 筛选A2-IV/和A6-CV/开头的标签
      const targetTags = existingTags.filter(tag => 
        tag.tag && (tag.tag.startsWith("A2-IV/") || tag.tag.startsWith("A6-CV/"))
      );
      
      let hasModified = false;
      
      // 遍历每种固定效应类型
      for (let feType of feMapping) {
        // 检查目标标签中是否包含该类型的关键词
        const hasKeyword = targetTags.length > 0 && targetTags.some(tag => 
          feType.keywords.some(keyword => tag.tag.includes(keyword))
        );
        
        // 检查FE标签是否已存在
        const hasFETag = existingTags.some(tag => tag.tag === feType.tag);
        
        if (hasKeyword) {
          // 有关键词，应该添加FE标签
          if (!hasFETag) {
            item.addTag(feType.tag);
            hasModified = true;
            addedTags[feType.key]++;
          }
        } else {
          // 无关键词，应该删除FE标签（如果存在）
          if (hasFETag) {
            item.removeTag(feType.tag);
            hasModified = true;
            removedTags[feType.key]++;
          }
        }
      }
      
      // 如果有修改则保存
      if (hasModified) {
        try {
          await item.saveTx();
          processCount++;
        } catch {
          errorCount++;
        }
      } else {
        skipCount++;
      }
      
    } catch (e) {
      errorCount++;
    }
  }
  
  // 构建统计信息
  let stats = `固定效应标签处理完成（V2 智能清理版）\n`;
  stats += `\n【处理统计】`;
  stats += `\n- 处理成功: ${processCount}`;
  stats += `\n- 无变化: ${skipCount}`;
  stats += `\n- 错误: ${errorCount}`;
  
  stats += `\n\n【添加标签统计】`;
  stats += `\n- /@省份/FE: ${addedTags.location}`;
  stats += `\n- /@年份/FE: ${addedTags.year}`;
  stats += `\n- /@行业/FE: ${addedTags.industry}`;
  
  stats += `\n\n【删除标签统计】`;
  stats += `\n- /@省份/FE: ${removedTags.location}`;
  stats += `\n- /@年份/FE: ${removedTags.year}`;
  stats += `\n- /@行业/FE: ${removedTags.industry}`;
  
  return stats;
})()
}$
```
