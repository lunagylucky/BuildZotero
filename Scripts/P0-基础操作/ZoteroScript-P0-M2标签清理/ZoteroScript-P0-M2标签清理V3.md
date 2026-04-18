---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P0-M2标签清理V3
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
- 保护性设计
- 标签清理
- 代码
- 规则驱动
- 九脚本生态
- 批量处理
- 智能过滤
- 自动化工具
- JavaScript
- Zotero
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P0-M2 标签清理 V3

### 第一部分：完整代码

```javascript
#🏷️ClearTags[color=#ff6b6b][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processedItemsCount = 0;    // 处理的条目数
  let totalRemovedTags = 0;       // 删除的标签总数
  let errorCount = 0;             // 处理失败的条目数
  
  // 定义允许保留的标签前缀
  const allowedPrefixes = [
    "/", 
    "Item/", 
    "sMeth/",           // 研究方法标签
    "Sample/", 
    "Result/", 
    "Theory/",
    "Scholar/",         // 学者标签
    "A1-DV/",           // 因变量标签
    "A2-IV/",           // 自变量标签
    "A3-MO/",           // 调节变量标签
    "A4-ME/",           // 中介变量标签
    "A5-INV/",          // 工具变量标签
    "A6-CV/"            // 控制变量标签
  ];
  
  for (let item of items) {
    try {
      const existingTags = item.getTags();
      let removedTagsCount = 0;
      let hasChanges = false;
      
      // 检查每个标签
      for (let tagObj of existingTags) {
        const tagName = tagObj.tag;
        
        // 检查标签是否以允许的前缀开头
        const isAllowedPrefix = allowedPrefixes.some(prefix => tagName.startsWith(prefix));
        
        // 检查标签是否以字母或数字开头（需要删除的）
        const startsWithAlphanumeric = /^[a-zA-Z0-9]/.test(tagName);
        
        // 只删除以字母或数字开头且不符合允许前缀的标签
        if (startsWithAlphanumeric && !isAllowedPrefix) {
          // 删除不符合要求的标签
          try {
            item.removeTag(tagName);
            removedTagsCount++;
            hasChanges = true;
          } catch {
            // 单个标签删除失败不影响其他标签
          }
        }
      }
      
      // 如果有变更，保存条目
      if (hasChanges) {
        try {
          await item.saveTx();
          processedItemsCount++;
          totalRemovedTags += removedTagsCount;
        } catch {
          errorCount++;
        }
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `标签清理完成 - 处理条目: ${processedItemsCount}, 删除标签: ${totalRemovedTags}, 失败条目: ${errorCount}`;
})()
}$
```
